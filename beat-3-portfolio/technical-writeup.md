# Hardening Dockerfile Parsing: Enforcing Strict Syntax in wheelgap

## Overview

In this project, I identified and resolved a container parser inconsistency in **wheelgap**, an open-source Python tool that detects missing prebuilt wheels for target platforms before Docker builds fail. The parser in `src/wheelgap/dockerfile.py` was previously more lenient than Docker itself, quietly accepting space-separated flags like `FROM --platform linux/arm64`, ignoring unknown flags like `FROM --foo=bar`, and failing to catch missing stage aliases in lines like `FROM python:3.13-alpine AS`. Using a combination of automated scanning tools and local verification via my ship-gate pre-commit harness, I reproduced the lenient parsing behavior, refactored `_split_from()` to strictly require key-value syntax and exact token counts, and expanded the project test suite with parametrized rejection cases. My pull request was reviewed by maintainer Patrick Ballard, who validated that our token-count rules matched Docker's underlying BuildKit parser, and it was merged into the upstream repository in [PR #88](https://github.com/wheelgap/wheelgap/pull/88).

## What happened

During my open-source exploration, I used an issue-scout tool to search for active Python repositories, and I discovered issue #82 on `wheelgap/wheelgap` titled "--dockerfile: reject FROM lines Docker rejects (flag syntax, missing stage name)". The core design principle for wheelgap's `--dockerfile` option is to extract the exact base image and target platform that Docker would resolve, or immediately raise a `DockerfileError` so that developers discover broken Dockerfiles before initiating slow builds.

To verify the problem before writing any code, I stood up a clean environment with Python 3.13 and `uv`, and I wrote a direct reproduction script testing `_split_from()` against invalid inputs. The function silently parsed the malformed lines and returned extracted tuples instead of raising exceptions:

```text
INPUT: FROM --platform linux/arm64 python:3.13
  -> RETURNED: ('python:3.13', 'linux/arm64', None) (BUG: should have raised DockerfileError)
INPUT: FROM --foo=bar python:3.13
  -> RETURNED: ('python:3.13', None, None) (BUG: should have raised DockerfileError)
INPUT: FROM python:3.13-alpine AS
  -> RETURNED: ('python:3.13-alpine', None, None) (BUG: should have raised DockerfileError)
INPUT: FROM python:3.13 extra args
  -> RETURNED: ('python:3.13', None, None) (BUG: should have raised DockerfileError)
```

I modified `_split_from()` in `src/wheelgap/dockerfile.py` to enforce that any leading flag must explicitly start with `--platform=`, raising a `DockerfileError` for any unknown flag or invalid syntax. Next, I updated the token validation to require that words following the flags must have a length of either 1 (an image name alone) or 3 (where the second token must be `AS` case-insensitively, followed by the stage alias), and any other word count raises a `DockerfileError`.

Here is the core diff implementing the validation in `src/wheelgap/dockerfile.py`:

```python
<<<<
     while words and words[0].startswith("--"):
         flag = words.pop(0)
         if flag.startswith("--platform="):
             platform = flag.split("=", 1)[1]
         elif flag == "--platform" and words:
             platform = words.pop(0)
     if not words:
         raise DockerfileError(f"FROM {args}: no image")
     stage = words[2].lower() if len(words) >= 3 and words[1].upper() == "AS" else None
     return words[0], platform, stage
====
     while words and words[0].startswith("--"):
         flag = words.pop(0)
         if flag.startswith("--platform=") and len(flag) > len("--platform="):
             platform = flag.split("=", 1)[1]
         else:
             raise DockerfileError(f"FROM {args}: unknown flag {flag!r}")
     if not words:
         raise DockerfileError(f"FROM {args}: no image")
     if len(words) == 1:
         return words[0], platform, None
     if len(words) == 3 and words[1].upper() == "AS":
         return words[0], platform, words[2].lower()
     raise DockerfileError(f"FROM {args}: invalid FROM syntax")
>>>>
```

When I ran the full test suite with `uv run --group dev pytest`, I hit an unexpected failure. It turned out that an existing test in `tests/test_dockerfile.py` had actually codified the old bug by asserting that `--platform linux/arm64 python:3.13-alpine` succeeded. This was a critical moment where I had to change my plan and remove that obsolete test case, migrating the check into `test_dockerfiles_docker_rejects` alongside seven new parametrized failure cases covering unknown flags, missing stage names, extra words, and empty values.

After running my pre-commit ship gate and verifying that all 317 unit tests, the `ruff` linter, and `mypy` typechecker passed cleanly, I committed my changes (commit `b189bbe`) and opened pull request #88. Maintainer Patrick Ballard reviewed the PR and verified that the change aligned with Docker's internal Go parser:

> "Thanks @Vansh300901, this is a nice tight change, and every new reject case really does fail on main. The word-count rule (1 word, or 3 with `AS`) and the case-sensitive flag name match BuildKit exactly. Checked against BuildKit's own parser (`instructions/parse.go`, `bflag.go`, `parser/split_command.go`)..."

The pull request landed in production via merge commit `2ba7174`, which credited me as co-author on the commit and added my attribution to the release notes in `CHANGELOG.md`.

## Limitations and what's next

While this patch firmly closes the gap for flag syntax and stage alias word counts, Dockerfile parsing in the wild contains additional edge cases. In the review feedback, the maintainer pointed out two related limits that were scoped out to avoid overcomplicating our PR:

1. Stage name character validation: Docker disallows stage names starting with numbers or containing special symbols, such as `FROM python:3.13 AS 1build`.
2. Quoted flag parameters: Docker allows quotes around flag values, such as `FROM --platform="linux/arm64" python:3.13`, whereas `_split_from` currently expects unquoted arguments.
3. Empty default values: As Docker's BuildKit treats `--platform=` with an empty string as a signal to use the host default platform, future iterations can allow the empty string while maintaining rejection of unknown flags.

For my next step, I plan to investigate BuildKit's `instructions/parse.go` tokenizer to draft a dedicated pull request addressing quoted flags and stage naming validation, ensuring wheelgap remains an accurate mirror of container build engines.

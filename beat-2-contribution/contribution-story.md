# Contribution: --dockerfile: reject FROM lines Docker rejects (flag syntax, missing stage name)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in the repository is not read. **Keep every section header below under its exact name** — the format criteria grade exactly that. Your course repo link is the whole submission: no tool files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Vansh
**GitHub Username:** Vansh300901
**Issue:** https://github.com/wheelgap/wheelgap/issues/82
**Pull Request:** https://github.com/wheelgap/wheelgap/pull/88

One repository, built across units 5 to 8, telling the whole story of your real contribution. Fill it as you work, not at the end — the sections below ask for evidence that is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull Request, ends with a Toolkit block: which tool did that phase's work and how you used it, what it produced, what difficulties or shortcomings it presented, and what you changed or would change because of them. Together the blocks are the thread the rubric's Toolkit points read, and they are the raw material for your unit-9 portfolio, so keep them current as you work rather than reconstructing them later.

A Toolkit block looks like this (copy it into each story section and fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work, starting below: what you built or changed that week, what broke, and what you changed because of it. The blocks tell the story where it happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each one in the week it belongs to.

**Unit 5: issue-scout scanner and wild repository ranking**

I built the issue-scout overlay on top of my unit 1 selection tool, and I configured a search strategy with targeted GitHub CLI queries for good first issues in active Python repositories. The scan surfaced several candidates across multiple repos, but several initial candidates either lacked reproduction specifics or touched TypeScript codebases, so I updated my ranking rules to prioritize standalone Python bugs with explicit test references like wheelgap#82.

**Unit 6: canonical voice guard hook and scoped repository memory**

I installed the voice guard hook to check all outgoing pull request comments and claim text against my voice guide, and I set up CLAUDE.local.md to capture distilled architecture notes for the wheelgap dockerfile parser subsystem. The initial automated hook check failed when my local session credentials expired, so I used the manual checker on my draft to confirm that my intent and evidence rules passed cleanly before posting my claim upstream.

**Unit 7: ship-gate pre-commit verification and manifest orchestration**

I configured the ship-gate hook and created ship-manifest.md in my local wheelgap clone, and I mapped out the project commands for ruff linting, mypy typechecking, and pytest test execution. Running pytest initially failed because editable installation paths were missing in my local environment, so I configured the uv toolchain and verified that all 317 tests and lint checks passed before creating the pull request.

**Unit 8: pull request monitoring and automated playbook drafting**

I installed the PR monitor and authored a playbook specifying response rules, a seven-day staleness threshold, polite nudge conventions, and explicit walk-away criteria. The monitor allowed me to track GitHub Actions check runs and confirm mergeability for my branch, which gave me confidence that the pull request was clean and ready for maintainer review.

---

## Why I Chose This Issue

I searched across active public repositories using GitHub CLI queries targeted at open issues labeled with good first issue and bug in Python projects, and my scan surfaced five candidate issues across three different repositories. The scout graded wheelgap#82 with an accept verdict because the repository maintainer was actively committing, the issue had zero competing assignees and zero comments, the project CONTRIBUTING.md explicitly welcomed AI-assisted contributions, and the issue body provided clear and self-contained reproduction cases in a single module. This issue fits my profile well because I enjoy working on Python backend tools and CLI utilities, and I wanted an opportunity to understand how production parsers handle container specifications and strict validation rules.

When evaluating the tool on wild searches, the initial query surfaced some issues where the repository language was marked as Python, but the actual subsystem needing modification was written in TypeScript or React Native, which showed me that language filters at the repository level can be misleading. I adjusted my ranking rules to inspect the file paths mentioned in the issue body before selecting a target, and seeing the exact function `_split_from` in `src/wheelgap/dockerfile.py` named directly in wheelgap#82 confirmed that the scout had found a feasible and well-scoped bug.

Claim comment link: https://github.com/wheelgap/wheelgap/issues/82#issuecomment-6734978064

**Toolkit**

- Tool: issue-scout with custom search strategy and wild repository rubric
- What it produced: Ranked scan output accepting wheelgap#82 with pass grades across maintainer activity, alive status, availability, AI policy, and feasible scope
- Difficulties: Initial GitHub queries returned cross-language repositories where the issue was actually located in frontend JavaScript rather than Python
- What changed: Added ranking rules requiring explicit Python file paths in the issue description and prioritizing issues with named functions

---

## Understanding the Issue

**Problem Description**

The `_split_from()` function in `src/wheelgap/dockerfile.py` parses `FROM` instructions in Dockerfiles to extract the base image, target platform, and build stage name, but it is currently more forgiving than Docker itself. Docker requires that flag options like `--platform` use key-value syntax with an equals sign, Docker rejects unknown flags completely, and Docker requires valid stage declarations with an image and an optional `AS <stage>` name, but wheelgap currently parses these invalid instructions without raising an error.

**Expected Behavior**

wheelgap should enforce the project rule of matching Docker behavior or raising an error, so any Dockerfile with invalid flag syntax, unknown flags, missing stage names after `AS`, or unexpected word counts should immediately raise a `DockerfileError` to alert users that their Dockerfile is invalid.

**Current Behavior**

When given `FROM --platform linux/arm64 python:3.13`, wheelgap quietly accepts the space-separated platform flag and extracts `('python:3.13', 'linux/arm64', None)`, and when given `FROM --foo=bar python:3.13`, it ignores the unknown flag and returns `('python:3.13', None, None)`, and when given `FROM python:3.13-alpine AS`, it returns `('python:3.13-alpine', None, None)` without checking for the missing stage name.

**Affected Components**

The affected component is `src/wheelgap/dockerfile.py`, specifically the `_split_from()` helper function called by `parse_from()` and `final_image()`, along with unit tests in `tests/test_dockerfile.py`.

**Toolkit**

- Tool: Scoped orientation via CLAUDE.local.md and source code inspection
- What it produced: Pinpointed `_split_from()` at line 122 of `src/wheelgap/dockerfile.py` as the sole parser function responsible for splitting flags, image tokens, and stage aliases
- Difficulties: Tracing where `_split_from()` was called to make sure raising `DockerfileError` would not cause unexpected crashes in other parsing paths
- What changed: Verified all call sites in `parse_from()` and `final_image()` already expect `DockerfileError` and propagate it cleanly

---

## Reproduction Process

**Environment Setup**

Reproduction was conducted on macOS Darwin arm64 with Python 3.13.0, uv 0.13.7, and pytest 9.2.0. I cloned the wheelgap repository, created an isolated virtual environment with uv, and ran the test suite to establish a clean baseline. The project dependencies installed without issues, and running the test suite confirmed that 311 unit tests passed out of the box.

**Steps to Reproduce**

1. Run an interactive Python command that imports `_split_from` from `wheelgap.dockerfile`:
   `python3 -c "from wheelgap.dockerfile import _split_from; print(_split_from('--platform linux/arm64 python:3.13'))"`
2. Run the same function against unknown flags:
   `python3 -c "from wheelgap.dockerfile import _split_from; print(_split_from('--foo=bar python:3.13'))"`
3. Run the function against missing stage names:
   `python3 -c "from wheelgap.dockerfile import _split_from; print(_split_from('python:3.13-alpine AS'))"`
4. Observed result: All three commands successfully returned tuples instead of raising `DockerfileError`, confirming that invalid Docker syntax was silently accepted.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/Vansh300901/wheelgap/commit/b189bbe504b5e568a93ff47746d5ed8856f10efb
- **Screenshots/logs:**
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
- **My findings:** The reproduction verified that `_split_from()` used a lenient while loop that stripped any token starting with `--` and accepted `--platform` followed by a space, while stage extraction checked `words[1].upper() == 'AS'` but defaulted to `None` when fewer than three words were present instead of validating the token count.

**Toolkit**

- Tool: repro-check with python reproduction script
- What it produced: Precise log excerpt capturing the unexpected return tuples across all four invalid syntax variations
- Difficulties: Ensuring the script imported the local `src` directory cleanly rather than looking in system site-packages
- What changed: Invoked reproduction with `PYTHONPATH=src` to guarantee execution against local source files

---

## Solution Approach

**Analysis**

In `src/wheelgap/dockerfile.py`, `_split_from()` iterated over leading `--` tokens and permitted either `--platform=VALUE` or `--platform VALUE`, while dropping any other `--` flag silently without error. Furthermore, after consuming flags, the code checked if `len(words) >= 3 and words[1].upper() == 'AS'`, but if `words` only contained two items like `['python:3.13-alpine', 'AS']`, it simply returned `stage = None` rather than rejecting the malformed line.

**Proposed Solution**

The solution strictly restricts accepted flags in `_split_from()` to only flags starting with `--platform=` having a non-empty value, and raises `DockerfileError` for any other flag. After flags are consumed, the remaining word list must have a length of either 1 (representing an image without a stage name) or 3 (where the second word must be `AS` case-insensitively and the third word becomes the lowercase stage name), and any other word count raises `DockerfileError`. Deliberately, no changes are made to image name resolution, heredoc parsing, or downstream target computation.

**Implementation Plan**

1. Modify `_split_from` in `src/wheelgap/dockerfile.py` to raise `DockerfileError` for unknown flags, flags without values, or flags missing `=`.
2. Enforce strict word count validation in `_split_from` requiring either 1 word or 3 words with `AS`.
3. Update `tests/test_dockerfile.py` to remove the outdated test expecting `--platform linux/arm64` to succeed, and add parametrized rejection cases to `test_dockerfiles_docker_rejects`.
4. Add a user-facing entry in `CHANGELOG.md` under `## Unreleased`.

Mid-build deviation: During test execution, I discovered that `test_parse_from` in `tests/test_dockerfile.py` previously contained an explicit test case asserting that space-separated `--platform linux/arm64` returned a parsed target, so I removed that obsolete test case and added comprehensive rejection assertions to ensure the new strict behavior was thoroughly covered.

**Toolkit**

- Tool: plan-check with wild scope guidelines
- What it produced: Bounded four-step implementation plan focusing strictly on `_split_from` and covering tests
- Difficulties: Discovering the existing positive test case in `test_dockerfile.py` that asserted the buggy behavior
- What changed: Recorded the deviation to remove the old test case and migrate it to the rejection suite

---

## Testing Strategy

The testing strategy proves the fix through observable before and after execution checks on invalid Dockerfile inputs, followed by complete execution of the repository test suite, linter, and type checker.

Before the fix:
Running `_split_from('--platform linux/arm64 python:3.13')` returned `('python:3.13', 'linux/arm64', None)`, and running `_split_from('--foo=bar python:3.13')` returned `('python:3.13', None, None)`, and running `_split_from('python:3.13-alpine AS')` returned `('python:3.13-alpine', None, None)`.

After the fix:
Running pytest against the updated `test_dockerfiles_docker_rejects` confirms that each malformed Dockerfile raises `DockerfileError`:
- `FROM --platform linux/arm64 python:3.13` raises `DockerfileError: FROM --platform linux/arm64 python:3.13: unknown flag '--platform'`
- `FROM --foo=bar python:3.13` raises `DockerfileError: FROM --foo=bar python:3.13: unknown flag '--foo=bar'`
- `FROM python:3.13-alpine AS` raises `DockerfileError: FROM python:3.13-alpine AS: invalid FROM syntax`
- `FROM python:3.13 builder` raises `DockerfileError: FROM python:3.13 builder: invalid FROM syntax`
- `FROM python:3.13 AS a b` raises `DockerfileError: FROM python:3.13 AS a b: invalid FROM syntax`
- `FROM --platform= python:3.13` raises `DockerfileError: FROM --platform= python:3.13: unknown flag '--platform='`

Full repository verification:
- `uv run --group dev pytest` ran 318 test items, resulting in 317 passed and 1 skipped in 0.41 seconds.
- `uv run --group dev ruff check .` passed with all checks clean.
- `uv run --group dev mypy` passed with no issues found across 11 source files.
- `uv run --group dev ruff format --check .` confirmed all 28 files are cleanly formatted.

**Toolkit**

- Tool: ship-gate manifest runner
- What it produced: Complete green execution across pytest, ruff, and mypy confirming that zero regressions were introduced
- Difficulties: Setting up the local uv environment so that pytest found the editable source files automatically
- What changed: Installed wheelgap in editable mode into the local virtual environment and verified tests ran without PYTHONPATH overrides

---

## Implementation Notes

**2026-11-09**

I initiated the issue scan and selected wheelgap#82 based on its clean scope, active maintainer, and explicit AI policy in CONTRIBUTING.md. I composed my claim comment draft following my voice guide, verified that it adhered to my rules against promising deadlines or using em-dashes and semi-colons, and posted the claim comment on the upstream issue thread.

**2026-11-09**

I forked the upstream wheelgap repository to Vansh300901/wheelgap and cloned it to my local machine. I created the working branch `fix/82-dockerfile-from-syntax` and set up `CLAUDE.local.md` and `ship-manifest.md`, ensuring that both files were excluded via `.git/info/exclude` so they would not contaminate git commits.

**2026-11-09**

I reproduced the bug using an inline Python command and observed that `_split_from` allowed space-separated platform flags and missing stage names. I updated `_split_from` in `src/wheelgap/dockerfile.py` to enforce `--platform=VALUE` syntax and restrict word counts after flags to exactly 1 or 3 tokens. When running the test suite, I found that an existing test in `test_dockerfile.py` had asserted the old lenient behavior for space-separated flags, so I removed that obsolete test case and expanded `test_dockerfiles_docker_rejects` with seven new parametrized test cases.

**2026-11-09**

I added an entry to `CHANGELOG.md` under `## Unreleased` describing the stricter FROM parsing behavior. I ran the full test suite, linter, and type checker, confirmed that all 317 tests passed, committed the changes as commit `b189bbe`, pushed the branch to my fork, and opened pull request #88 against `wheelgap/wheelgap:main`.

**Toolkit**

- Tool: git, uv, and ship-gate logging
- What it produced: Clean commit history containing three modified files (`CHANGELOG.md`, `src/wheelgap/dockerfile.py`, `tests/test_dockerfile.py`)
- Difficulties: Balancing strict flag rejection while keeping valid multi-stage builds and flags functional
- What changed: Added checks for non-empty flag values and verified that existing valid flags like `--platform=linux/arm64` continue to parse properly

---

## Pull Request

**PR Link:** https://github.com/wheelgap/wheelgap/pull/88

**PR Description:**

Closes #82

### What this changes

Strictly validate FROM instruction syntax in `_split_from()` to align with Docker parser behavior:
- Reject flags without `=`, such as `FROM --platform linux/arm64 python:3.13`
- Reject unknown flags, such as `FROM --foo=bar python:3.13`
- Reject word counts other than 1 or 3 after flags, preventing missing stage names in `FROM python:3.13-alpine AS`
- Added parametrized test coverage in `tests/test_dockerfile.py` and updated `CHANGELOG.md` under `## Unreleased`

### Checklist

- [x] Tests added or updated (no network access: use `fake_pypi` or a fixture)
- [x] `uv run --group dev ruff format .` and `uv run --group dev ruff check .` pass
- [x] `uv run --group dev pytest` passes
- [x] `CHANGELOG.md` has a line under `## Unreleased` (if users would notice the change)
- [x] README updated (if there's a new flag, target or input format)

**Maintainer Feedback:**

- 2026-11-09: Pull request opened successfully. GitHub Actions workflow CI triggered on pull request event, awaiting initial maintainer review from @patrickballard.
- 2026-11-09: None yet, as the pull request has just landed and the maintainer reviews asynchronously.

**Status:** Awaiting review

**Toolkit**

- Tool: pr-monitor and pr-precheck
- What it produced: Verified pull request state, base and head branch tracking, and clean mergeability against upstream main
- Difficulties: GitHub Actions requires maintainer approval to run workflow jobs for first-time contributors from forks
- What changed: Recorded the pull request state and verified all local checks independently so the maintainer receives a tested branch

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how Docker specifications differ from naive line splitting, and I gained practical experience writing defensive parsing logic in Python that enforces strict key-value flag syntax and token count boundaries. I also gained deeper experience using `uv` to manage fast Python environments, running `ruff` and `mypy` against typed codebases, and structuring parametrized pytest fixtures that cleanly test failure modes and regex exception matching.

**Challenges Overcome**

A key challenge was discovering that an existing unit test had originally codified the bug by testing that space-separated `--platform linux/arm64` returned a valid target. Rather than leaving conflicting tests in place, I traced the issue description and Docker documentation, recognized that the test was testing obsolete behavior, and safely removed it while adding seven comprehensive rejection cases to `test_dockerfiles_docker_rejects`. Another challenge was configuring local environment tooling when OAuth tokens expired, which I resolved by running direct verification scripts and logging evidence systematically.

**What I'd Do Differently Next Time**

Next time I would inspect the test suite earlier in the reproduction phase to see if existing tests already touch the target function, because knowing upfront that a test asserted the old behavior would have saved time during implementation. I would also set up local virtual environment path configurations immediately after cloning to streamline test execution from the start.

---

## Resources Used

- wheelgap issue #82: https://github.com/wheelgap/wheelgap/issues/82
- wheelgap pull request #88: https://github.com/wheelgap/wheelgap/pull/88
- Upstream claim comment: https://github.com/wheelgap/wheelgap/issues/82#issuecomment-6734978064
- wheelgap CONTRIBUTING guide: https://github.com/wheelgap/wheelgap/blob/main/CONTRIBUTING.md
- Dockerfile reference documentation on FROM syntax: https://docs.docker.com/reference/dockerfile/#from

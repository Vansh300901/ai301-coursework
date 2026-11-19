# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Vansh

## Featured Tools

I chose these four tools because together they formed the complete pipeline that carried me from finding an open-source bug to getting a clean pull request merged upstream. Starting with issue discovery, moving through maintainer communication, pre-commit validation, and review monitoring, each tool took over a critical phase where manual judgment previously caused friction or missed edge cases.

**issue-scout**

**What it is, and the judgment hole I wrote:** An automated issue discovery and ranking tool that queries active GitHub repositories and evaluates candidate issues against a multi-factor suitability rubric. The judgment hole I wrote was the search strategy queries and the customized evaluation rubric, which prioritizes isolated Python bug fixes with explicit reproduction criteria and active maintainers while rejecting vague feature requests.

**How it evolved, dated:**
- 2026-09-23: Initial Unit 1 rubric evaluated issues based purely on effort tiers and estimated hours, but it often accepted issues that lacked concrete reproduction steps.
- 2026-11-09: Upgraded to issue-scout with wild repository scoring and patched the rubric with difficulty ratings, prioritizing issues that named exact module functions like `_split_from` in `src/wheelgap/dockerfile.py`.

**Evidence:**
```json
{
  "item": "https://github.com/wheelgap/wheelgap/issues/82",
  "checks": [
    {"name": "maintainer-active", "grade": "pass", "evidence": "Maintainer pushed commits on 2026-11-09"},
    {"name": "repo-alive", "grade": "pass", "evidence": "Repository is active and pushed within 24 hours"},
    {"name": "issue-available", "grade": "pass", "evidence": "0 assignees and 0 comments"},
    {"name": "ai-policy-permitted", "grade": "pass", "evidence": "CONTRIBUTING.md explicitly states 'Using AI tools is fine'"},
    {"name": "scope-feasible", "grade": "pass", "evidence": "Bounded bug fix targeting _split_from in src/wheelgap/dockerfile.py"}
  ],
  "verdict": "accept"
}
```

**voice-guard**

**What it is, and the judgment hole I wrote:** A pre-tool hook and verification tool that holds every outgoing public comment, issue claim, and pull request description against my personal voice guide. The judgment hole I wrote was the upstream communication rules in `voice-guide.md`, specifying wrong/right pairs for claims, questions, and pings, while enforcing my personal communication rules such as promising investigation rather than premature delivery dates and avoiding em-dashes and semicolons.

**How it evolved, dated:**
- 2026-09-28: Unit 2 initial voice guide checked drafts in sandbox issues, focusing primarily on citing reproduction test output before making conclusions.
- 2026-11-09: Promoted to the canonical `~/.claude/voice-guard/voice-guide.md` and added upstream rules for maintainer claims and pings, verifying that claim comments state immediate verification steps without promising fixed deadlines.

**Evidence:**
```text
Draft checked: "I would like to work on this issue as part of my coursework, and I have set up the local environment and confirmed the reproduction where _split_from() currently parses --platform linux/arm64 and --foo=bar without raising an error..."
Verdict:
- 2026-11-09 02:50 [manual] pass | target: claim_draft.md
Result: Posted to https://github.com/wheelgap/wheelgap/issues/82#issuecomment-6734978064
```

**ship-gate**

**What it is, and the judgment hole I wrote:** A pre-commit and pre-pull-request gate that executes the project's native test suites, linters, and typecheckers before allowing a pull request to open. The judgment hole I wrote was `ship-manifest.md`, which defines the mandatory checks (`uv run --group dev pytest`, `ruff check`, `mypy`) and logs dated exclusions with technical justifications.

**How it evolved, dated:**
- 2026-10-12: Unit 4 initial PR precheck verified PR templates and commit diffs in sandbox exercises.
- 2026-11-09: Upgraded to ship-gate with live repository manifests, running full test suites in clean environments and catching an obsolete test case in `tests/test_dockerfile.py` that had previously asserted the buggy lenient behavior.

**Evidence:**
```text
uv run --group dev pytest
======================== 317 passed, 1 skipped in 0.41s ========================
uv run --group dev ruff check .
All checks passed!
uv run --group dev mypy
Success: no issues found in 11 source files
```

**pr-monitor**

**What it is, and the judgment hole I wrote:** A pull request tracking tool that inspects GitHub Actions CI status, review decisions, and comment threads on open pull requests to recommend next actions without modifying code autonomously. The judgment hole I wrote was `playbook.md`, which defines response conventions, a 7-day staleness threshold, nudge etiquette, and walk-away criteria.

**How it evolved, dated:**
- 2026-11-09: Initial playbook created with a 7-day wait threshold and guidelines for addressing technical review feedback directly with code diffs.
- 2026-11-09: Used to monitor GitHub Actions and maintainer review on wheelgap PR #88, confirming mergeability and tracking maintainer Patrick Ballard's review until the pull request was merged into main.

**Evidence:**
```text
PR #88 (wheelgap/wheelgap)
State: merged
Merge commit: 2ba7174dc6221243bd281e69a7929452272811ac
Review note: "Thanks @Vansh300901, this is a nice tight change, and every new reject case really does fail on main. The word-count rule (1 word, or 3 with AS) and the case-sensitive flag name match BuildKit exactly."
```

## Inventory

- repro-check: Isolated reproduction script harness that runs test cases and logs exact terminal output before proposing fixes.
- plan-check: Structured planning tool that holds proposed file modifications and test strategies against scope boundaries before implementation begins.
- pr-precheck: Pre-submission verification tool that checks pull request checklists, changelog updates, and git commit cleanliness.

## The Arc

**Beat 1**
Date: September 2026
Friction: When searching for issues manually, I frequently encountered issues that seemed simple but were either already claimed by other contributors or lacked enough context to stand up a local reproduction.
Change: I created a structured evaluation rubric in issue-select that verified maintainer commit activity within thirty days, checked for zero competing assignees, and required concrete reproduction commands in the issue body.
Result: The tool filtered out stale or contentious issues and reliably surfaced high-quality candidate bugs with clear reproduction paths.

**Beat 2**
Date: October 2026
Friction: Early comment drafts risked slipping into common student habits like promising fixed delivery dates or proposing speculative refactors before verifying the error locally.
Change: I built a voice guide and integrated the voice-guard hook, creating strict rules that require stating local reproduction evidence first and forbidding delivery promises or informal punctuation habits.
Result: Outgoing claim comments and pull request descriptions remained consistently factual and professional, earning positive feedback from maintainers upstream.

**Beat 3**
Date: November 2026
Friction: During local implementation on wheelgap, running the test suite unexpectedly failed because an existing test in the repository had actually codified the bug by expecting invalid syntax to succeed.
Change: The ship-gate caught this failure during pre-commit checks, prompting me to remove the obsolete positive test and add seven comprehensive rejection test cases covering malformed flags and missing stage aliases.
Result: All 317 unit tests, ruff lint checks, and mypy type checks passed cleanly, ensuring zero regressions before the pull request was opened.

**Beat 4**
Date: November 2026
Friction: Waiting for maintainer reviews can cause uncertainty about CI status, merge conflicts, or when it is appropriate to follow up on a quiet thread.
Change: I established a playbook for pr-monitor defining a seven-day staleness threshold and structured response rules for answering technical review points directly.
Result: Maintainer Patrick Ballard reviewed PR #88, verified the alignment with Docker's internal BuildKit parser, and merged the pull request into main with co-author credit.

## Limitations and What's Next

The toolkit as a whole excels at verifying isolated Python backend bugs and guarding outgoing communication, but it still requires manual inspection when analyzing complex multi-language repositories where issue labels do not match the underlying file paths. Furthermore, while the ship-gate handles project-native test suites, it does not currently simulate end-to-end container builds or network-isolated integration environments. If I were to continue developing the toolkit, I would build an automated tokenizer inspector that can cross-reference Python parser rules directly against upstream Go specifications like BuildKit, and I would add automated multi-stage Docker build testing to verify parser edge cases in live containers.

**Tool limits.**

issue-scout: Can be misled by repository primary language tags when the actual bug lives in a different subsystem like frontend TypeScript or mobile JavaScript.
voice-guard: Only inspects outgoing GitHub CLI commands and cannot intercept comments posted directly through web browser interfaces without manual file checks.
ship-gate: Requires pre-existing project virtual environments and does not automatically install missing native system compilers or package managers.
pr-monitor: Only tracks PR state and drafts recommendations without automated git conflict rebase execution.

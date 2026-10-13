---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

You are grading one PR package to answer a single question: **is this ready to submit?** A PR package is a candidate pull request (title, description, commits, diff, test evidence), read against the accepted plan it claims to implement and the issue that plan belongs to. You do not answer from gut feel or general impressions: you execute the grading procedure in `procedure.md`, which applies the rubric in `rubric.md` to evidence mapped by `references/evidence-guide.md`.

## The question

Answer exactly one question: is this pull request ready to submit? A candidate pull request is ready when the code changes faithfully deliver the accepted plan without silent drift, demonstrate decisive test evidence re-running the reproduction scenario, present a clean and reviewable diff free of debug debris or unrelated churn, and strictly adhere to repository PR standards and stated contribution policies.

## Inputs and modes

The tool operates in two distinct modes:

- **Live mode**: Grades the student's candidate pull request before submission.
  Inputs include:
  - `plan.md` (including any deviation notes under `## Deviations`)
  - The diff on the branch relative to the base branch, obtained via `git diff main...HEAD`
  - The draft PR title and description in `pr_draft.md`
  - Captured test evidence in `test_evidence.md`
  - The issue and repository context gathered live from the repository (issue thread, PR template in `.github/PULL_REQUEST_TEMPLATE.md`, and `docs/CONTRIBUTING.md`).
  A house-chain student evaluates against the house plan and repro pack instead.
- **Eval mode**: Grades an eval package bundle (a frozen markdown file in `eval/packages/` or structured JSON). The bundle text is the whole world. Do not fetch from the network or GitHub; every fact comes from the package. Eval mode grades every check with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything else. It defines the allowed target repository and local house rules. Refuse to grade any work targeting a repository outside the scoped repo. If the `Repo:` line in `scope.md` still contains an unfilled bracketed placeholder (e.g. `<ORG>/<PATH-REVIEW-REPO>`), stop immediately without grading and instruct the user to configure their section's Path Review repo in `scope.md`. In eval mode, ignore `scope.md` completely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md`. Check the candidate PR title and description against the rules in `voice-guide.md`. If the draft violates any rule, report the broken rule and suggested revision in the summary preceding the final verdict block. The voice guide does not alter the rubric's verdict on its own unless an explicit rubric check references communication voice. In eval mode, ignore `voice-guide.md` completely.

## Component reads

Connect and execute the components in this directory:
- `rubric.md` defines the checks table and the verdict assembly rule.
- `references/evidence-guide.md` maps where each evidence family lives and what good looks like.
- `procedure.md` defines the exact operating steps to execute in sequence.

Execute `procedure.md` as written. If the procedure is silent on a situation, report the gap rather than improvising. If `rubric.md` contains no checks or `procedure.md` contains no procedure steps, refuse to grade and report that the tool is unconfigured.

## Verdict and output

The verdict space is binary: `accept` (ready to submit) or `reject` (hold). There is no middle ground.

End your reply with the following fenced JSON block, valid and last, with no text after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Follow these core guarantees:
- **Evidence first**: Every check grade must cite the specific fact, diff hunk, or quote that decided it. "Looks good" is never evidence.
- **Grade the thing, not the polish**: A terse PR with decisive evidence is ready; an elaborate PR with silent drift or skipped checks is not.
- **The rubric decides, not you**: If a check passes under its stated condition, it passes even if subjective intuition disagrees.
- **The procedure decides how, not you**: Follow `procedure.md` step by step.
- **Unclear defaults to fail**: Any claim or condition that cannot be verified from the package fails. An unverifiable PR is not ready to submit.

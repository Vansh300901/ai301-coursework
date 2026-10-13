# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- **Where it lives**:
  - In an eval bundle: compare the `Plan context` block (specifically the plan steps and scope boundaries) with the `Candidate PR`'s `Diff` and `Description`.
  - In live mode: compare `plan.md` (and any `## Deviations` section) with `git diff main...HEAD` and the draft description in `pr_draft.md`.
- **What good looks like**:
  - The diff modifies only files and logic specified in the accepted plan.
  - No unannounced additions: no new configuration settings, extra flags, or refactoring/renaming of unrelated functions.
  - The description claims only what the diff actually delivers (no claiming documentation or tests that were not included).
  - Honest shortfalls pass: if a feature or follow-up was explicitly deferred in both the plan's deviation notes and the PR description, it is not silent drift.

## Test evidence (harness category: not-tested)

- **Where it lives**:
  - In an eval bundle: check the `Test evidence` section under `Candidate PR` and compare against the `Repro evidence` and `Test plan` in `Plan context`.
  - In live mode: check `test_evidence.md` (and the `Testing` section of `pr_draft.md`) against the reproduction steps and the repo's verification commands (`make test-unit`, `make lint`, etc.).
- **What good looks like**:
  - Decisive, observable before-and-after output showing the failure before the fix and the resolution after the fix.
  - Exercises the specific broken code path reported in the issue, not merely an unaffected control path.
  - Covers all failure modes specified in the plan's test plan.
  - Concrete command invocations and outputs are shown, not unsupported claims like "tested locally" or "works on my machine".

## Diff quality (harness category: unreviewable)

- **Where it lives**:
  - In an eval bundle: inspect the `Diff` and `Commits` sections under `Candidate PR`.
  - In live mode: inspect `git diff main...HEAD` and `git log main..HEAD`.
- **What good looks like**:
  - The diff is clean, minimal, and focused strictly on the fix.
  - Free of debug leftovers (temporary print, eprintln, console.log, or probe functions).
  - Free of commented-out dead code or abandoned experimental attempts.
  - Free of formatting, whitespace, or import churn that obscures review.
  - Commits have meaningful, descriptive messages rather than placeholder churn ("wip", "misc cleanups").

## Standards and comms (harness category: standards-wall)

- **Where it lives**:
  - In an eval bundle: compare `Repo facts` (pull request template requirements, contributing instructions, and AI policy) against the `Candidate PR`'s `Title` and `Description`.
  - In live mode: compare `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` against `pr_draft.md`.
- **What good looks like**:
  - All mandatory PR template sections and checklists are filled out with substantive content, not omitted or left blank.
  - Closes/Fixes issue references match repository conventions.
  - If the repository has a stated AI policy requiring disclosure (or if repo facts state AI usage must be disclosed), an explicit disclosure is present in the PR description.
  - Stated guidelines (such as changelog/whatsnew entries or release note requirements) are respected.

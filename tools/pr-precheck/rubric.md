# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Diff and description read against the accepted plan's scope, boundary, and deviation notes | The diff implements only what the accepted plan (including explicit deviation notes) specifies, without unannounced feature additions, new flags/configs, unrequested rewrites, or omitted deliverables. The description accurately reflects the delivered diff without claiming missing work. Honestly disclosed and planned scope deferrals pass. | required |
| Test evidence | Test evidence section read against the plan's test plan and the issue's reproduction steps | Test evidence demonstrates the fix by showing observable before-and-after results for the specific failure scenario reported in the issue and plan, exercising the broken code path rather than an unaffected control path. All failure modes identified in the plan's test plan must be evidenced. Mere assertions like 'tested locally' or 'works on my machine' without showing the reproduction re-run fail. | required |
| Diff quality | Unified diff and commit history read against codebase quality standards | The diff is reviewable and free of debug debris (temporary print/eprintln statements, debug logs), commented-out code or dead functions, and unnecessary formatting or import churn. Commits are clean rather than placeholder noise ('wip', 'misc cleanups'). | required |
| Standards compliance | PR description, title, and diff read against repo facts, PR template, and stated contribution policy | All explicit repository requirements from repo facts, the PR template, and CONTRIBUTING.md are satisfied: required template sections and checklists are filled in rather than omitted, required issue links (e.g., Closes #...) are present, and AI usage is disclosed if the repository policy mandates disclosure. | required |

## Verdict rule

Accept if every required check passes ('pass'). Reject if any required check fails ('fail') or is 'unclear'. Preferred checks never change the verdict. Any unclear grade defaults to fail.

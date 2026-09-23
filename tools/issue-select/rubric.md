# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| `maintainer-active` | `repo-facts` maintainer first-response sample and default-branch commit history | The repo shows active maintainer engagement: at least one default-branch commit or maintainer response within the last 90 days of the snapshot date. | required |
| `repo-alive` | `repo-facts` repo status, archive flag, and last push date | Repository is not archived, and the last push to any branch is within the last 90 days of the snapshot date. | required |
| `issue-available` | `repo-facts` Assignee field and comment thread | Assignees is `none`, and no user has claimed or posted an active, unmerged PR in the comment thread within the last 30 days without being unassigned or abandoning it. | required |
| `ai-policy-permitted` | `repo-facts` contribution policy section | The repository contribution policy does not ban or disallow AI-generated code, AI documentation, or AI-assisted contributions. | required |
| `manageable-scope` | Issue title, body, and labels | The issue describes an actionable, localized task or bug report rather than a sprawling multi-module architecture redesign, open-ended research question, or epic tracking issue. Passes even if the description is brief, open-ended, or lacks reproduction steps, as long as it does not demand an overhaul of the entire repository. | required |
| `fresh-discussion` | `repo-facts` (total comments, linked PRs) and comment thread | The issue is not an excessively churned graveyard (does not have dozens of abandoned claims and multiple closed, unmerged PR attempts across multiple years without resolution). | required |
| `has-repro-steps` | Issue body | Explicit step-by-step reproduction instructions or code samples are present. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if and only if all `required` checks receive a pass (`P`). If any `required` check receives a fail (`F`) or unclear (`?`), the verdict is `reject`. Checks marked `preferred` never change the binary verdict; they only rank or distinguish accepted issues.
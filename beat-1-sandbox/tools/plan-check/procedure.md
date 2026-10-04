# Procedure: how this skill grades a plan package

## Read order

1. In live mode, read `scope.md` first to confirm the issue is inside the scoped source and note any house rules.
2. Read `rubric.md` and `references/evidence-guide.md` to list the rubric's checks and the verdict rule.
3. Read the whole package before grading anything: read the issue context first, then the repo-facts, then the repro evidence, then the candidate plan comment, and finally the candidate plan itself. The order matters because you must understand the issue context and repro evidence to judge if the plan actually fixes the root cause. Note down the root cause and the specific error shown in the repro evidence.

## Evidence gathering

1. For each check in the rubric, gather exactly the evidence named in the "Evidence" column, using the `references/evidence-guide.md` to find where it lives.
2. In live mode, gather issue-side evidence from the locations the guide names; the drafts in the working directory are the candidate side.
3. In eval mode, use only the text provided in the bundle as evidence; quote the relevant lines directly from the bundle. Record the specific quote or fact that decides the check.

## Check execution

1. Execute the checks in the order they appear in the rubric.
2. Grade each check `pass`, `fail`, or `unclear`, using the pass condition defined in the rubric.
3. Provide a one-line evidence quote or fact for each grade. 
4. If the evidence needed to make a decision is genuinely absent from the package, grade the check as `unclear`.
5. You may grade a check from the notes taken during the initial read if they contain the necessary evidence; otherwise, go back and re-read the specific section of the package to find the evidence.

## Verdict assembly

1. Apply the verdict rule exactly as stated in `rubric.md` to combine the per-check grades into the final verdict.
2. The final verdict must be strictly binary: `accept` (ready to post) or `reject` (hold).
3. Treat `unclear` grades exactly as the verdict rule directs (e.g., as `fail`).
4. Output the result as a fenced JSON block with the final verdict and each check's name, grade, and one-line deciding evidence, exactly as the skill's output format requires.

# Procedure: how this tool grades a PR package

## Read order

1. **Issue context and thread**: Read the issue description and comments to understand the reported bug, maintainer guidance, and exact reproduction steps.
2. **Repo facts**: Read the repository's PR template requirements, contribution guide rules, and stated AI-use policy.
3. **Plan context**: Read the accepted plan, noting its stated scope boundary, implementation approach, test plan, and any explicit deviation notes.
4. **Candidate PR description and commits**: Read the PR title, description, and commit messages. Note claimed changes, issue links, template checklist entries, and AI disclosures.
5. **Candidate PR diff**: Read the unified diff side-by-side against the plan context.
6. **Candidate PR test evidence**: Read the test evidence side-by-side against the plan's test plan and the issue's repro steps.

## Evidence gathering

- **Plan fidelity (silent-drift)**: Compare the files and hunks changed in the diff against the plan's scope. Note any unannounced additions (new flags, config options, rewrites of unaffected functions, or untouched files modified). Compare description claims against the diff to ensure no claimed changes are missing from the diff. Check whether any departures from the plan are documented in deviation notes and disclosed in the PR description.
- **Test evidence (not-tested)**: Check whether the test evidence re-runs the specific reproduction scenario(s) from the issue/plan. Verify that it demonstrates observable before-and-after output on the failing path, rather than testing only a control path or asserting 'tested locally'. Verify that all failure modes in the plan's test plan are covered and that project test suites pass.
- **Diff quality (unreviewable)**: Inspect the unified diff for debug statements (e.g. print/eprintln), commented-out code, dead/unused helper functions, or indentation/import churn. Check commit messages for uninformative placeholders ('wip', 'fix', 'cleanup').
- **Standards compliance (standards-wall)**: Compare the PR against the repo facts. Check whether required PR template sections and checklists are filled out. Check whether required issue references (Closes #...) are present. Check whether AI usage is disclosed if the repository requires disclosure.

## Check execution

Execute the checks in rubric order:
1. Grade **Plan fidelity** as pass, fail, or unclear. Record the deciding file, hunk, or quote as evidence.
2. Grade **Test evidence** as pass, fail, or unclear. Record the decisive test run or missing repro as evidence.
3. Grade **Diff quality** as pass, fail, or unclear. Record any debris found or note that the diff is clean.
4. Grade **Standards compliance** as pass, fail, or unclear. Record the satisfied or missing repo requirement.

If evidence for a check is missing or unverifiable, grade that check as fail.

## Verdict assembly

1. Apply the rubric's verdict rule:
   - If all required checks pass, the verdict is `accept`.
   - If any required check fails or is unclear, the verdict is `reject`.
2. Format the output with a concise per-check summary, followed by the mandatory fenced JSON block as the very last element of the response.

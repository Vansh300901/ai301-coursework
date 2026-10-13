# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/89

**Branch**

fix/68-empty-keyword-index

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

19/20 scored items  (bar: 18/20: PASS)

**Package analysis**

pkg-20: Gold label is 'reject', and my rubric decided 'reject'. The package addresses Ghostty issue #13604 with a clean, bounded fix and mode-2031 evidence. However, Ghostty's stated contribution policy strictly requires disclosing all AI assistance. The candidate PR description contained no AI-use disclosure. Because my rubric's 'Standards compliance' check mandates compliance with repository AI disclosure policies, the package failed and was correctly rejected.

**Check rationale**

"| Standards compliance | PR description, title, and diff read against repo facts, PR template, and stated contribution policy | All explicit repository requirements from repo facts, the PR template, and CONTRIBUTING.md are satisfied: required template sections and checklists are filled in rather than omitted, required issue links (e.g., Closes #...) are present, and AI usage is disclosed if the repository policy mandates disclosure. | required |"

This check explicitly includes verifying whether "AI usage is disclosed if the repository policy mandates disclosure" alongside required template sections and checklists. Without this explicit clause, rubrics easily overlook repository-specific AI disclosure policies (such as Ghostty in pkg-20) or required PR template checklists (such as pandas in pkg-01). I formulated this check to enforce both structured template compliance and external repository policies as required gates before submission.

**Trade-offs**

By enforcing strict evidence requirements on all failure paths named in the plan, my rubric rejected pkg-16 (minikube) because it judged the deferral of the start-path behavior as insufficiently evidenced, whereas the gold label accepted it as an honestly disclosed and scoped deferral. The trade-off is erring on the side of caution—holding back candidate PRs that defer planned scope unless the deferral and its boundaries are overwhelmingly clear—in order to prevent untested or silently drifted changes from slipping through.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.

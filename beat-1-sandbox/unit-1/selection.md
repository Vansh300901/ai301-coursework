# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**


**Verdict output**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
All three are in-scope (codepath/pathreview-ai301-fa26-s3). Evidence gathered live via the public GitHub API on 2026-09-23.

Repo-level facts (shared by all three candidates)
- archived: false, pushed_at: 2026-09-16T21:50:20Z — 7 days ago.
- Last 5 default-branch commits all by Aburke225 (COLLABORATOR), newest 2026-09-16.
- docs/CONTRIBUTING.md (189 lines), README, and .github/PULL_REQUEST_TEMPLATE.md contain no mention of AI/LLM/assistant/generated — no ban, and silence passes.
- The repo has 0 pull requests total, and the timelines for #68/#2/#4 show only labeled events — no linked or abandoned PRs anywhere.

Ranked read-out — all three accepted

1. #68 — Keyword search ZeroDivisionError on empty index — the tightest of the three: bug + good first issue + tier-1, 2–4h, two named files, and it's the only one passing the preferred has-repro-steps check (index([]) reproduces it; the covering xfail test H-01 is named). The one claim comment (acordero4852, 2026-09-19, association NONE) is a classmate, which the Path Review house rule says does not block — claim it anyway.
2. #2 — Ingest a portfolio website URL — tier-2, 5–8h, one new parser plus two touch points; bounded feature, zero comments, no competition.
3. #4 — Parse GitHub Actions workflows for CI/CD skills — tier-3, 6–10h, six labels spanning ingestion/docs/tests; still a single bounded parser, but the largest surface of the three.

⚠️ The fit profile in scope.md is still the placeholder ((Write a few sentences here.)). Ranking above therefore rests on rubric-visible signals only — effort estimate, tier, preferred-check grade, file count — not on your stated experience. Fill that section in if you want the ordering to reflect your languages and goals.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 7 days before 2026-09-23."},
      {"name": "repo-alive", "grade": "pass", "evidence": "archived=false, disabled=false, pushed_at=2026-09-16T21:50:20Z (7 days)."},
      {"name": "issue-available", "grade": "pass", "evidence": "assignees: none; only claim is classmate acordero4852 (author_association NONE, 2026-09-19), which the Path Review house rule says does not block; repo has 0 PRs."},
      {"name": "ai-policy-permitted", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, README.md and PULL_REQUEST_TEMPLATE.md contain no AI/LLM/generated-code restriction."},
      {"name": "manageable-scope", "grade": "pass", "evidence": "Labeled 'good first issue'/'tier-1'; localized fix in rag/retriever/keyword_search.py + its test, 'Estimated effort: 2-4 hours'."},
      {"name": "fresh-discussion", "grade": "pass", "evidence": "Opened 2026-09-10, 1 comment, no closed/unmerged PR attempts (repo has 0 PRs)."},
      {"name": "has-repro-steps", "grade": "pass", "evidence": "Body gives the reproducing call 'index([])' raising ZeroDivisionError and names the covering xfail test (manifest id H-01)."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/2",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 7 days before 2026-09-23."},
      {"name": "repo-alive", "grade": "pass", "evidence": "archived=false, disabled=false, pushed_at=2026-09-16T21:50:20Z (7 days)."},
      {"name": "issue-available", "grade": "pass", "evidence": "assignees: none, 0 comments, timeline shows only 'labeled' events and the repo has 0 PRs."},
      {"name": "ai-policy-permitted", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, README.md and PULL_REQUEST_TEMPLATE.md contain no AI/LLM/generated-code restriction."},
      {"name": "manageable-scope", "grade": "pass", "evidence": "One bounded feature (new ingestion/parsers/web_parser.py plus pipeline and schema touch points), 'Estimated effort: 5-8 hours'; not an epic or tracking issue."},
      {"name": "fresh-discussion", "grade": "pass", "evidence": "Opened 2026-09-10, 0 comments, no prior PR attempts."},
      {"name": "has-repro-steps", "grade": "fail", "evidence": "Feature request body has no reproduction steps or code sample."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/4",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR), 7 days before 2026-09-23."},
      {"name": "repo-alive", "grade": "pass", "evidence": "archived=false, disabled=false, pushed_at=2026-09-16T21:50:20Z (7 days)."},
      {"name": "issue-available", "grade": "pass", "evidence": "assignees: none, 0 comments, timeline shows only 'labeled' events and the repo has 0 PRs."},
      {"name": "ai-policy-permitted", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, README.md and PULL_REQUEST_TEMPLATE.md contain no AI/LLM/generated-code restriction."},
      {"name": "manageable-scope", "grade": "pass", "evidence": "Single new parser (ingestion/parsers/workflow_parser.py) feeding skill_extractor.py, 'Estimated effort: 6-10 hours'; bounded feature, not a redesign."},
      {"name": "fresh-discussion", "grade": "pass", "evidence": "Opened 2026-09-10, 0 comments, no prior PR attempts."},
      {"name": "has-repro-steps", "grade": "fail", "evidence": "Feature request body has no reproduction steps or code sample."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

1. Run 1: 14/20 scored items. `reproducible-scope` was excessively strict and rejected valid non-bug tasks (`issue-01`, `issue-04`, `issue-11`, `issue-16`, `issue-19`), while `issue-15` slipped through as an accept despite having 97 comments, multiple abandoned claims, and unmerged closed PRs spanning multiple years.
2. Run 2: 17/20 scored items. Added `fresh-discussion` to catch churned issue graveyards like `issue-15`. However, `issue-12` was accepted when gold marked it reject because the rubric lacked an AI contribution policy check, failing the required category floor (`policy 0/1`).
3. Run 3: 20/20 scored items (agreement: 20/20 scored items (bar: 18/20: PASS)). Added the `ai-policy-permitted` check and calibrated `manageable-scope` so concise issues and localized enhancements are not rejected for lacking formal stack traces.

**Issue analysis**

- **Issue ID**: `issue-12`
- **Rubric decision**: `reject`
- **Gold verdict**: `reject`
- **Reasoning**: In `eval/issues/issue-12.md`, the issue itself is a small, bounded UI feature ("Include in-progress books in the reading-goal progress-bar") with an active maintainer and no assignees. However, the `repo-facts` section quotes BookWyrm's `CONTRIBUTING.md` under section "Generative AI": *"Meaningful human interaction is the whole point of BookWyrm. We do not accept AI-generated code or documentation."* Because this course workflow utilizes AI tooling, contributing to a repository that explicitly bans AI-generated code and documentation violates repository policy. The rubric caught this via `ai-policy-permitted`, producing an exact agreement with the gold label.

**Check rationale**

```markdown
| `ai-policy-permitted` | `repo-facts` contribution policy section | The repository contribution policy does not ban or disallow AI-generated code, AI documentation, or AI-assisted contributions. | required |
```

Many open-source repositories explicitly disallow contributions written or assisted by LLMs. Attempting to submit AI-generated pull requests to these projects wastes maintainer time, leads to immediate closure, and violates community standards. Setting this check to required guarantees that even if an issue has ideal scope, zero assignees, and active maintainers, our skill rejects it if our AI-assisted development workflow cannot be used compliantly.

**Trade-offs**

This check sacrifices repositories that have blanket AI-ban statements in their guidelines but might realistically be flexible about minor documentation fixes or localized bug patches. It also risks false rejections on ambiguous policy text where AI is merely cautioned against rather than banned. However, in our evaluation suite, it cleanly separated compliant projects from strict ban repositories like BookWyrm (issue-12), ensuring zero policy violations without altering any verdicts across the other 19 scored benchmark bundles.
---

## Selection rationale


**Selection rationale**

1. Fit to interests and time: Issue #68 is a localized Python bug in rag/retriever/keyword_search.py addressing a ZeroDivisionError when an index is empty. With an estimated effort of 2–4 hours (tier-1), it fits perfectly within my available bandwidth for Unit 2 and aligns with my interests in Python backend debugging and retrieval pipelines.

2. What the verdict identified correctly vs. what I weighed: The rubric correctly verified that the repository has active maintainers (pushed 7 days ago), has no AI policy ban, has zero conflicting PRs, and has clear reproduction evidence (index([]) with an existing xfail test H-01). Beyond the rubric, I personally weighed that an existing failing unit test makes setting up the reproduction environment in Unit 2 much faster and safer than a feature issue (such as #2 or #4) where I would need to design parsers and test suites from scratch.

3. Anticipated difficulty in claiming: The anticipated difficulty is very low. The issue has no assigned owner. Although classmate acordero4852 posted a claim comment on 2026-09-19, Path Review house rules explicitly state that classmate claims do not block other students from claiming and working on the issue.

# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Vansh300901

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5984543185

Hi maintainers, I am investigating this issue as part of a course project.

I have reproduced the `ZeroDivisionError` and found that `BM25Okapi` crashes when initialized with an empty corpus because it attempts to compute average document length. 

I propose the following plan to resolve the root cause:
- **Scope**: I will modify `rag/retriever/keyword_search.py` and the `tests/unit/test_keyword_search.py` test file. I will not modify `rank_bm25` or other retrievers.
- **Approach**: I'll add a guard clause at the start of `KeywordSearcher.index` that returns early when `chunks` is empty, setting `self.bm25 = None` and `self.chunks = []`. This aligns with `__init__` and how `search` gracefully handles an empty state. Additionally, as requested in the issue, I will remove the `@pytest.mark.xfail` marker from `test_empty_index` so that the CI job passes (it currently fails on XPASS because strict=True).
- **Test Plan**: I will verify by running the python script `python3 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([])"` to ensure it exits cleanly. I will also run `pytest tests/unit/test_keyword_search.py -k "test_empty_index" -v` and `make check && make test-unit` to confirm the test is green.
- **Risks**: I'm assuming other components handle `self.bm25 = None` properly, which seems to be the case based on how `search()` is written.

I'll be building this on branch `fix/68-empty-keyword-index`. Would this approach be acceptable to merge?


---

## Your branch

**Branch**

fix/68-empty-keyword-index

**Evidence**

### Before Fix (Reproducing Issue)
```bash
python3 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([])"
```
```
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/vansh/Desktop/pathreview-ai301-fa26-s3/rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/vansh/Desktop/pathreview-ai301-fa26-s3/.venv/lib/python3.13/site-packages/rank_bm25.py", line 52, in __init__
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
```
```bash
pytest tests/unit/test_keyword_search.py -k "test_empty_index" -v
```
```
============================= test session starts ==============================
collected 17 items / 16 deselected / 1 selected                                

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index XFAIL [100%]

================== 16 deselected, 1 xfailed in 0.23s ===================
```

### After Fix (Verification)
```bash
python3 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([])"
```
```
2026-10-04 17:20:14 [info     ] keyword_index_empty           
```
```bash
pytest tests/unit/test_keyword_search.py -k "test_empty_index" -v
```
```
============================= test session starts ==============================
collected 17 items / 16 deselected / 1 selected                                

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [100%]

======================= 1 passed, 16 deselected in 0.22s =======================
```


## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

18/20 scored items (bar: 18/20: PASS)

**Package analysis**

pkg-05: The gold label was 'accept', but my rubric rejected it because it failed the 'Unknowns acknowledged' check. My rubric strictly required the plan to explicitly state assumptions or risks. Since pkg-05 lacked this, it was rejected.

**Check rationale**

`Unknowns acknowledged: The plan must state what is unknown, such as whether other components might expect a different default state.`
This check reads this way because maintaining an existing codebase often involves unforeseen ripple effects. I revised this check to be strict to ensure the author communicates their awareness of risks to the maintainers, which builds trust.

**Trade-offs**

By strictly enforcing the 'Unknowns acknowledged' check, I accepted that plans like `pkg-05` and `pkg-14` would be rejected despite being labeled 'accept' by the gold standard. The trade-off is rejecting potentially functional plans in order to guarantee that all accepted plans thoroughly communicate their risks and unknowns.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.

## Diagnosis
Calling `.index([])` raises a `ZeroDivisionError` as seen in the repro evidence's traceback at `rank_bm25.py", line 52`. The `rank_bm25` library expects a non-empty corpus to compute the average document length. When we pass an empty list of chunks, `KeywordSearcher.index` passes an empty list to `BM25Okapi`, causing the crash.

## Scope
We will modify `rag/retriever/keyword_search.py` to handle empty indexing gracefully. We will also update the test `tests/unit/test_keyword_search.py::test_empty_index` to remove any `xfail` marker so it can pass normally. We will not modify the `rank_bm25` library or other retrievers.

## Approach
1. In `rag/retriever/keyword_search.py`, modify the `index` method.
2. Add a check at the beginning of `index`:
   ```python
   if not chunks:
       self.chunks = []
       self.bm25 = None
       logger.info("keyword_index_empty")
       return
   ```
3. In `tests/unit/test_keyword_search.py`, locate `test_empty_index` and remove the `@pytest.mark.xfail` decorator.

## Test plan
1. Run the python script from the repro:
   `python3 -c "from rag.retriever.keyword_search import KeywordSearcher; s = KeywordSearcher(); s.index([])"`
   Expectation: The command will exit with code 0 and no output, proving the `ZeroDivisionError` is resolved.
2. Run the pytest command from the repro:
   `pytest tests/unit/test_keyword_search.py -k "test_empty_index" -v`
   Expectation: The test `test_empty_index` will show as `PASSED`.

## Risks and unknowns
- I am not entirely sure if other components expect `self.bm25` to be initialized to an empty BM25 object rather than `None` when `index([])` is called. However, `__init__` sets `self.bm25 = None` initially and `search()` explicitly checks `if not self.bm25: return []`, so this approach appears safe.

## Deviations
Nothing changed during the build; the implementation matches the plan exactly.

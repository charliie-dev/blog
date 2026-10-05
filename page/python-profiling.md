# Python | Finding Slow Code with cProfile

Before optimising Python code, measure where the time actually goes. These notes follow mCoding's
video on diagnosing slow code;[^mcoding] the tool is a profiler, and Python ships one.

## Profile with cProfile

Wrap the code you want to measure in a `cProfile.Profile` context manager, then print the
statistics with `pstats`:

```python
import cProfile
import pstats

with cProfile.Profile() as pr:
    count_https_in_web_pages()  # the function to measure

stats = pstats.Stats(pr)
stats.sort_stats(pstats.SortKey.TIME)
stats.print_stats()
```

`SortKey.TIME` sorts by `tottime`, the time spent inside each function itself, not counting the
functions it calls. That is the column to read first: the functions at the top are where the
program really spends its time. `cumtime`, by contrast, includes everything a function calls.

## Explore the results with snakeviz

A long table is hard to read. snakeviz turns the same data into an interactive web page. Save the
statistics to a file instead of, or as well as, printing them:

```python
stats.dump_stats("needs_profiling.prof")
```

Then open it:

```sh
pip install snakeviz
snakeviz ./needs_profiling.prof
```

snakeviz starts a local server and shows the call tree in the browser, where each function's
share of the time is drawn to scale.

[^mcoding]: [mCoding: Diagnose slow Python code. (Feat. async/await)](https://youtu.be/m_a0fN48Alw)


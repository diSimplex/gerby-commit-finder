# Test benchmark failures for Align.tex

## Effected tests

- attempt-2-20240307
  - [test_benchmark.align.tex.md](../attempt-2-20240307/test_benchmark.align.tex.md)

## Problem

Commit `2017_09_29-22_19_23-8f98825`, "Make sure the EndRow argument for
matrix environments leaves initial whitespace", adds extra space
characters into the output of the LaTeX -> HTML rendering of the
`unittests/sources/align.tex` example.

## Solution

Correct the benchmark output `unittests/benchmarks/align.html`.

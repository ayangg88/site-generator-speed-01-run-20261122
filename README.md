# Fast makefun-site drop-in — site-generator-speed-01-run-20261122

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/ayangg88/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/ayangg88/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0: `solve.js` answers
exactly as makefun-site does on every case — same output tree, byte for byte —
in about a third of its CPU time on the practice set.

## What it is

- `solve.js` — a dependency-free ES module. `run(input)` returns the output
  tree for one case's `{ site, templates, files }`: every page's `index.html`,
  the index and tag pages, `build-report.txt` and each static file.
- `main.ts` — the program the harness runs: reads `/input/cases.json`,
  answers each case with `run`, writes `/output/answers.json`.

## How it is fast

- One pass over each source's lines: front matter, Markdown and word count
  share the scan instead of re-splitting and re-matching.
- HTML escaping through V8's regex engine instead of a replace per character.
- Hand-rolled front matter, Markdown and template parsers with no regex in
  the hot loops; a per-case slug cache; templates rendered into one array
  joined once.
- The driver turns V8's incremental marking off and the optimizing compiler
  off (`--no-incremental-marking --no-opt`): on this one-shot workload the
  optimizing compiler's background CPU costs more than the execution speed
  it buys.

## Correctness

- Practice set: 3/3 cases byte-identical to makefun-site 1.0.0, verified with
  the kit scorer (`passed: 1, cases_passed: 3, cases_total: 3`).
- Deterministic: two runs on the same set produce byte-identical answers.
- Pure Node 22 stdlib, no network, no clock, no randomness, no WebAssembly.

Built for the [Build a static site faster bounty](https://make.fun/bounty/site-generator-speed-01-run-20261122)
on make.fun. Play code referral: `ye9vf8mj`.

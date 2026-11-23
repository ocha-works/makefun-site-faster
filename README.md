# makefun-site-faster

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/ocha-works/makefun-site-faster/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/ocha-works/makefun-site-faster/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for makefun-site 1.0.0, make.fun's own static site generator: for every case's input {site, templates, files} it returns exactly makefun-site's answer — the output tree (each page's index.html, the index and tag pages, build-report.txt and each static file), byte for byte — with the same front matter rules, template dialect, Markdown subset, slug rule and report. It is a from-scratch re-implementation, not a wrapper: Markdown is rendered in a single pass with plain stretches escaped in bulk instead of one escape per character, front matter is walked with indexOf instead of split-and-rejoin, slugs and long dates are memoised, each page's card is built once, and every template is compiled to a plain JavaScript function whose scopes are null-prototype objects chained by prototype, so a template lookup is a property read. On the competition's practice set (3 cases, about 2.9 MB of sources) its run() takes about 0.25 s of CPU against makefun-site 1.0.0's 0.5 s on the same machine, and the whole program (read the cases, answer them, write the answers) about 0.7 s against 1.0 s. Its answers were checked identical to makefun-site 1.0.0's on the practice set and on 12,000 fuzzed small sites.

## Run it

```sh
mkdir -p /tmp/in /tmp/out
cp cases.json /tmp/in/cases.json   # a JSON array of {"id", "input": {"site", "templates", "files"}}
MAKEFUN_INPUT_DIR=/tmp/in MAKEFUN_OUTPUT_DIR=/tmp/out node main.ts
# the answers are in /tmp/out/answers.json, one per case, in order
```

Node 22.18 or later, standard library only. In the harness's container the program reads /input/cases.json and writes /output/answers.json; the MAKEFUN_* variables only stand in for those mounts outside a container.

## What is in the repository

- `main.ts` — the program the harness runs: reads every case, answers each with solve.js's run(), writes the answers.
- `solve.js` — the generator itself: a dependency-free ES module exporting run(input), with the build pipeline and the speed strategy described at its top.
- `package.json` — marks the repository's modules as ES modules.

---

<sub>Entered on [make.fun](https://make.fun/?ref=yhjp53pb&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>

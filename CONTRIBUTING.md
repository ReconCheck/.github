# Contributing to ReconCheck

Thanks for being here. The engine is **open, early and interface-moving**; the
rule packs in [rules](https://github.com/ReconCheck/rules) are private and not
open for contribution yet.

## The single most useful thing you can give us

**Real-world document pairs that break existing tools.** Open an issue and
describe the pair — scans, skewed tables, borderless multi-column PDFs, Excel
exports with merged cells, oddly-encoded CSVs. The hardest part of this
project is layout recovery and entity alignment, and both are starved for
samples. You can't break the engine with documents that are *too messy*.

## Before you open an issue

1. Read the [docs](https://github.com/ReconCheck/core/blob/main/README.md)
   (quick start, capabilities, limits) — some behaviours are deliberate:
   read-only, exact-key alignment for now, PDF text layer only, per-pair
   failure isolation, TTL retention.
2. Search existing issues — duplicates get closed fast.
3. If you can run it locally, reproduce with the smallest pair that shows the
   problem (please never attach real invoices/PII; sanitise first).

## Issue templates

- **Bug report** — use the [bug template](ISSUE_TEMPLATE/bug_report.yml):
  how to reproduce, the documents involved (sanitised), expected vs actual,
  version. Findings without a reproducible pair are hard to act on.
- **Feature request** — use the [feature template](ISSUE_TEMPLATE/feature_request.yml):
  what problem it solves, for whom, and what "done" looks like. Feature ideas
  that come with a sample document pair (same rules as above) jump the queue.

## Pull requests

Not merged yet — interfaces are still moving and a PR today would be churned
by tomorrow's refactor. Until v1 freezes:

- discuss the change in an issue first, and
- expect the maintainers to say "wait for v1" more often than not.

Once the public interfaces stabilise (report JSON ver 1, rule format ver 1),
this page will flip to a normal: fork → branch → tests → PR.

## Code of conduct

Be constructive, in any language — the project is bilingual (English +
中文). Maintainers are volunteers; treat them accordingly.

## License

Contributions are accepted under the repo's Apache-2.0 license.
# StagePulse Evaluation Data (`stagepulse-eval`)

Dedicated repository storing daily evaluation snapshots and benchmark datasets for [StagePulse](https://github.com/abx67/stagepulse).

## Overview
StagePulse's evaluator records daily pipeline execution outputs (`<run_date>.json`), capturing:
- Raw scraped articles
- Selected article batches
- Scoring function hashes and per-article signal classifications (casting, career, business, awards, etc.)
- Breadth scoring and deduplication results

Separating this evaluation data from the main application codebase keeps the primary repository lightweight while preserving version history, diffs, and historical replays for evaluation experiments.

## Snapshot Structure
Each daily file (`YYYY-MM-DD.json`) conforms to schema version 1:
```json
{
  "schema_version": 1,
  "run_date": "2026-11-19",
  "captured_at": "2026-11-19T22:10:10.268016",
  "score_fn_hash": "69e3455b",
  "raw_count": 25,
  "selected_count": 8,
  "config": {
    "standard_max_items": 8,
    "extended_max_items": 20
  },
  "articles": [ ... ]
}
```

## Usage with StagePulse
When cloned alongside `stagepulse` (as a sibling folder `../stagepulse-eval`), StagePulse automatically detects and reads evaluation snapshots from this repository.

Alternatively, configure the path via environment variable:
```bash
export STAGEPULSE_EVAL_DIR="/path/to/stagepulse-eval"
```

To replay an evaluation run:
```bash
python -m src.evaluator --run 2026-11-14
```

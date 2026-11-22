# StagePulse Data Repository (`stagepulse-data`)

Dedicated data repository storing evaluation snapshots, benchmarks, and dataset assets for [StagePulse](https://github.com/abx67/stagepulse).

## Directory Structure
```
stagepulse-data/
├── eval/             # Daily evaluation run snapshots (*.json)
└── (future datasets) # Benchmarks, golden sets, scraped archives, etc.
```

## Data Categories

### 1. `eval/` — Daily Evaluation Snapshots
Records daily pipeline execution outputs (`<run_date>.json`), capturing:
- Raw scraped articles
- Selected article batches
- Scoring function hashes and per-article signal classifications (casting, career, business, awards, etc.)
- Breadth scoring and deduplication results

#### Snapshot Structure
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
When cloned alongside `stagepulse` (as sibling folder `../stagepulse-data`), StagePulse automatically discovers the data directory.

Alternatively, configure the path via environment variable:
```bash
export STAGEPULSE_DATA_DIR="/path/to/stagepulse-data"
# or point directly to the eval subfolder:
export STAGEPULSE_EVAL_DIR="/path/to/stagepulse-data/eval"
```

To replay an evaluation run in StagePulse:
```bash
python -m src.evaluator --run 2026-11-14
```

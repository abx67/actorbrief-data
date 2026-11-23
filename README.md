# ActorBrief Data Repository (`actorbrief-data`)

Dedicated data repository storing evaluation snapshots, production state backups, and dataset assets for **ActorBrief** (formerly StagePulse).

## Directory Structure
```
actorbrief-data/
├── eval/             # Daily evaluation run snapshots (*.json)
├── state/            # Production state backup (processed URLs, daily newsletters)
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
  "run_date": "2026-11-20",
  "captured_at": "2026-11-20T22:04:15.123456",
  "score_fn_hash": "69e3455b",
  "raw_count": 115,
  "selected_count": 8,
  "config": {
    "standard_max_items": 8,
    "extended_max_items": 20
  },
  "articles": [ ... ]
}
```

### 2. `state/` — Production Runtime State Backup
Stores production snapshots of:
- `processed_urls.json`: Deduplication history of article URLs processed over the 30-day sliding window to prevent duplicate deliveries.
- `daily_articles.json`: The latest standard 8-item newsletter batch.
- `daily_articles_extended.json`: The latest extended 20-item newsletter batch.

## Usage with ActorBrief
When cloned alongside the codebase (as sibling folder `../actorbrief-data`), ActorBrief automatically discovers the evaluation directory.

Alternatively, configure the path via environment variable:
```bash
export ACTORBRIEF_DATA_DIR="/path/to/actorbrief-data"
# or point directly to the eval subfolder:
export ACTORBRIEF_EVAL_DIR="/path/to/actorbrief-data/eval"
```

To replay an evaluation run in ActorBrief:
```bash
python -m src.evaluator --run 2026-11-20
```

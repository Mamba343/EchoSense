# Database

EchoSense currently has no database, migrations, database schema, ORM, or connection configuration.

## Current Persistence

Only repository files exist. No runtime data has been generated.

## Planned Dataset Storage

The existing docs plan file-based dataset sessions under `datasets/`:

```text
datasets/
  YYYY-MM-DD_HH-MM-SS/
    metadata.json
    raw.csv
    processed.csv
```

Planned metadata fields include:

- timestamp,
- firmware version,
- backend version,
- board type,
- router channel,
- sampling rate,
- calibration information,
- source mode,
- protocol version,
- room/setup notes.

## Planned Log Storage

The existing docs plan local structured logs under `logs/`:

- `backend.log`
- `events.log`

## Future Database Possibility

The existing roadmap mentions a lightweight local database only as a future improvement for long-running monitoring. It is not part of the current design.

## Open Decisions

- Whether sample datasets should be committed.
- Exact dataset metadata schema.
- Whether `raw.csv` stores raw device packets, parsed CSI rows, or both.
- Retention policy for logs and large recordings.
- Whether future long-running monitoring needs SQLite or another local database.


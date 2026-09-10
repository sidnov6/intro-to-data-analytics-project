# Data Area

The chosen project will use this structure:

- `raw/` — immutable source files exactly as collected
- `interim/` — partially cleaned or joined data
- `processed/` — analysis-ready data
- `external/` — small reference files from third parties where redistribution is permitted

The folders are ignored by default because datasets may be large, private, revised, or restricted. Keep only `.gitkeep` placeholders until the team documents that a file is safe and useful to commit.

For every source, record:

- direct source and documentation links;
- owner/publisher and access date;
- licence or usage conditions;
- collection query, parameters, and time zone;
- coverage, row count, and unit of analysis;
- field meanings and units;
- missing-data codes and known revisions;
- transformations from raw to processed data; and
- a reproducible way to acquire or reconstruct the data.

Never commit credentials, tokens, private personal data, or a dataset whose redistribution terms are unclear.

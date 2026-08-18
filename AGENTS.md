# AGENTS.md

## Cursor Cloud specific instructions

This repository is a static [Frictionless Data Package](https://frictionlessdata.io/):
a dataset of world airport codes. It has no application server or build step. The
"product" is `datapackage.json` (the schema descriptor) plus its CSV resource at
`data/airport-codes.csv`. The core workflow is describing/extracting/validating the
data package with the `frictionless` CLI.

- Dev tooling lives in a Python virtualenv at `.venv` (git-ignored) with `frictionless`
  installed from `requirements-dev.txt`. The startup update script recreates/refreshes it.
- Run commands via the venv, e.g. `.venv/bin/frictionless <cmd>` (or activate with
  `source .venv/bin/activate`). Useful commands:
  - `.venv/bin/frictionless describe datapackage.json` — infer/print resource metadata.
  - `.venv/bin/frictionless extract data/airport-codes.csv --limit-rows 5` — read/cast rows.
  - `.venv/bin/frictionless validate datapackage.json` — validate the CSV against the schema.
- Gotcha: `frictionless validate` currently reports the package as INVALID. This is a
  known data/schema mismatch, NOT an environment problem: the `coordinates` field is
  declared as `geopoint`, and frictionless v5's default geopoint parser rejects the
  `"lat, lon"` string cells used in the CSV. The tooling itself works end-to-end (it
  reads all ~46k rows and produces a report). Do not "fix" this by editing the data
  unless explicitly asked.
- System dependency: creating the venv needs the `python3-venv` package
  (`python3.12-venv` on this image). It is preinstalled in the environment snapshot;
  the update script does not install system packages.

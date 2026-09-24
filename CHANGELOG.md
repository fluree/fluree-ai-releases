# Fluree AI changelog

Customer-facing changes and upgrade information for Fluree AI.

## [0.10.0-rc.3] - 2026-09-23

### Added

- Windows x64 developer downloads and a PowerShell installer, with foreground
  server startup, local setup, and offline licensing. Local data storage requires
  no Windows Developer Mode or administrator privileges.
- Supply a license directly through `FLUREE_AI_LICENSE` for container and cloud
  secret configuration without a license-file mount.
- Guided setup accepts a license key or license file and installs it during setup.
- Settings given to `init` as flags, such as `--data-dir` or `--license-file`, are
  saved in the new configuration.
- Setup finishes with a summary, next steps, and a pointer to command-line help.
- Dedicated transaction servers: transactions and indexing can run on their own
  pool of servers, separate from the servers that run the app and background
  processing. Adding app servers never changes the transaction pool. An
  asynchronous transaction's status stays available for about an hour after it
  finishes.
- The included documentation describes multi-node deployments: member clusters,
  query peers and dedicated transaction servers, with their storage, network and
  upgrade requirements.
- A new mapping screen brings an uploaded spreadsheet (Excel or CSV) or a data
  lake's tables into a governed dataset. You first choose where the data goes:
  a new model built from the file, or an existing model.
- Build a model from a spreadsheet. The Model Drafter proposes, for each sheet,
  the class it becomes, the column that identifies a row, and what each column
  means, with its reasons. The Model Reconciler finds sheets that describe the
  same kind of thing and columns that mean the same across them. Nothing is
  applied until you accept it, and you can edit the draft before saving.
- Map a spreadsheet onto an existing model. A fit check shows where the file's
  records break the model's rules, and the Import Resolver proposes a fix for
  each: a model change, a value map, another column to fill the property from,
  or leaving those records out. Unmapped columns are matched to properties the
  model already has under another name.
- Import an updated spreadsheet again to update your data. Records are matched
  by their identifying column, so changed rows update the existing records
  instead of creating duplicates, and a cell that was emptied clears its value.
  A row with no identifying value is rejected rather than given a made-up
  identity.
- Value maps translate a column's codes to the model's values, and a separator
  splits one cell into several values.
- People who can only propose changes import onto a branch and can propose
  merging it into `main`.
- A dataset governed by a model is shown and browsed in Knowledge using that
  model's classes.
- Agents can declare an input and output schema and be called like a function:
  structured input in, one JSON result out, checked against the schema on the
  server.
- Workbooks that are really documents, such as a policy index, are extracted as
  documents: each sheet becomes a page and each block of cells a table.
- Extracted pages record their printed page number and display size, including
  rotated pages.
- Anyone who can read a dataset can open a source document's original file and
  its individual pages, so an app can show the page a citation points to.

### Changed

- Imports into a governed dataset are much faster.
- Model dialogs offer saving to `main` only to people who can write to it.

### Fixed

- Catalog and identity-provider credentials sent in requests are no longer
  written to server logs.
- Word, Excel and PowerPoint files saved by tools such as Excel Online are
  recognized and extracted instead of failing as generic archives.
- A dataset set to structure only is no longer materialized when a run starts
  from a mapping.
- A processing step that fails permanently is no longer retried.
- Writes during heavy loading wait for indexing to catch up instead of failing.
- Binding a model that has no access policies no longer locks everyone out of
  the dataset. The dataset allows access until the model has policies, and a
  warning says so.
- Administrators and dataset owners see all data in a governed dataset;
  previously their reads could return nothing.

### Known limitations

- A spreadsheet import handles up to 50,000 rows. A larger file is refused
  before anything is written, never partly imported.

### Upgrade notes

- Each server converts its Raft coordination logs to a new format on its first
  start, and earlier versions cannot read the converted logs. Back up the data
  directory before upgrading; to roll back, restore that backup.
- All servers in a multi-node deployment must run the same version. Stop them
  all, upgrade them together, then start them.

## [0.9.25-rc.3] - 2026-09-11

### Added

- Downloads for Apple Silicon Macs and Linux on x86-64 and ARM64.
- Standalone command-line application with guided configuration for login
  settings, AI provider keys, and server ports.
- Setup identifies optional AI provider keys and their uses, recognizes keys
  already set in environment variables, and keeps those values out of saved files.
- Foreground and background server commands, including status, logs, and shutdown.
- An included web app and customer documentation.
- License installation and status commands, with support for renewing a license.

### Fixed

- AI provider keys can be saved and updated in the app on standalone servers,
  with saved keys taking precedence over environment defaults.
- Startup reports an occupied HTTP address before initializing the database or
  starting background work, with guidance for choosing another address.
- Missing-license startup messages explain how to install a license and contact
  support.
- Archive upgrades use the web app included in the new executable and preserve
  explicitly configured custom web directories.
- Superseded copies of the embedded web app under the data directory are removed
  automatically instead of accumulating across upgrades.

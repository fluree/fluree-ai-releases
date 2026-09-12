# Fluree AI changelog

Customer-facing changes and upgrade information for Fluree AI.

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

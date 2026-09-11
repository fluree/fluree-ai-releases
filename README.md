# Fluree AI

This repository hosts official binary downloads and installation instructions.
The first public release is being prepared; the install commands below become
available when that release is published.

Fluree AI runs the database, AI chat and agents, extraction, entity resolution,
and web app in one process. A customer license is required to start the server.

## Install

On macOS or supported GNU/Linux systems:

```sh
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/fluree/fluree-ai-releases/releases/latest/download/fluree-ai-installer.sh | sh
```

Or install with Homebrew:

```sh
brew install fluree/tap/fluree-ai
```

Downloads are public; no GitHub login is required. Archives are also available at
https://github.com/fluree/fluree-ai-releases/releases. Extract the archive for
your platform and use `./fluree-ai` in place of `fluree-ai` below. The executable
includes the web app. Linux x86-64 requires glibc 2.35 or newer, and Linux ARM64
requires glibc 2.36 or newer; Alpine is not supported by these native binaries.

## Configure and run

From the directory where you want to keep the installation's settings:

```sh
fluree-ai init                       # prompts for login, AI keys, and port
fluree-ai license install /path/to/customer-license.jwt
fluree-ai server run
```

Open http://localhost:8090 and sign in with your configured admin email and
password. `init` binds to loopback by default. A provider key is needed for the
AI features you use; you can configure it later.

Configuration lives in `.fluree-ai/config.toml`. Local passwords and API keys
are saved in `.fluree-ai/secrets.toml` with owner-only permissions on macOS/Linux.
Secrets are plaintext; protect these files and their backups. The default data
directory is `.fluree-ai/data`.

```sh
fluree-ai config set server.listen 127.0.0.1:8090
fluree-ai config set admin.password        # hidden prompt
fluree-ai config set ai.openai_api_key     # hidden prompt
fluree-ai config list                     # secrets redacted
fluree-ai config validate
fluree-ai server start                    # background, macOS/Linux
fluree-ai server status
fluree-ai server logs --follow
fluree-ai server stop
```

Changes take effect on the next start. The admin password setting seeds a new
account; it does not reset an existing account's password. Foreground `server
run` (also `serve`, or no subcommand) sends logs to the terminal. Background
`start` waits for readiness and writes `<data-dir>/.runtime/server.log`.

Use `init --defaults` for scripted setup, `init --global` for user-wide settings,
or `--config /path/config.toml` to select a file. Otherwise the closest project
config is discovered from your current directory, then user config. Paths in
TOML are relative to its directory. CLI flags override environment variables,
which override TOML settings. Existing `env.local` installations can continue
using the archive's `run.sh`; without TOML config, data defaults to `./data`.
The CLI itself does not source `env.local`.

## License and renewal

A valid offline Fluree license is required at startup, including on query peers.
By default it is installed at `<data-dir>/license.jwt`; use `--license-file` or
`FLUREE_AI_LICENSE_FILE` for another location. `fluree-ai license status` reports
verified ownership and expiration without displaying the reusable key. Renewal
uses `license install` again, then a restart. Expiration does not interrupt a
running server in this version. Use the same config/data directory for license
and server commands. Keep customer license files private.

## Upgrade and backup

Stop the server, back up the configured data directory and settings, then rerun
the shell installer or run `brew upgrade fluree-ai`, depending on how you installed.
Restart with the same config/data directory. Upgrading the executable preserves
these directories. Do not run two servers against one data directory.

For archive installations, use the included upgrade script:

```sh
./fluree-ai server stop
./upgrade.sh --latest
# Or: ./upgrade.sh --release fluree-ai/v0.1.0
# Or: ./upgrade.sh /path/fluree-ai-aarch64-apple-darwin.tar.gz
./fluree-ai server start
```

The script verifies downloaded checksums, preserves data, `.fluree-ai/`, and
`env.local`, and keeps the previous executable at `fluree-ai.prev`. For legacy
`fluree-ai-server` installations, stop the old server and copy the new `upgrade.sh`
into the installation before the first upgrade. The current binary supplies its
own web assets unless you explicitly configure `server.web_dir`.

## Linux service

The archive includes `fluree-ai.service` with installation instructions. Run the
server in the foreground under systemd using `fluree-ai --config /path/config.toml
server run`; systemd manages the process and captures logs. Use an explicit config
path and a data directory writable by the service account.

## Docker

```sh
docker run --rm -p 8090:8090 -v fluree-data:/data \
  --mount type=bind,src=/absolute/path/customer-license.jwt,dst=/license.jwt,readonly \
  -e FLUREE_AI_LICENSE_FILE=/license.jwt \
  -e FLUREE_AI_ADMIN_EMAIL=admin@example.com \
  -e FLUREE_AI_ADMIN_PASSWORD \
  fluree/fluree-ai:0.1.0
```

Set `FLUREE_AI_ADMIN_PASSWORD` in your shell before running this example. Add
provider-key environment variables for the AI features you use. The image runs
as a nonroot user (UID 65532); mounted data must be writable and the license must
be readable by that user. Port 8090 serves the API and app. Port 8091, if configured,
is the private cluster listener and must stay on a trusted network.

Use the in-app documentation for authentication, extraction, resolution, and
cluster setup. `fluree-ai --version` identifies the server build and engine pin;
include it when contacting Fluree support.

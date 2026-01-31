# Changelog

## 0.4.6
- Startup: run OpenClaw git installer only when repo changes or dist entry is missing.

## 0.4.5
- Config: replace update_channel with ref for git ref pinning.
- Startup: fail fast if configured ref is missing.
- Docs: update configuration guidance for ref pinning.

## 0.4.4
- Startup: exit after `openclaw update --no-restart` to simulate restart.

## 0.4.3
- Startup: run OpenClaw git installer if dist entry is missing.

## 0.4.2
- Startup: run OpenClaw git installer on first clone to avoid missing dist on initial boot.

## 0.4.1
- Add update_channel option to select stable/beta/dev via add-on config.

## 0.4.0
- BREAKING: remove ref pinning; rely on `openclaw update` for source updates.
- Startup: run `pnpm openclaw update --no-restart` instead of manual git/pnpm build steps.
- Docs: document update-channel switching via OpenClaw CLI.

## 0.3.0
- Rename add-on to OpenClaw and refresh documentation/metadata.
- Switch persistent data root to /config/openclaw.
- Replace branch option with ref for tag/commit/branch pinning.
- Default upstream repo to openclaw/openclaw.
- Docker: set WORKDIR to /opt/openclaw.

## 0.2.20
- Rename legacy files before directories during migration.

## 0.2.19
- Remove gateway token env var aliasing.

## 0.2.18
- Rename legacy clawd* paths under /config/moltbot on startup.

## 0.2.17
- Export legacy CLAWDBOT_* env aliases for gateway token/config compatibility.

## 0.2.16
- Migrate legacy /config/clawdbot data to /config/moltbot on startup.

## 0.2.15
- Rename legacy references to Moltbot across the add-on.

## 0.2.14
- Add pretty log formatting options for the add-on Log tab.

## 0.2.13
- Add icon.png and logo.png (cyber-lobster mascot).
- Add DOCS.md with detailed documentation.
- Simplify README.md as add-on store intro.
- Follow Home Assistant add-on presentation best practices.

## 0.2.12
- Docker: install Bun runtime.

## 0.2.11
- Docker: install GitHub CLI.
- Storage: persist root home directories under /config/moltbot.
- Docker: refresh base image/toolchain and update gogcli. Thanks @niemyjski! (PR #2)

## 0.2.10
- Fix: remove unsupported pnpm install flag in add-on image.

## 0.2.9
- Install: auto-confirm module purge only when needed.

## 0.2.8
- Install: always reinstall dependencies without confirmation.

## 0.2.7
- Docker: install molthub and Home Assistant CLI.

## 0.2.6
- Auto-restart gateway on unclean exits (e.g., shutdown timeout).

## 0.2.5
- BREAKING: Renamed `repo_ref` to `branch`. Set to track a specific branch; omit to use repo's default.
- Config: `github_token` now uses password field (masked in UI).

## 0.2.4
- Docs: repo-based install steps and add-on info links.
- Docker: set WORKDIR to /opt/moltbot.
- Logs: stream gateway log file into add-on stdout.
- Docker: add ripgrep for faster log searches.

## 0.2.3
- Docs: repo-based install steps and add-on info links.
- Docker: set WORKDIR to /opt/moltbot.
- Logs: stream gateway log file into add-on stdout.

## 0.2.2
- Add HA add-on repository layout and improved SIGUSR1 handling.
- Support pinning upstream refs and clean checkouts.

## 0.2.1
- Ensure gateway.mode=local on first boot.

## 0.2.0
- Initial Home Assistant add-on.

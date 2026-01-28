# Moltbot Gateway Documentation

This add-on runs the Moltbot Gateway on Home Assistant OS, providing secure remote access via SSH tunnel.

## Overview

- **Gateway** runs locally on the HA host (binds to loopback by default)
- **SSH server** provides secure remote access for Moltbot.app or the CLI
- **Persistent storage** under `/config/moltbot` survives add-on updates
- On first start, runs `moltbot setup` to create a minimal config

## Installation

1. In Home Assistant: **Settings → Add-ons → Add-on Store → ⋮ → Repositories**
2. Add: `https://github.com/ngutman/moltbot-ha-addon`
3. Reload the Add-on Store and install **Moltbot Gateway**

## Configuration

### Add-on Options

| Option | Description |
|--------|-------------|
| `ssh_authorized_keys` | Your public key(s) for SSH access (required for tunnels) |
| `ssh_port` | SSH server port (default: `2222`) |
| `port` | Gateway WebSocket port (default: `18789`) |
| `repo_url` | Moltbot source repository URL |
| `branch` | Branch to checkout (uses repo's default if omitted) |
| `github_token` | Token for private repository access |
| `verbose` | Enable verbose logging |
| `log_format` | Log output format in the add-on Log tab: `pretty` or `raw` |
| `log_color` | Enable ANSI colors for pretty logs (may be ignored in the UI) |
| `log_fields` | Comma-separated metadata keys to append (e.g. `connectionId,uptimeMs,runId`) |

### First Run

The add-on performs these steps on startup:

1. Clones or updates the Moltbot repo into `/config/moltbot/moltbot-src`
2. Installs dependencies and builds the gateway
3. Runs `moltbot setup` if no config exists
4. Ensures `gateway.mode=local` if missing
5. Starts the gateway

### Moltbot Configuration

SSH into the add-on and run the configurator:

```bash
ssh -p 2222 root@<ha-host>
cd /config/moltbot/moltbot-src
pnpm moltbot onboard
```

Or use the shorter flow:

```bash
pnpm moltbot configure
```

The gateway auto-reloads config changes. Restart the add-on only if you change SSH keys or build settings:

```bash
ha addons restart local_moltbot
```

## Usage

### SSH Tunnel Access

The gateway listens on loopback by default. Access it via SSH tunnel:

```bash
ssh -p 2222 -N -L 18789:127.0.0.1:18789 root@<ha-host>
```

Then point Moltbot.app or the CLI at `ws://127.0.0.1:18789`.

### Bind Mode

Configure bind mode via the Moltbot CLI (over SSH), not in the add-on options.
Use `pnpm moltbot configure` or `pnpm moltbot onboard` to set it in `moltbot.json`.

## Data Locations

On startup, the add-on migrates `/config/clawdbot` to `/config/moltbot` and renames legacy `clawd*` paths.

| Path | Description |
|------|-------------|
| `/config/moltbot/.moltbot/moltbot.json` | Main configuration |
| `/config/moltbot/.moltbot/agent/auth.json` | Authentication tokens |
| `/config/moltbot/workspace` | Agent workspace |
| `/config/moltbot/moltbot-src` | Source repository |
| `/config/moltbot/.ssh` | SSH keys |
| `/config/moltbot/.config` | App configs (gh, etc.) |

## Included Tools

- **gog** — Google Workspace CLI ([gogcli.sh](https://gogcli.sh))
- **gh** — GitHub CLI ([cli.github.com](https://cli.github.com))
- **molthub** — Skill marketplace CLI
- **hass-cli** — Home Assistant CLI

## Troubleshooting

### SSH doesn't work
Ensure `ssh_authorized_keys` is set in the add-on options with your public key.

### Gateway won't start
Check logs:
```bash
ha addons logs local_moltbot -n 200
```

### Build takes too long
The first boot runs a full build and may take several minutes. Subsequent starts are faster.

## Security Notes

- For `bind=lan/tailnet/auto`, enable gateway auth in `moltbot.json`
- The add-on uses host networking for SSH access
- Consider firewall rules for the SSH port if exposed to LAN

## Links

- [Moltbot](https://github.com/moltbot/moltbot) — Main repository
- [Documentation](https://docs.molt.bot) — Full documentation
- [Community](https://discord.com/invite/molt) — Discord server
- [gog CLI](https://gogcli.sh) — Google Workspace CLI
- [GitHub CLI](https://cli.github.com) — GitHub CLI

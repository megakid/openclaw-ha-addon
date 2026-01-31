# OpenClaw Gateway Documentation

This add-on runs the OpenClaw Gateway on Home Assistant OS, providing secure remote access via SSH tunnel.

## Overview

- **Gateway** runs locally on the HA host (binds to loopback by default)
- **SSH server** provides secure remote access for OpenClaw.app or the CLI
- **Persistent storage** under `/config/openclaw` survives add-on updates
- On first start, runs `openclaw setup` to create a minimal config

## Installation

1. In Home Assistant: **Settings → Add-ons → Add-on Store → ⋮ → Repositories**
2. Add: `https://github.com/megakid/openclaw-ha-addon`
3. Reload the Add-on Store and install **OpenClaw Gateway**

## Configuration

### Add-on Options

| Option | Description |
|--------|-------------|
| `ssh_authorized_keys` | Your public key(s) for SSH access (required for tunnels) |
| `ssh_port` | SSH server port (default: `2222`) |
| `port` | Gateway WebSocket port (default: `18789`) |
| `repo_url` | OpenClaw source repository URL |
| `ref` | Git ref to checkout (branch, tag, or commit) |
| `github_token` | Token for private repository access |
| `verbose` | Enable verbose logging |
| `log_format` | Log output format in the add-on Log tab: `pretty` or `raw` |
| `log_color` | Enable ANSI colors for pretty logs (may be ignored in the UI) |
| `log_fields` | Comma-separated metadata keys to append (e.g. `connectionId,uptimeMs,runId`) |

### First Run

The add-on performs these steps on startup:

1. Clones or updates the OpenClaw repo into `/config/openclaw/openclaw-src`
2. Runs the OpenClaw git installer on first clone or when `dist/entry.js` is missing
3. Bootstraps the OpenClaw CLI if needed
4. Runs `pnpm openclaw update --no-restart` (handles fetch/build/doctor)
5. Exits to simulate restart after update
6. Runs `openclaw setup` if no config exists
7. Ensures `gateway.mode=local` if missing
8. Starts the gateway

### OpenClaw Configuration

SSH into the add-on and run the onboarding wizard:

```bash
ssh -p 2222 root@<ha-host>
cd /config/openclaw/openclaw-src
pnpm openclaw onboard
```

Or use the shorter flow:

```bash
pnpm openclaw configure
```

The gateway auto-reloads config changes. Restart the add-on only if you change SSH keys or build settings:

```bash
ha addons restart local_openclaw_gateway
```

## Usage

### SSH Tunnel Access

The gateway listens on loopback by default. Access it via SSH tunnel:

```bash
ssh -p 2222 -N -L 18789:127.0.0.1:18789 root@<ha-host>
```

Then point OpenClaw.app or the CLI at `ws://127.0.0.1:18789`.

### Bind Mode

Configure bind mode via the OpenClaw CLI (over SSH), not in the add-on options.
Use `pnpm openclaw configure` or `pnpm openclaw onboard` to set it in `openclaw.json`.

If you bind beyond loopback (`lan/tailnet/auto`), ensure gateway authentication is configured in `openclaw.json`.

### Ref Pinning

Set `ref` in the add-on options to a branch, tag, or commit SHA. The ref must exist in the remote repository or the add-on will exit with an error. Restart the add-on to apply changes.

## Data Locations

| Path | Description |
|------|-------------|
| `/config/openclaw/.openclaw/openclaw.json` | Main configuration |
| `/config/openclaw/.openclaw/agent/auth.json` | Authentication tokens |
| `/config/openclaw/.openclaw/workspace` | Agent workspace |
| `/config/openclaw/openclaw-src` | Source repository |
| `/config/openclaw/.ssh` | SSH keys |
| `/config/openclaw/.config` | App configs (gh, etc.) |

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
ha addons logs local_openclaw_gateway -n 200
```

### Build takes too long
The first boot runs a full build and may take several minutes. Subsequent starts are faster.

## Security Notes

- For `bind=lan/tailnet/auto`, enable gateway auth in `openclaw.json`
- The add-on uses host networking for SSH access
- Consider firewall rules for the SSH port if exposed to LAN

## Links

- [OpenClaw](https://github.com/openclaw/openclaw) — Main repository
- [Documentation](https://docs.openclaw.ai) — Full documentation
- [Website](https://openclaw.ai)

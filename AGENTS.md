# Repository Guidelines
- Repo: https://github.com/ngutman/moltbot-ha-addon

## Project Structure & Files
- Add-on source and metadata live in `moltbot_gateway/`.
- Add-on metadata: `moltbot_gateway/config.json` (version, options, ingress settings).
- Docs: `moltbot_gateway/README.md`, `moltbot_gateway/DOCS.md`.
- Changelog: `moltbot_gateway/CHANGELOG.md` (latest release at top, no `Unreleased`).

## Commit Messages
- Follow conventional commit format: `type(scope): short description`.
- Keep messages concise and action-oriented (imperative, lowercase, no trailing period).
- Example: `feat(gateway): add verbose flag to send`.
- Group related changes; avoid bundling unrelated refactors.

## Pull Requests & Approval
- New features and bugfixes must be opened as GitHub PRs.
- PRs should summarize scope, note testing performed, and mention any user-facing changes or new flags.
- Wait for approval from `ngutman` before merging.

## Docs & Data Safety
- When changing add-on behavior or options, update docs and the changelog.
- Do not commit secrets or real user data; use obvious placeholders.

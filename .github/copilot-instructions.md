## Learn GitHub Copilot CLI

This is a sandbox repository for learning and experimenting with the GitHub Copilot CLI.

## Repository Structure

- `.github/hooks/hooks.json` — Copilot CLI session hooks configuration (version 1 format)

## Hooks Configuration

The `hooks.json` file at `.github/hooks/hooks.json` defines lifecycle hooks for Copilot CLI sessions. The current hook runs on `sessionStart` and appends a timestamp entry to `logs/session.log`.

Hook schema:
- `version`: must be `1`
- `hooks.<event>`: array of hook objects
- Each hook object has `bash` and/or `powershell` command strings, optional `cwd`, and optional `timeoutSec`

Supported events: `sessionStart`

The `logs/` directory is expected at the repo root (created at runtime by the sessionStart hook).

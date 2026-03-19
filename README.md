# learn-gh-copilot

A sandbox repository for learning and experimenting with the [GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli/about-github-copilot-in-the-cli).

This repo demonstrates key Copilot CLI features including lifecycle hooks, custom agent definitions, and workspace instructions — providing a hands-on playground to explore how the CLI can be configured and extended.

---

## Prerequisites

| Requirement | Notes |
|---|---|
| [GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli/about-github-copilot-in-the-cli) | Must be installed and authenticated |
| A GitHub account with Copilot access | Required for CLI authentication |
| PowerShell **or** Bash | The session hook runs on either shell |

> **Note:** There are no language runtimes, package managers, or build tools required — this is a pure configuration/learning repository.

---

## Project Structure

```
learn-gh-copilot/
├── .github/
│   ├── agents/
│   │   └── readme-expert.agent.md    # Custom agent definition (readme-expert)
│   ├── hooks/
│   │   └── hooks.json                # Copilot CLI lifecycle hooks (version 1)
│   └── copilot-instructions.md       # Workspace-level Copilot instructions
└── logs/                             # Created at runtime by the sessionStart hook
    └── session.log                   # Timestamped log of each Copilot CLI session
```

> The `logs/` directory and `logs/session.log` file are **created automatically** the first time the sessionStart hook runs. They are not committed to the repository.

---

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/matthewcanning/learn-gh-copilot.git
cd learn-gh-copilot
```

### 2. Open a Copilot CLI session

Launch the GitHub Copilot CLI in this directory:

```bash
gh copilot
```

When the session starts, the `sessionStart` hook fires automatically and appends a timestamped entry to `logs/session.log`.

### 3. Verify the session log

After starting a session, confirm the hook ran successfully:

```powershell
# PowerShell
Get-Content logs/session.log
```

```bash
# Bash
cat logs/session.log
```

Expected output:

```
Session started: 03/19/2026 21:21:38
Session started: 03/19/2026 21:25:40
...
```

---

## Configuration

### Lifecycle Hooks — `.github/hooks/hooks.json`

Hooks let you run shell commands automatically at key points in the Copilot CLI session lifecycle.

```json
{
    "version": 1,
    "hooks": {
        "sessionStart": [
            {
                "type": "command",
                "bash": "echo \"Session started: $(date)\" >> logs/session.log",
                "powershell": "Add-Content -Path logs/session.log -Value \"Session started: $(Get-Date)\"",
                "cwd": ".",
                "timeoutSec": 10
            }
        ]
    }
}
```

**Hook schema reference:**

| Field | Type | Description |
|---|---|---|
| `version` | `number` | Must be `1` |
| `hooks.<event>` | `array` | List of hook objects for the named lifecycle event |
| `type` | `string` | Hook type — currently `"command"` |
| `bash` | `string` | Shell command to run on Linux/macOS |
| `powershell` | `string` | PowerShell command to run on Windows |
| `cwd` | `string` | Working directory for the command (relative to repo root) |
| `timeoutSec` | `number` | Maximum seconds the command may run before being killed |

**Supported lifecycle events:**

| Event | Fires when… |
|---|---|
| `sessionStart` | A new Copilot CLI session begins in this workspace |

### Workspace Instructions — `.github/copilot-instructions.md`

This file provides persistent, workspace-scoped context that is automatically included in every Copilot CLI session. Use it to describe the project, define conventions, or give Copilot standing instructions specific to this repository.

### Custom Agents — `.github/agents/`

Agent definition files (`.agent.md`) live in `.github/agents/` and are picked up automatically by the Copilot CLI. Each file defines a named agent with a system prompt and behaviour description.

**Included agent:**

| Agent | File | Purpose |
|---|---|---|
| `readme-expert` | `readme-expert.agent.md` | Specialist agent for creating and updating README files |

Invoke a custom agent in a session by referencing its name (e.g. `@readme-expert`).

---

## Experimenting & Learning

This repository is intentionally minimal — it is a starting point. Some things to try:

- **Add new hooks** — e.g. a `sessionEnd` event to record when sessions close (once supported).
- **Extend `copilot-instructions.md`** — add domain-specific context, coding conventions, or project notes.
- **Create new agents** — add `.agent.md` files to `.github/agents/` to build specialised assistants.
- **Edit the sessionStart hook** — log additional metadata such as the working directory, git branch, or machine hostname.

---

## Session Log

`logs/session.log` is a plain-text append-only file managed entirely by the `sessionStart` hook. It records a timestamp for every Copilot CLI session opened in this workspace.

The file and its parent directory are created automatically on first use — no manual setup is needed.

---

## Resources

- [GitHub Copilot CLI documentation](https://docs.github.com/en/copilot/github-copilot-in-the-cli/about-github-copilot-in-the-cli)
- [Customising Copilot CLI with hooks](https://docs.github.com/en/copilot)
- [GitHub Copilot for the CLI — getting started](https://docs.github.com/en/copilot/github-copilot-in-the-cli/using-github-copilot-in-the-cli)

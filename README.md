[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue)](https://www.typescriptlang.org/)
[![Git](https://img.shields.io/badge/Git-backed-orange)](https://git-scm.com/)
[![License](https://img.shields.io/badge/license-MIT-brightgreen)](./LICENSE)

# Persistent memory for the pi coding agent

## How to Install

```bash
pi install git:github.com/dapovoa/pi-memory
```

Open `~/.pi/agent/settings.json` and add:

```json
{
  "pi-memory": {
    "memoryDir": {
      "repoUrl": "git@github.com:username/repo.git"
    }
  }
}
```

Then run inside pi:

```
/skill:memory-init
```

## Use

At the start of each session, pi pulls the repo, reads the `core/*.md` files, and summarizes the context for the LLM. If details are needed, the LLM reads the full file on demand.


## Commands

- `/skill:memory-init` — clone the repo and create the directory structure
- `/memory-status` — show git status
- `/memory-refresh` — rebuild and resend the context
- `/memory-check` — print the directory tree
- `/memory-anchor` — create a session anchor


## Tools

- `memory_write` — create or edit a file
- `memory_list` — list files
- `memory_search` — search by tags, description, or text
- `memory_sync` — `pull`, `push`, or `status`
- `memory_check` — print the directory tree


## Structure

Each project lives under `~/.pi/memory-md/`:

```
~/.pi/memory-md/
└── {project}/
    └── core/
        ├── USER.md
        ├── MEMORY.md
        ├── TASK.md
        └── project/
```


## File format

```markdown
---
description: "What this file contains"
tags: ["project", "architecture"]
---

# Content
```


## Tape mode (optional)

To let pi automatically pick context based on session activity, enable it in `settings.json`:

```json
{
  "pi-memory": {
    "tape": {
      "enabled": true,
      "anchor": {
        "keywords": { "project": ["refactor", "deploy"] }
      }
    }
  }
}
```

Tape tools:

- `tape_handoff` — create a checkpoint
- `tape_list` — list anchors
- `tape_read` — read session entries
- `tape_search` — search entries and anchors
- `tape_info` — show stats
- `tape_delete` — delete an anchor
- `tape_reset` — clear all anchors


## Common settings

| Key | Default | Description |
|---|---|---|
| `memoryDir.repoUrl` | — | required |
| `memoryDir.localPath` | `~/.pi/memory-md` | local storage path |
| `delivery` | `message-append` | `message-append` or `system-prompt` |
| `hooks.sessionStart` | `["pull"]` | actions on session start |
| `hooks.sessionEnd` | `[]` | add `"push"` for auto-commit on exit |
| `tape.enabled` | `false` | enable tape mode |
| `tape.context.strategy` | `smart` | `smart` or `recent-only` |
| `tape.context.fileLimit` | `10` | file limit |
| `tape.anchor.mode` | `auto` | `auto` or `manual` |

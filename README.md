# Kimi WebBridge — AI Agent Skill

This repository is a backup of the [Kimi WebBridge](https://www.kimi.com/features/webbridge) AI agent skill.

Kimi WebBridge is a browser extension that lets AI agents control your real browser — navigate, click, fill forms, take screenshots, and read web content using your existing login sessions.

> 中文版說明請見 [`README.zh.md`](README.zh.md)

## About This Skill

This skill is the official `SKILL.md` distributed by the Kimi WebBridge installer (`install.ps1` / `install.sh` from `cdn.kimi.com/webbridge/`). It teaches the AI agent how to control the browser via the local daemon's API. This backup preserves a specific version for easy access across machines.

## Directory Structure

```
kimi-webbridge/                ← skill folder, copy directly to your agent's skills directory
├── SKILL.md                   ← main skill definition (tools, usage, notes)
└── references/
    └── operations.md          ← install, start, diagnose guide
```

## Prerequisites

Install [Kimi WebBridge](https://chromewebstore.google.com/detail/kimi-webbridge/fldmhceldgbpfpkbgopacenieobmligc) from Chrome Web Store.

If you cannot access the store, install manually:
1. Download the extension package from the [Kimi WebBridge official page](https://www.kimi.com/features/webbridge)
2. Open `chrome://extensions/` and enable **Developer mode**
3. Click **Load unpacked** and select the extracted folder

## Quick Usage

Once the daemon is running, call it directly with curl:

```bash
# Health check
~/.kimi-webbridge/bin/kimi-webbridge status

# Navigate (open new tab) and take a screenshot
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"navigate","args":{"url":"https://example.com","newTab":true}}'

# Read page content (accessibility tree)
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"snapshot","args":{}}'

# Screenshot
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"screenshot","args":{}}'

# Click an element (@e ref from snapshot)
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"click","args":{"selector":"@e123"}}'

# Fill an input
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"fill","args":{"selector":"@e456","value":"hello"}}'
```

For the full tool list see [`kimi-webbridge/SKILL.md`](kimi-webbridge/SKILL.md).

## Installation

### Official Install

```bash
# macOS / Linux
curl -fsSL https://cdn.kimi.com/webbridge/install.sh | bash

# Windows (PowerShell)
irm https://cdn.kimi.com/webbridge/install.ps1 | iex
```

### Manual Install (from this backup)

Copy the `kimi-webbridge/` folder to your AI agent's skills path:

| Agent | Path |
|-------|------|
| OpenCode / Claude Code | `~/.claude/skills/kimi-webbridge/` |
| Kimi Code | `~/.kimi-code/skills/kimi-webbridge/` |
| Generic agent | `~/.agents/skills/kimi-webbridge/` |

## Version

This backup corresponds to the skill version installed by the official install script. The daemon, extension, and skill share the same version string — check it via `~/.kimi-webbridge/bin/kimi-webbridge status`.

## Links

- [Kimi WebBridge official page](https://www.kimi.com/features/webbridge)
- [Kimi WebBridge Help Center](https://www.kimi.com/help/kimi-webbridge)
- [Moonshot AI](https://www.moonshot.ai/)

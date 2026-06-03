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

## For AI Agents — Installation Prompt

If you are an AI agent (e.g. Claude Code, OpenCode, Kimi Code, or any coding agent) and the user wants you to install Kimi WebBridge, use the following steps:

> **Copy-paste this block into your agent's context to let it install automatically:**

```
Please install the Kimi WebBridge skill so I can control my browser.

Steps:
1. Run the official installer for my OS:
   - macOS / Linux: curl -fsSL https://cdn.kimi.com/webbridge/install.sh | bash
   - Windows (PowerShell): irm https://cdn.kimi.com/webbridge/install.ps1 | iex

2. If the CDN is unreachable, use the offline backup from:
   https://github.com/leoshome/kimi-webbridge-skill

   - Download the ZIP, extract it, and move:
     a. `.kimi-webbridge/` → `%USERPROFILE%\.kimi-webbridge\` (Windows) or `~/.kimi-webbridge/` (macOS/Linux)
     b. `kimi-webbridge/` skill folder → your agent's skills directory:
        - Claude Code / OpenCode: ~/.claude/skills/kimi-webbridge/
        - Kimi Code: ~/.kimi-code/skills/kimi-webbridge/
        - Generic agent: ~/.agents/skills/kimi-webbridge/

3. Add `~/.kimi-webbridge/bin` (or `%USERPROFILE%\.kimi-webbridge\bin` on Windows) to PATH.

4. Start and verify the daemon:
   kimi-webbridge start
   kimi-webbridge status
   curl http://127.0.0.1:10086/health

5. Install the Chrome extension from:
   https://chromewebstore.google.com/detail/kimi-webbridge/fldmhceldgbpfpkbgopacenieobmligc

After setup, load the skill file at kimi-webbridge/SKILL.md to learn the available browser-control tools.
```

### What the agent will be able to do after installation

| Action | API call |
|--------|----------|
| Open a URL in a new tab | `navigate` |
| Read page accessibility tree | `snapshot` |
| Take a screenshot | `screenshot` |
| Click an element | `click` |
| Fill an input field | `fill` |
| Scroll the page | `scroll` |
| Run JavaScript | `evaluate` |

For the full tool list see [`kimi-webbridge/SKILL.md`](kimi-webbridge/SKILL.md).

## Windows Binary Backup

This repo includes a backup of the `kimi-webbridge` daemon binary for offline use when the official installer is unavailable (no PowerShell, no internet, CDN down, etc.). Only the Windows `amd64` build is backed up here.

```
.kimi-webbridge/
  bin/
    kimi-webbridge.exe      ← Windows amd64 binary
```

### Version

| Field      | Value |
|------------|-------|
| Version    | **v1.9.16** |
| Source     | `https://cdn.kimi.com/webbridge/v1.9.16/releases/kimi-webbridge-windows-amd64.exe` |
| Size       | 10,223,616 bytes (~10 MB) |
| SHA256     | `7cf38d2c3dfc8365ec9fb351624f7a8fc7681a76f129cd8bbcdf0127be4ff1af` |
| Downloaded | 2026-06-02 |

### How to use this backup

1. **Copy the binary to `%USERPROFILE%\.kimi-webbridge\bin` and add to user `PATH`**:

   ```powershell
   # Clone the repository to a temporary folder
   git clone --depth 1 https://github.com/leoshome/kimi-webbridge-skill.git "$env:TEMP\kimi-webbridge-skill"
   # Move the .kimi-webbridge folder to the user home folder
   Move-Item -Path "$env:TEMP\kimi-webbridge-skill\.kimi-webbridge" -Destination "$env:USERPROFILE\" -Force
   # Clean up the cloned repository folder
   Remove-Item -Path "$env:TEMP\kimi-webbridge-skill" -Recurse -Force
   # Add the bin directory to user PATH
   $bin = "$env:USERPROFILE\.kimi-webbridge\bin"
   [Environment]::SetEnvironmentVariable("Path", ([Environment]::GetEnvironmentVariable("Path", "User") + ";$bin"), "User")
   $env:Path += ";$bin"
   ```

2. **Move the skill folder** (`kimi-webbridge/`) into your agent's skills directory — pick the one that matches your agent:
   | Agent                  | Destination path |
   |------------------------|------------------|
   | OpenCode / Claude Code | `%USERPROFILE%\.claude\skills\kimi-webbridge\` |
   | Kimi Code              | `%USERPROFILE%\.kimi-code\skills\kimi-webbridge\` |
   | Generic agent          | `%USERPROFILE%\.agents\skills\kimi-webbridge\` |

4. **Start the daemon** and verify:

   ```powershell
   kimi-webbridge start
   kimi-webbridge status
   curl http://127.0.0.1:10086/health
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

# Kimi WebBridge — AI Agent Skill

本倉庫是 [Kimi WebBridge](https://www.kimi.com/features/webbridge) 的 AI agent skill 備份。

Kimi WebBridge 是一個瀏覽器擴充功能，讓 AI agent 能控制你的真實瀏覽器 — 導航、點擊、輸入、截圖、讀取網頁內容，使用你已有的登入狀態。

> English version: [`README.md`](README.md)

## 關於此 Skill

此 skill 是 Kimi WebBridge 安裝腳本（`install.ps1` / `install.sh`，來自 `cdn.kimi.com/webbridge/`）內附的官方 `SKILL.md`。它告訴 AI agent 如何透過本地 daemon 的 API 控制瀏覽器。此備份保存特定版本，方便跨機器取用。

## 目錄結構

```
kimi-webbridge/                ← skill 資料夾，可直接複製到 agent 的 skills 目錄
├── SKILL.md                   ← 主 skill 定義（工具、用法、注意事項）
└── references/
    └── operations.md          ← 安裝、啟動、診斷手冊
```

## 前置需求

從 Chrome Web Store 安裝 [Kimi WebBridge](https://chromewebstore.google.com/detail/kimi-webbridge/fldmhceldgbpfpkbgopacenieobmligc)。

如果無法存取商店，也可手動安裝：
1. 從 [Kimi WebBridge 官方頁面](https://www.kimi.com/features/webbridge) 下載擴充功能套件
2. 開啟 `chrome://extensions/` 並開啟**開發人員模式**
3. 點擊**載入未封裝項目**，選擇解壓縮後的資料夾

## 快速用法

Daemon 啟動後，直接用 curl 呼叫 API：

```bash
# 健康檢查
~/.kimi-webbridge/bin/kimi-webbridge status

# 導航（開新分頁）並截圖
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"navigate","args":{"url":"https://example.com","newTab":true}}'

# 讀取頁面內容（accessibility tree）
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"snapshot","args":{}}'

# 截圖
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"screenshot","args":{}}'

# 點擊元素（@e ref 來自 snapshot）
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"click","args":{"selector":"@e123"}}'

# 填寫輸入框
curl -X POST http://127.0.0.1:10086/command \
  -d '{"action":"fill","args":{"selector":"@e456","value":"hello"}}'
```

完整工具列表請見 [`kimi-webbridge/SKILL.md`](kimi-webbridge/SKILL.md)。

## 安裝方式

### 原始安裝（官方）

```bash
# macOS / Linux
curl -fsSL https://cdn.kimi.com/webbridge/install.sh | bash

# Windows (PowerShell)
irm https://cdn.kimi.com/webbridge/install.ps1 | iex
```

### 手動使用此備份

將 `kimi-webbridge/` 目錄複製到你的 AI agent skills 路徑：

| Agent | 路徑 |
|-------|------|
| OpenCode / Claude Code | `~/.claude/skills/kimi-webbridge/` |
| Kimi Code | `~/.kimi-code/skills/kimi-webbridge/` |
| 通用 agent | `~/.agents/skills/kimi-webbridge/` |

## 版本

此備份對應官方安裝腳本安裝的 skill 版本。daemon、extension 與 skill 共用同一個版本字串，可透過 `~/.kimi-webbridge/bin/kimi-webbridge status` 查看。

## 相關連結

- [Kimi WebBridge 官方頁面](https://www.kimi.com/features/webbridge)
- [Kimi WebBridge 幫助中心](https://www.kimi.com/help/kimi-webbridge)
- [Moonshot AI](https://www.moonshot.ai/)

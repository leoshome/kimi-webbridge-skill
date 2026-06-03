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

## For AI Agents — 安裝 Prompt

如果你是 AI agent（例如 Claude Code、OpenCode、Kimi Code 或任何 coding agent），使用者希望你安裝 Kimi WebBridge，請按照以下步驟操作：

> **將以下區塊貼入你的 agent 對話，讓它自動完成安裝：**

```
請幫我安裝 Kimi WebBridge skill，讓我可以用 AI agent 控制瀏覽器。

步驟如下：
1. 依照我的作業系統執行官方安裝腳本：
   - macOS / Linux：curl -fsSL https://cdn.kimi.com/webbridge/install.sh | bash
   - Windows（PowerShell）：irm https://cdn.kimi.com/webbridge/install.ps1 | iex

2. 如果 CDN 無法存取，請改用離線備份：
   https://github.com/leoshome/kimi-webbridge-skill

   - 下載 ZIP 並解壓縮，然後搬移：
     a. `.kimi-webbridge/` → `%USERPROFILE%\.kimi-webbridge\`（Windows）或 `~/.kimi-webbridge/`（macOS/Linux）
     b. `kimi-webbridge/` skill 資料夾 → 對應 agent 的 skills 目錄：
        - Claude Code / OpenCode：~/.claude/skills/kimi-webbridge/
        - Kimi Code：~/.kimi-code/skills/kimi-webbridge/
        - 通用 agent：~/.agents/skills/kimi-webbridge/

3. 將 `~/.kimi-webbridge/bin`（Windows 為 `%USERPROFILE%\.kimi-webbridge\bin`）加入 PATH。

4. 啟動並確認 daemon：
   kimi-webbridge start
   kimi-webbridge status
   curl http://127.0.0.1:10086/health

5. 安裝 Chrome 擴充功能：
   https://chromewebstore.google.com/detail/kimi-webbridge/fldmhceldgbpfpkbgopacenieobmligc

安裝完成後，載入 kimi-webbridge/SKILL.md 以了解所有可用的瀏覽器控制工具。
```

### 安裝後 agent 可執行的操作

| 操作 | API 動作 |
|------|----------|
| 開啟網址（新分頁） | `navigate` |
| 讀取頁面 accessibility tree | `snapshot` |
| 截圖 | `screenshot` |
| 點擊元素 | `click` |
| 填寫輸入框 | `fill` |
| 捲動頁面 | `scroll` |
| 執行 JavaScript | `evaluate` |

完整工具列表請見 [`kimi-webbridge/SKILL.md`](kimi-webbridge/SKILL.md)。

## Windows 二進位備份

本倉庫包含 `kimi-webbridge` daemon 二進位檔案的備份，方便在無法使用官方安裝腳本時（沒 PowerShell、沒網路、CDN 掛了等）離線使用。此備份只包含 Windows `amd64` 版本。

```
.kimi-webbridge/
  bin/
    kimi-webbridge.exe      ← Windows amd64 二進位
```

### 版本

| 欄位       | 值 |
|------------|---|
| 版本       | **v1.9.16** |
| 來源       | `https://cdn.kimi.com/webbridge/v1.9.16/releases/kimi-webbridge-windows-amd64.exe` |
| 大小       | 10,223,616 bytes（約 10 MB） |
| SHA256     | `7cf38d2c3dfc8365ec9fb351624f7a8fc7681a76f129cd8bbcdf0127be4ff1af` |
| 下載日期   | 2026-06-02 |

> CDN 只保留最近的版本，所有 v1.9.5 以前的版本（包括 v1.0.0、v1.8.0、整個 v0.x 系列）皆回傳 404。此備份保留 v1.9.16 確保可穩定取用。

### 如何使用此備份

> 需 PowerShell 5+ 與網路（**不需要 git**）。以下路徑皆為絕對路徑，可從任何目錄執行。

1. **下載 repo ZIP**：從 `https://github.com/leoshome/kimi-webbridge-skill/archive/refs/heads/master.zip` 下載並解壓縮到暫存目錄（例如 `%TEMP%\kimi-webbridge-skill-master`）。
2. **搬移 binary 資料夾**：把解壓縮後的 `.kimi-webbridge/` 整個資料夾搬到使用者目錄（`%USERPROFILE%`），最終位置是 `C:\Users\<你的帳號>\.kimi-webbridge\`，裡面的 `bin\kimi-webbridge.exe` 就是 daemon 主程式。
3. **搬移 skill 資料夾**：把 `kimi-webbridge/` 資料夾搬到對應的 agent skills 目錄（依你的 agent 擇一）：
   | Agent                  | 目標路徑 |
   |------------------------|---------|
   | OpenCode / Claude Code | `%USERPROFILE%\.claude\skills\kimi-webbridge\` |
   | Kimi Code              | `%USERPROFILE%\.kimi-code\skills\kimi-webbridge\` |
   | 通用 agent              | `%USERPROFILE%\.agents\skills\kimi-webbridge\` |
4. **把 binary 資料夾加入 PATH**：在「使用者環境變數」（非系統）的 `Path` 附加 `%USERPROFILE%\.kimi-webbridge\bin`，讓 `kimi-webbridge` 在任何目錄都能執行。
5. **啟動 daemon**：執行 `kimi-webbridge start`，然後用 `kimi-webbridge status`（或 `curl http://127.0.0.1:10086/health`）驗證。
6. **清理**下載的 ZIP 與解壓縮的暫存資料夾（選擇性）。

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

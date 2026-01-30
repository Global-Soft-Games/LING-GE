# MCP 連線設定教學

靈閣透過 **MCP (Model Context Protocol)** 協議與 AI 工具整合。本文件說明如何設定連線。

---

## 什麼是 MCP？

MCP (Model Context Protocol) 是由 Anthropic 開發的開放標準協議，讓 AI 工具能夠：

- 🔌 連接外部服務（如靈閣）
- 🛠️ 呼叫工具與 API
- 📚 存取專業知識庫
- 🤖 擴展 AI 的能力

**官方文件：** https://modelcontextprotocol.io

---

## 前置準備

### 1. 註冊靈閣帳號

前往 [https://lingge.app](https://lingge.app) 註冊並完成 TOTP 雙重認證設定。

### 2. 取得 MCP Server URL

登入後，在 Dashboard → MCP 設定頁面，你會看到：

```
MCP Server URL: https://mcp.lingge.app
```

> ⚠️ **重要**：URL 是公開的，但需要透過 OAuth 認證才能使用服務。

---

## ChatGPT Desktop 設定

### 支援版本

- ChatGPT Desktop App (macOS / Windows)
- 需要 ChatGPT Plus 或 Pro 訂閱

### 設定步驟

#### 1. 開啟 MCP 設定

在 ChatGPT Desktop App 中：

1. 點擊左下角的 **個人頭像**
2. 選擇 **Settings** (設定)
3. 點擊 **Integrations** (整合)
4. 找到 **MCP Servers** 區塊
5. 點擊 **Add Connector** (新增連接器)

#### 2. 輸入 MCP Server 資訊

在彈出視窗中填入：

| 欄位 | 值 |
|------|------|
| **Name** | 靈閣 (Líng Gé) |
| **URL** | `https://mcp.lingge.app` |
| **Authentication** | OAuth 2.0 |

#### 3. 選擇 OAuth 認證

1. 選擇 **OAuth** 認證方式
2. 點擊 **Connect** (連線)
3. 瀏覽器會自動開啟靈閣的 OAuth 授權頁面

#### 4. 完成 OAuth 授權

在授權頁面：

1. 使用你的靈閣帳號登入（Email + TOTP 驗證碼）
2. 確認授權範圍：
   - 召喚智靈
   - 查看額度
3. 點擊 **授權 (Authorize)**
4. 完成後，瀏覽器會自動跳回 ChatGPT

#### 5. 驗證連線

回到 ChatGPT Desktop App，你應該會看到：

```
✅ 靈閣 (Líng Gé) - Connected
```

### 開始使用

現在你可以在 ChatGPT 中直接召喚靈閣智靈：

```
「用敏捷開發的思維幫我規劃婚禮」
```

ChatGPT 會自動透過 MCP 呼叫靈閣服務。

---

## Claude Code 設定

### 支援版本

- Claude Code CLI (需要 Claude Pro 訂閱)

### 設定步驟

#### 1. 編輯 MCP 設定檔

Claude Code 使用 JSON 設定檔管理 MCP Servers。

**macOS/Linux:**
```bash
~/.claude/mcp_servers.json
```

**Windows:**
```
%APPDATA%\.claude\mcp_servers.json
```

#### 2. 新增靈閣 MCP Server

在設定檔中加入：

```json
{
  "mcpServers": {
    "lingge": {
      "command": "npx",
      "args": [
        "-y",
        "@anthropic-ai/mcp-client",
        "https://mcp.lingge.app"
      ],
      "auth": {
        "type": "oauth",
        "authorizationUrl": "https://api.lingge.app/oauth/authorize",
        "tokenUrl": "https://api.lingge.app/oauth/token"
      }
    }
  }
}
```

> 💡 **說明**：
> - `command` 和 `args` 告訴 Claude Code 如何連接 MCP Server
> - `auth` 設定啟用 OAuth 認證流程

#### 3. 重啟 Claude Code

```bash
# 重啟 Claude Code CLI
claude code restart
```

#### 4. 完成 OAuth 授權

首次使用時，Claude Code 會提示你完成 OAuth 授權：

1. 終端機會顯示授權 URL
2. 在瀏覽器中開啟該 URL
3. 使用靈閣帳號登入（Email + TOTP）
4. 授權完成後，回到終端機繼續

#### 5. 驗證連線

在 Claude Code 中執行：

```bash
claude mcp list
```

你應該會看到：

```
✅ lingge - Connected
```

### 開始使用

在 Claude Code 中，你可以：

```
「呼叫靈閣，用敏捷開發思維幫我規劃專案」
```

或直接在對話中提到相關概念，Claude Code 會自動匹配智靈。

---

## 常見問題

### Q: OAuth 授權會過期嗎？

是的。OAuth Token 會定期過期（通常 30-90 天），屆時需要重新授權。系統會自動提示你。

### Q: 我可以同時在 ChatGPT 和 Claude Code 中使用嗎？

可以！同一個靈閣帳號可以在多個平台上授權使用。

### Q: 如何取消授權？

在靈閣 Dashboard → MCP 設定頁面，點擊「已連結帳號」旁的「解除連結」按鈕。

### Q: 設定後看不到任何智靈？

請檢查：

1. ✅ MCP Server URL 是否正確：`https://mcp.lingge.app`
2. ✅ OAuth 授權是否成功完成
3. ✅ 帳號額度是否充足（至少需要 1 次額度）
4. ✅ 網路連線是否正常

如果問題持續，請聯繫 support@lingge.app

### Q: 為什麼需要 OAuth 認證？

OAuth 2.0 是業界標準的授權協議，優點：

- ✅ **不洩漏密碼**：不需要把靈閣密碼給 ChatGPT/Claude
- ✅ **可撤銷授權**：隨時可以在靈閣 Dashboard 取消授權
- ✅ **安全性高**：Token 加密儲存，且會定期過期

詳細說明：[OAUTH_FLOW.md](./OAUTH_FLOW.md)

---

## 進階設定

### 自訂 Timeout

如果召喚智靈時出現 Timeout，可以調整 MCP 設定：

**Claude Code (`mcp_servers.json`):**

```json
{
  "mcpServers": {
    "lingge": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-client", "https://mcp.lingge.app"],
      "auth": { "type": "oauth" },
      "timeout": 30000
    }
  }
}
```

**ChatGPT Desktop:**

目前 ChatGPT Desktop 的 Timeout 由系統自動管理，無法自訂。

---

## 下一步

- 🔐 [OAuth 認證流程說明](./OAUTH_FLOW.md)
- ❓ [常見問題 FAQ](./FAQ.md)
- 📖 [快速開始指南](./GETTING_STARTED.md)

---

## 需要協助？

- 📧 Email: support@lingge.app
- 🌐 官網: [https://lingge.app](https://lingge.app)

---

> **靈閣 (Líng Gé)** - 借他山之石，攻你心中玉
>
> © 2026 古蘿蔔遊戲 Global-soft Games

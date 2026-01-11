# Wazuh AI Command Center: 基於 MCP 協議之對話式威脅狩獵系統

> **實作成果展示**：本專案成功實作了透過 **AI Agent (Claude)** 結合 **Model Context Protocol (MCP)** 協議，對 **Wazuh SIEM** 進行自然語言導向的資安防護監測與威脅狩獵。

---
*   **核心技術**：Model Context Protocol (MCP)
*   **目標**：讓管理員透過簡單的 **Chat (對話)** 即可獲取實時資產清查、漏洞分析與威脅報告。

---

## 系統架構 (Architecture)
系統採用全容器化架構：

1.  **Wazuh Stack**: 
    *   使用 **Docker** 部署 Wazuh Indexer 與 Manager，作為數據採集與儲存的核心。
2.  **Wazuh MCP Server**: 
    *   以 **Rust** 語言實作的高效能 MCP 伺服器，將 Wazuh REST API 轉化為 AI 可直接調用的 **Function Call**。
3.  **Claude AI**: 
    *   透過 Claude Desktop 整合，將 LLM 賦予「理解資安情資」的能力，實現自動化分析與推理。

---

## 技術實現細節 (Technical Implementation)

### 1. 容器化工具鏈開發 (Dockerfile)
為確保 AI 調用工具的效能，採用了 **Rust 多階段編譯 (Multi-stage Build)**：
*   **編譯環境**：`rust:1.86-slim`，負責編譯 MCP 原始碼並處理系統依賴。
*   **運行環境**：`debian:bookworm-slim`，僅保留二進位執行檔，達成映像檔輕量化，顯著降低 AI 喚醒延遲。

### 2. 跨容器連動與 API 配置
*   **網路連動**：實作 Docker 自定義網路橋接，確保 MCP Server 容器能安全地存取 `wazuh-manager` 的 API 端口 (55000)。
*   **認證機制**：解決了 401 Unauthorized 驗證問題，透過環境變數注入 API 憑證，確保 AI 獲取日誌的合法性。

### 3. AI 工具調用邏輯 (Function Calling)
*   實作 `get_wazuh_agents`、`get_wazuh_alerts` 等核心工具。
*   讓 AI 具備「過濾大數據日誌」的能力，將 JSON 數據轉化為結構化的中文資安建議。

---

## 實作功能展示 (Core Features)

### 自然語言資產清查 (Asset Inventory)
*   **指令**：「幫我列出目前所有 Wazuh Agents 的連線狀態，有沒有機器掉線？」
*   **成果**：AI 即時調用 API 工具，回報主機在線清單與 IP 位址，無需手動搜尋 Dashboard。

### 對話式威脅狩獵 (Threat Hunting via Chat)
*   **指令**：「分析最近一小時內 Level 10 以上的嚴重告警，並總結攻擊行為。」
*   **成果**：AI 自動過濾大數據日誌，主動識別出 **CIS Benchmark 合規性弱點**，並分析出 SSH 權限與密碼策略風險。

### 智能合規性建議
*   **成果**：根據偵測到的合規性失敗項目，AI 會自動產出對應的 **Bash 修復指令**，展現 AI Agent 作為資安大腦的決策能力。

---

## 實作截圖證明 (Evidence)

### AI 成功調用底層工具並進行數據分析
<img width="1919" height="1019" alt="Wazuh Tool Invocation" src="https://github.com/user-attachments/assets/dc799d28-af76-494c-a34f-ef1cb7ac4a23" />
*描述：展示 Claude Agent 透過 MCP 進行實時威脅判讀。*

### 資安漏洞總結與合規性分析
<img width="1919" height="1015" alt="Compliance Review" src="https://github.com/user-attachments/assets/c711d52b-ceaa-47d9-be41-d911e8e9ccc5" />
*描述：展示 AI 將複雜的數據轉化為具體的修復建議報告。*

---

## 技術堆疊 (Tech Stack)
*   **SIEM**: Wazuh (Indexer, Manager, Dashboard)
*   **Bridge**: Wazuh MCP Server (Rust-based)
*   **AI Engine**: Claude Desktop (Agent Mode)
*   **Runtime**: Docker Desktop (WSL 2)

## 總結
本專案實作證明了 **AI 賦能資安 (AI-Driven SecOps)** 的可行性。透過 MCP 協議，我們成功降低了資安維運的門檻，讓複雜的威脅狩獵轉變為直觀、高效的對話式體驗。

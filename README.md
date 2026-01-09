# 實作成果展示：基於 AI Agent 的 Wazuh 威脅狩獵指揮中心
**實作項目**：第一題 - 使用 Docker 與 MCP 協議實現對話式資安防護監控

---

## 📂 實作核心概念
本專案成功將 **Wazuh SIEM** 與 **Claude AI Agent** 整合。資安人員不再需要查詢複雜的資料庫語法，只需透過 **自然語言聊天 (Chat)**，即可由 AI 自動調用底層工具，進行全方位的威脅狩獵與資產狀態清查。

## 🛠️ 技術實現紀錄 (Technical Implementation)
1.  **容器化部署 (Dockerized)**：
    *   利用 Docker 部署全套 Wazuh 堆疊（Indexer, Manager, Dashboard）。
    *   手動編譯 **Rust 版本 MCP Server** 並封裝為 Docker 映像檔 `wazuh-mcp-server`。
2.  **MCP 協議整合**：
    *   實作 **Model Context Protocol (MCP)**，將 Wazuh API 轉換為 AI 可識別的 **Functions/Tools**。
    *   解決 Docker 跨容器網路連線問題，確保數據傳輸的安全與穩定。
3.  **AI 聯動配置**：
    *   配置 Claude Desktop 設定檔，實作 AI 工具調用邏輯，賦予 LLM 讀取資安日誌的能力。

---

## 🚀 功能展示 (Demonstration)

### 1. 自然語言資產清查 (Asset Inventory)
*   **指令**：「請列出目前環境中所有的 Wazuh Agents 狀態。」
*   **成果**：AI 成功抓取 Docker 內部的 Manager 數據，即時回報主機在線狀態與 IP，證明連線完全打通。

### 2. 對話式威脅狩獵 (Threat Hunting via Chat)
*   **指令**：「查詢最近一小時內 Level 10 以上的嚴重告警，並幫我分析攻擊行為。」
*   **成果**：AI 自動過濾大數據日誌，識別出 **CIS Benchmark 合規性弱點**，並總結出 SSH 權限、密碼策略等風險項目。

### 3. 智能資安建議與回應
*   **成果**：AI 針對偵測到的弱點，主動提供 **Bash 修復指令** 與 **pfSense 防火牆阻擋建議**，展現了 AI 作為資安大腦的推理能力。

---

## 📊 實作截圖證明
1.  <img width="1919" height="1019" alt="螢幕擷取畫面 2026-01-06 215420" src="https://github.com/user-attachments/assets/dc799d28-af76-494c-a34f-ef1cb7ac4a23" />

2.  <img width="1919" height="1015" alt="image" src="https://github.com/user-attachments/assets/c711d52b-ceaa-47d9-be41-d911e8e9ccc5" />

---

## 💡 總結
本實作成功展示了 **「AI 賦能資安」** 的未來趨勢。透過 Docker 與 MCP 協議，我們不僅降低了資安維運的門檻，更大幅提升了威脅狩獵的效率，讓 **Chat-based SecOps** 成為現實。

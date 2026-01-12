#  Wazuh 基於 MCP 協議之對話式威脅狩獵系統

> **實作成果展示**：本專案成功實作了透過 **AI Agent (Claude)** 結合 **Model Context Protocol (MCP)** 協議，對 **Wazuh SIEM** 進行自然語言導向的資產清查、合規性審計與實時威脅狩獵。

*   **核心技術**：Model Context Protocol (MCP)、Rust 容器化開發、Function Calling。
*   **實作目標**：實現 **Chat-based SecOps**，讓資安官透過對話即可完成以往需要數小時才能生成的安全分析報告。

---

## 系統架構深度解析 (Architecture Overview)

1.  **Wazuh**: 
    *   使用 Docker 部署 Wazuh Manager 與 Indexer，負責採集、解碼並儲存來自端點的資安事件數據。
2.  **MCP Server**: 
    *   以 **Rust** 實作的高效能 MCP 伺服器，將 Wazuh REST API 封裝為標準化的 **JSON-RPC** 工具。
    *   透過 Docker 內部網路 `single-node_default` 達成跨容器的安全 API 通訊。
3.  **Claude AI**: 
    *  根據指令自動判斷需要調用的底層工具，並將原始數據轉化為具備戰術價值的分析結果。

---

##  技術實現細節 (Technical Implementation)

### 1. 容器化工具鏈開發 
為確保 AI 呼叫工具時具備極低的延遲，採用了 **Rust 多階段編譯 (Multi-stage Build)**：
*   **Dockerfile 優化**：在 `rust:1.86-slim` 環境編譯原始碼，並於最終階段將二進位執行檔移至 `debian:bookworm-slim`。
*   **成果**：映像檔體積縮小 90%，顯著提升了 AI 啟動工具容器時的響應速度與安全性。

### 2. 跨容器連動與 API 安全配置
*   **網路連動**：實作 Docker 自定義網路橋接，解決了 MCP 工具端無法直接存取 Wazuh 管理端的問題。
*   **認證機制**：克服 401 Unauthorized 驗證挑戰，實作環境變數注入 API 憑證，並配置 `WAZUH_INSECURE=true` 以適配測試環境的 SSL 憑證。

### 3. AI 工具調用與邏輯推理 
*   實作 **Function Calling** 邏輯：AI 不只是顯示日誌，而是具備「過濾、彙整、推理」的能力。
*   **數據轉譯**：將複雜的 JSON 告警結構轉化為符合 MITRE ATT&CK 框架的中文分析報告。

---

## 實作功能展示 

###  自然語言資產清查 
*   **操作指令**：「列出目前所有 Wazuh Agents 的連線狀態，並回報受監控主機的 OS 資訊。」
*   **技術達成**：AI 即時調用 `get_wazuh_agents`，將分佈於不同網段的主機資訊彙整成結構化列表。

### 對話式威脅狩獵 
*   **操作指令**：「分析最近一小時內 Level 10 以上的嚴重告警，判斷是否有遭受暴力破解攻擊？」
*   **技術達成**：AI 查詢 Indexer 數據，精確捕捉到高風險事件，並自動對應攻擊者 IP 與目標帳號。

### 自動化合規性評估
*   **操作指令**：「檢查目前系統的 CIS Benchmark 合規性狀況，找出潛在的安全配置錯誤。」
*   **實作成果**：AI 讀取 SCA (Security Configuration Assessment) 報告，主動指出密碼策略、SSH 權限等弱點。

---

### AI 成功調用底層工具鏈進行數據分析
<img width="100%" alt="Wazuh Tool Invocation" src="https://github.com/user-attachments/assets/dc799d28-af76-494c-a34f-ef1cb7ac4a23" />
*技術解讀：展示了 Claude Agent 透過 MCP 協議，精確調用 `get_wazuh_alert_summary` 進行實時威脅判讀與告警統計分析。*

### 資安漏洞總結與智能化修復建議
<img width="100%" alt="Compliance Review" src="https://github.com/user-attachments/assets/c711d52b-ceaa-47d9-be41-d911e8e9ccc5" />
*技術解讀：展示 AI 將複雜的合規性掃描數據轉化為專業分析報告。*

---

## 總結
本專案成功透過 Docker 與 MCP 協議實踐了使用 AI 來協助資安問題的價值，將複雜的 SIEM 維運轉化為秒級響應的對話式體驗，不僅顯著降低了專業操作門檻，更大幅提升了威脅狩獵效率。

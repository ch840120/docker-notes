# 🐳 docker-notes
這是自我學習docker的筆記
## 🐳 Docker 是什麼？
Docker 是一種容器化技術，可以讓你把應用程式和它的執行環境（如程式碼、相依套件、作業系統）打包成一個獨立的容器，確保在任何地方都能一致執行。

🔧 簡單來說，Docker 就像：
<br>
📦 把應用加環境打包起來 →
<br>
🚀 隨時隨地快速啟動 →
<br>
💻 不用擔心「我電腦跑不動」的問題

## 🧩 Docker 有什麼特色？
- 🛠 輕量：不像虛擬機要整個作業系統，容器只裝需要的東西
- 🌍 跨平台：開發環境、測試環境、上線環境都可以一模一樣
- ⚡️ 快速啟動：秒開，不需等作業系統開機
- 🔁 容易整合 CI/CD：適合 DevOps、雲端部署、自動化流程

## 🧭 圖片說明：Docker 架構運作原理
這張圖展示了 Docker 的基本組成與運作流程，分為三大核心元件：
<br><br>
<img src="https://github.com/user-attachments/assets/724cc43c-f6d7-4b40-8a87-a066cab65803" width="500"/>

### 1️⃣ Docker Client（用戶端）
- 是使用者與 Docker 互動的介面，通常透過 CLI（例如 docker build, docker run）發出指令。
- 指令會送到 Docker Engine。

### 2️⃣ Docker Engine（引擎）
- 是 Docker 的核心，負責接收指令並執行相關操作。
- 包含兩個關鍵部分：

#### 📦 Image（映像檔）
- 是一組可執行的程式、程式庫、依賴與設定的集合。
- 可視為容器的「模板」。

#### 🧱 Container（容器）
- 是從映像檔啟動後的實體執行環境，類似輕量虛擬機。
- 具有獨立檔案系統與執行空間，彼此隔離但共享主機核心。

#### 🔁 運作流程簡述：
- 使用者透過 Docker Client 發出指令（如 run）。
- Docker Engine 接收指令並使用對應的 Image。
- 從映像檔啟動並建立 Container，開始執行應用。


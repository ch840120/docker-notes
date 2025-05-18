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

## 📌Docker常用指令
### 🐳 基本指令
- docker --version	查看 Docker 版本
- docker info	顯示 Docker 系統資訊
- docker help	查看指令幫助

### 📦 映像檔（Image）相關
- docker search <關鍵字> 在 Docker Hub 上搜尋映像，例如：docker search ubuntu
- docker pull <映像名稱>	從 Docker Hub 下載映像檔，例如：docker pull nginx
- docker images	列出本地所有映像檔
- docker rmi <映像 ID>	刪除映像檔

### 🧱 容器（Container）操作
- docker run <映像名稱> 建立並啟動容器（例如：docker run nginx）
- docker run -it <映像> /bin/bash 啟動互動式終端機
- docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 使用額外參數啟動容器

##### 🔧 常用 [OPTIONS]：
| 選項             | 說明                                         |
| -------------- | ------------------------------------------ |
| `-d`           | 背景執行容器（detached mode）                      |
| `--name <名稱>`  | 指定容器名稱                                     |
| `-p 主機埠:容器埠`   | 對外開放埠口（port mapping）<br>例：`-p 8080:80`     |
| `-v 主機目錄:容器目錄` | 掛載磁碟（volume）<br>例：`-v ~/data:/app/data`    |
| `--rm`         | 容器停止後自動刪除                                  |
| `-e KEY=VALUE` | 設定環境變數（ENV）<br>例：`-e TZ=Asia/Taipei`       |
| `-i`           | 保持標準輸入開啟（interactive mode，可互動操作）           |
| `-t`           | 分配一個終端機（TTY，讓輸入輸出像真實終端機）                   |
| `-it`          | 結合 `-i` 與 `-t`，進入互動式終端機，最常用於手動操作容器如 bash 等 |


##### 📜 常見 [COMMAND] [ARG...]：
| 指令範例                     | 說明                      |
| ------------------------ | ----------------------- |
| `/bin/bash`              | 啟動 Bash 終端機             |
| `python app.py`          | 執行 Python 應用程式          |
| `nginx -g 'daemon off;'` | 執行 nginx 並讓它不背景執行（常見用法） |

#### 🧱 其他容器指令
- docker ps	查看正在執行中的容器
- docker ps -a	查看所有容器（包含已停止）
- docker stop <容器 ID>	停止容器
- docker start <容器 ID>	啟動容器
- docker restart <容器 ID>	重啟容器
- docker rm <容器 ID>	刪除容器


### 📝 建立映像（Dockerfile）
- docker build -t <映像名稱> .	用 Dockerfile 建立映像檔（在該資料夾中）

### 📁 檔案操作與 Volume
- docker cp <容器 ID>:<路徑> <本地路徑>	從容器複製檔案到主機
- docker volume ls	列出所有 Volume
- docker volume rm <Volume 名稱>	刪除 Volume

### 🐙 進階實用
- docker exec -it <容器 ID> bash	進入容器的 shell
- docker logs <容器 ID>	查看容器日誌
- docker network ls	查看網路
- docker-compose up	用 docker-compose.yml 啟動應用（需安裝 Compose）
- docker-compose down	停止並移除所有服務


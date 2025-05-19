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

## 🔧🐳 Docker 實作：自製 Ubuntu + Vim 映像並上傳 Docker Hub
```bash
## Step 1: 從 Docker Hub 下載最新的 Ubuntu 映像
docker pull ubuntu:latest

## Step 2: 查看目前本地的映像
docker images

## Step 3: 以 ubuntu:latest 建立並啟動互動式容器，命名為 myubuntu1
docker run -it --name myubuntu1 ubuntu:latest bash

## Step 4: 在容器內執行這些指令（安裝 vim）
apt update && apt install -y vim

## Step 5: 離開容器（回到主機終端）
exit

## Step 6: 將 myubuntu1 的容器內容提交為新的映像
# 請將 "yourname" 改為你的名字；"username" 改為你的 Docker Hub 帳號
docker commit -a "yourname" -m "install vim" myubuntu1 username/ubuntu-with-vim:latest

## Step 7: 查看已建立的新映像
docker images

## Step 8: 登入 Docker Hub（輸入帳號與密碼）
docker login

## Step 9: 將建立的映像推送到 Docker Hub
docker push username/ubuntu-with-vim:latest

## Step 10: （選用）登出 Docker Hub
docker logout

## Step 11: 到 Docker Hub 查看映像
# 開啟瀏覽器並前往 https://hub.docker.com/repositories
# 登入你的帳號後，即可看到 username/ubuntu-with-vim 的映像已經上傳
```

## 🔧🐳 Docker 實作：自製 Ubuntu + Vim 映像並上傳至 Private Docker Registry

```bash
# ✅ Step 1: 從 Docker Hub 下載最新的 Ubuntu 映像
docker pull ubuntu:latest  # 取得官方最新版本的 Ubuntu 映像

# ✅ Step 2: 查看目前本地的映像
docker images  # 確認映像是否已下載成功

# ✅ Step 3: 以 ubuntu:latest 建立並啟動互動式容器，命名為 myubuntu1
docker run -it --name myubuntu1 ubuntu:latest bash  
# 啟動容器並進入 bash shell，用來手動安裝 vim

# ✅ Step 4: 在容器內執行（安裝 vim）
apt update && apt install -y vim  
# 更新套件列表並安裝 vim 編輯器

# ✅ Step 5: 離開容器（回到主機）
exit  # 離開容器並回到本機終端機

# ✅ Step 6: 將 myubuntu1 容器內容提交為新的映像
docker commit -a "yourname" -m "install vim" myubuntu1 username/ubuntu-with-vim:latest  
# 將容器內容保存為新映像，加入作者和說明
# 👉 請將 "yourname" 改為你的名字；"username" 改為你的 Docker 使用者名稱

# ✅ Step 7: 查看新建立的映像
docker images  # 再次確認已建立的 ubuntu-with-vim 映像是否存在

# ✅ Step 8: 下載官方 Docker Registry 映像
docker pull registry  # 從 Docker Hub 拉下私有 registry 映像

# ✅ Step 9: 檢查 registry 映像是否已存在
docker images

# ✅ Step 10: 啟動本地 Registry 並將映像資料持久化到 Windows 路徑
docker run -d -p 5000:5000 -v /mnt/c/Users/User/myregistry:/var/lib/registry registry
# 將本機 5000 port 映射到容器，並使用 volume 掛載資料到 Windows 目錄

# ✅ Step 11: 確認 Registry 容器是否正在運作
docker ps  # 查看是否有 registry 容器在運行中

# ✅ Step 12: 測試 Registry 是否有正常回應
curl http://yourHostIP:5000/v2/_catalog  
# 檢查 registry 是否能回傳 repository 資訊（初次應為空）
# 👉 請將 "yourHostIP" 改為你的主機IP

# ✅ Step 13: 將映像打上標籤以準備推送到私有 registry
docker tag username/ubuntu-with-vim:latest yourHostIP:5000/username/ubuntu-with-vim:latest  
# 加上新的 tag，指向你的私有 registry 位置
# 👉  請將 "username" 改為你的 Docker 使用者名稱; "yourHostIP" 改為你的主機IP

# ✅ Step 14: 設定 Docker 接受 insecure registry（僅限 HTTP，未加密）

# 👉 適用 Docker Desktop for Windows + WSL 用戶
# 開啟 Docker Desktop → Settings → Docker Engine
# 修改設定 JSON，加入：

# ⚠️ 注意格式正確（如下所示）
# {
#   ...其他設定...
#   "insecure-registries": ["yourHostIP:5000"]
# }

# 修改後按「Apply & Restart」讓設定生效

# ✅ Step 15: 推送映像到私有 Registry
docker push yourHostIP:5000/username/ubuntu-with-vim:latest  
# 將你自製的映像傳送到私有 registry
# 👉  請將 "username" 改為你的 Docker 使用者名稱; "yourHostIP" 改為你的主機IP

# ✅ Step 16: 再次查詢 registry，確認映像是否已上傳
curl http://yourHostIP:5000/v2/_catalog  
# 回應中應會出現 "username/ubuntu-with-vim"
```
<img src="https://github.com/user-attachments/assets/4cecd3f6-a4b3-4ccb-bcb9-1f9a259210c1" width="500"/>



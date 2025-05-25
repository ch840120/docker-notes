# 🐳 Docker 教學
## 🗂️ 目錄
- [🐳 Docker 教學](#-docker-教學)
  - [🐳 前言](#-前言)
  - [🐳 Docker 是什麼？](#-docker-是什麼)
  - [🐳 Docker 有什麼優點？](#-docker-有什麼優點)
  - [🐳 Docker 架構簡單說明](#-docker-架構簡單說明)
    - [1️⃣ Docker Client（用戶端）](#1️⃣-docker-client用戶端)
    - [2️⃣ Docker Engine（引擎）](#2️⃣-docker-engine引擎)
  - [🐳 Docker 常用指令](#-docker-常用指令)
    - [📜 基本指令](#-基本指令)
    - [📦 映像檔相關](#-映像檔相關)
    - [🧱 容器操作](#-容器操作)
      - [🔧 常用選項（OPTIONS）](#-常用選項options)
      - [📜 常見命令 (COMMAND) 和參數 (ARG)](#-常見命令-command-和參數-arg)
    - [📜 建立映像（Dockerfile）](#-建立映像dockerfile)
    - [📁 檔案操作與 Volume](#-檔案操作與-volume)
    - [🐙 進階實用](#-進階實用)
  - [🐳 什麼是 Dockerfile？](#-什麼是-dockerfile)
    - [📜 Dockerfile範例檔](#-dockerfile範例檔)
        - [📜 Dockerfile範例檔指令說明](#-dockerfile範例檔指令說明)
    - [📜 Dockerfile常用指令](#-dockerfile常用指令)
  - [🐳 什麼是 Docker Compose？](#-什麼是-docker-compose)
    - [📜 Docker Compose範例檔](#-docker-compose範例檔)
        - [📜 範例檔指令說明](#-範例檔指令說明)
    - [📜 Docker Compose常用指令](#-docker-compose常用指令)
  - [⚙️ Docker 實作](#️-docker-實作)
    - [🔧 範例1:自製 Ubuntu + Vim 映像並上傳 Docker Hub](#-範例1自製-ubuntu--vim-映像並上傳-docker-hub)
    - [🔧 範例2:自製 Ubuntu + Vim 映像並上傳至 Private Docker Registry](#-範例2自製-ubuntu--vim-映像並上傳至-private-docker-registry)
      - [🛤️ 範例1、2流程圖](#️-範例12流程圖)
    - [🔧 範例3:自製Dockerfile建立安裝vim與net-tools的Ubuntu](#-範例3自製dockerfile建立安裝vim與net-tools的ubuntu)
      - [📁 專案目錄結構](#-專案目錄結構)
      - [📄 Dockerfile](#-dockerfile)
      - [📄 start.sh](#-startsh)
      - [🛠️ Docker build 範例](#️-docker-build-範例)
      - [🔔 小提醒](#-小提醒)
    - [🔧 範例4: 自製Docker Compose快速架設PostgreSQL + pgAdmin](#-範例4-自製docker-compose快速架設postgresql--pgadmin)
      - [📁 專案目錄結構](#-專案目錄結構-1)
      - [📄 docker-compose.yml](#-docker-composeyml)
      - [🛠️ Docker Compose 啟動範例](#️-docker-compose-啟動範例)
      - [🔔 小提醒](#-小提醒-1)


## 🐳 前言
你是否遇過「我的程式在我電腦可以跑，別人卻不行」的問題？  
Docker 可以幫你把程式和運行環境打包起來，不管在哪台電腦都能正常跑，解決這種困擾。  
這份教學從零開始，帶你一步步認識 Docker，超適合新手！

## 🐳 Docker 是什麼？
Docker 是一種容器技術，能把應用程式和它需要的環境（程式碼、依賴、作業系統）包成一個獨立容器，確保到哪都能跑。

簡單說就是：  
📦 把程式和環境打包 → 🚀 隨時啟動 → 💻 不用擔心環境差異。

## 🐳 Docker 有什麼優點？
- 🛠 輕量：比虛擬機只裝需要的東西。
- 🌍 跨平台：開發、測試、上線都一致。
- ⚡️ 快速啟動：秒開容器，不用等系統開機。
- 🔁 適合 CI/CD 自動化流程。

## 🐳 Docker 架構簡單說明
這張圖展示了 Docker 的基本組成與運作流程，分為三大核心元件：
<br><br>
<img src="https://github.com/user-attachments/assets/724cc43c-f6d7-4b40-8a87-a066cab65803" width="500"/>

### 1️⃣ Docker Client（用戶端）
你發出指令的地方，像 `docker run`。

### 2️⃣ Docker Engine（引擎）
接收指令，幫你做事。  
包含：
- 📦 Image（映像檔）：容器模板。
- 🧱 Container（容器）：執行中的實體環境。

## 🐳 Docker 常用指令

### 📜 基本指令
```bash
docker --version      # 查看 Docker 版本
docker info           # 查看系統資訊
docker help           # 查看指令說明
``` 

### 📦 映像檔相關
```bash
docker search <關鍵字>    # 在 Docker Hub 搜尋映像
docker pull <映像名稱>    # 下載映像
docker images             # 列出本地映像
docker rmi <映像ID>       # 刪除映像
```

### 🧱 容器操作
``` bash
docker run <映像名稱>            # 建立並啟動容器
docker run -it <映像> /bin/bash   # 互動模式啟動容器
docker ps                        # 查看正在執行的容器
docker ps -a                    # 查看所有容器（包含停止）
docker stop <容器ID>             # 停止容器
docker start <容器ID>            # 啟動容器
docker restart <容器ID>          # 重啟容器
docker rm <容器ID>               # 刪除容器
```
#### 🔧 常用選項（OPTIONS）
| 選項             | 說明                    |
| -------------- | --------------------- |
| `-d`           | 背景執行容器                |
| `--name <名稱>`  | 指定容器名稱                |
| `-p 主機埠:容器埠`   | 映射埠口（例如：`-p 8080:80`） |
| `-v 主機目錄:容器目錄` | 掛載磁碟卷（volume）         |
| `--rm`         | 容器停止後自動刪除             |
| `-e KEY=VALUE` | 設定環境變數                |
| `-it`          | 互動模式（結合 `-i` 和 `-t`）  |

#### 📜 常見命令 (COMMAND) 和參數 (ARG)
| 指令範例                     | 說明            |
| ------------------------ | ------------- |
| `/bin/bash`              | 啟動 Bash 終端機   |
| `python app.py`          | 執行 Python 程式  |
| `nginx -g 'daemon off;'` | 讓 nginx 不背景執行 |

### 📜 建立映像（Dockerfile）
```bash
docker build -t <映像名稱> .
```

### 📁 檔案操作與 Volume
```bash
docker cp <容器ID>:<路徑> <本機路徑>   # 複製檔案從容器到主機
docker volume ls                      # 列出所有 Volume
docker volume rm <Volume名稱>          # 刪除 Volume
```

### 🐙 進階實用
``` bash
docker exec -it <容器ID> bash       # 進入容器 shell
docker logs <容器ID>                # 查看容器日誌
docker network ls                   # 查看網路
docker-compose up                  # 使用 docker-compose 啟動
docker-compose down                # 停止並移除服務
```

## 🐳 什麼是 Dockerfile？

Dockerfile 是用來自動化建立 Docker 映像檔（image）的腳本檔案。  
透過撰寫 Dockerfile，我們可以定義應用程式所需的作業系統環境、安裝的套件、複製的程式碼，以及容器啟動時要執行的指令。  
簡單來說，Dockerfile 讓專案環境一致、部署更方便，任何人都能根據它重現相同的執行環境。🚀

### 📜 Dockerfile範例檔

```dockerfile
FROM python:3.10     # 指定基礎映像為 Python 3.10 官方映像
WORKDIR /app         # 設定工作目錄為 /app
COPY . .             # 複製當前目錄所有檔案到容器的工作目錄
RUN pip install -r requirements.txt   # 安裝必要套件
CMD ["python", "app.py"]               # 容器啟動時執行指令
```
##### 📜 Dockerfile範例檔指令說明
| 指令    | 說明                                    |
|--------|---------------------------------------|
| `FROM`  | 指定基礎映像。例：`FROM python:3.10`    |
| `WORKDIR` | 設定工作目錄，後續指令會在此目錄執行。        |
| `COPY`  | 將檔案或資料夾複製到映像檔中。                |
| `RUN`   | 建置映像時執行指令，通常用來安裝套件或設置環境。 |
| `CMD`   | 容器啟動時執行的預設指令。                    |

### 📜 Dockerfile常用指令
```bash
docker build -t <名稱> .               # 根據 Dockerfile 建立映像，並指定名稱
docker build --no-cache -t <名稱> .   # 不使用快取，強制重新建置映像
docker build -f <檔名> -t <名稱> .    # 指定使用特定的 Dockerfile 建立映像
docker images                         # 列出本地所有映像檔
docker rmi <映像ID或名稱>             # 刪除指定映像檔
```

## 🐳 什麼是 Docker Compose？

Docker Compose 是用來定義和管理多個 Docker 容器的工具，  
透過一個 `docker-compose.yml` 檔案，你可以描述多個服務（容器）的設定與關聯，  
一行指令就能同時啟動、停止整個應用環境。

### 📜 Docker Compose範例檔

```yaml
version: "3.8"
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: exampledb
    volumes:
      - dbdata:/var/lib/postgresql/data

volumes:
  dbdata:
```
##### 📜 範例檔指令說明
| 指令            | 說明                                                      |
|----------------|---------------------------------------------------------|
| `version`       | 指定 Docker Compose 檔案格式版本，例：`"3.8"`                     |
| `services`      | 定義多個服務（容器）的設定區塊                                        |
| `image`         | 指定服務使用的映像檔，例如 `nginx:latest` 或 `postgres:15`           |
| `ports`         | 設定本機與容器埠口的映射，例如 `"8080:80"` 表示本機 8080 連到容器 80 端口 |
| `environment`   | 設定容器的環境變數，用於設定資料庫使用者、密碼、資料庫名稱等                   |
| `volumes`       | 掛載持久化儲存卷，確保資料在容器刪除後仍保留                               |
| `volumes:`      | 在根部定義命名卷（volume），此例中定義 `dbdata`                             |

### 📜 Docker Compose常用指令
```bash
docker compose up -d                 # 以背景模式啟動所有服務
docker compose down                  # 停止並移除服務（容器、網路等）
docker compose ps                   # 查看目前正在執行的服務
docker compose logs -f              # 持續查看服務輸出日誌
docker compose restart <服務名稱>   # 重新啟動指定服務（如 web 或 db）
```

## ⚙️ Docker 實作

### 🔧 範例1:自製 Ubuntu + Vim 映像並上傳 Docker Hub
```bash
## ✅ Step 1: 從 Docker Hub 下載最新的 Ubuntu 映像
docker pull ubuntu:latest

## ✅ Step 2: 查看目前本地的映像
docker images

## ✅ Step 3: 以 ubuntu:latest 建立並啟動互動式容器，命名為 myubuntu1
docker run -it --name myubuntu1 ubuntu:latest bash

## ✅ Step 4: 在容器內執行這些指令（安裝 vim）
apt update && apt install -y vim

## ✅ Step 5: 離開容器（回到主機終端）
exit

## ✅ Step 6: 將 myubuntu1 的容器內容提交為新的映像
# 請將 "yourname" 改為你的名字；"username" 改為你的 Docker Hub 帳號
docker commit -a "yourname" -m "install vim" myubuntu1 username/ubuntu-with-vim:latest

## ✅ Step 7: 查看已建立的新映像
docker images

## ✅ Step 8: 登入 Docker Hub（輸入帳號與密碼）
docker login

## ✅ Step 9: 將建立的映像推送到 Docker Hub
docker push username/ubuntu-with-vim:latest

## ✅ Step 10: （選用）登出 Docker Hub
docker logout

## ✅ Step 11: 到 Docker Hub 查看映像
# 開啟瀏覽器並前往 https://hub.docker.com/repositories
# 登入你的帳號後，即可看到 username/ubuntu-with-vim 的映像已經上傳
```

---

### 🔧 範例2:自製 Ubuntu + Vim 映像並上傳至 Private Docker Registry

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
# 👉  請將 "username" 改為你的 Docker 使用者名稱; "yourHostIP" 改為你的主機IP
```
#### 🛤️ 範例1、2流程圖
<img src="https://github.com/user-attachments/assets/4cecd3f6-a4b3-4ccb-bcb9-1f9a259210c1" width="500"/>

---

### 🔧 範例3:自製Dockerfile建立安裝vim與net-tools的Ubuntu

#### 📁 專案目錄結構
```
📁 ubuntu-netvim/
├── 📄 Dockerfile
├── 📄 start.sh
```

#### 📄 Dockerfile
```dockerfile
# 使用官方 Ubuntu 作為基礎映像檔
FROM ubuntu

# 設定環境變數 MYPATH 並給預設值
ENV MYPATH=/usr/local

# 設定工作目錄
WORKDIR $MYPATH

# 更新套件列表
RUN apt update

# 安裝 net-tools（包含 ifconfig）和 vim
RUN apt install -y net-tools vim

# 開放 80 port
EXPOSE 80

# 複製啟動腳本到容器
COPY start.sh $MYPATH/start.sh
RUN chmod +x $MYPATH/start.sh

# 預設啟動執行腳本
CMD ["/usr/local/start.sh"]
```
#### 📄 start.sh
```bash
#!/bin/bash
echo "MYPATH is $MYPATH"
echo "install ipconfig and vim into ubuntu success-------ok"
bash
```

#### 🛠️ Docker build 範例
```bash
# 進行 build，並指定 image 名稱為 my-image:latest
docker build -t my-image:latest .
```
💡 備註：
- -t 可以指定 image 的名稱與 tag（例如 ubuntu-netvim:latest）。
- . 表示 Dockerfile 位於當前目錄。

🛠️ Docker run 範例
```bash
# 使用剛剛 build 好的 my-image:latest 建立 container 並啟動
docker run --name myUbuntu-netvim -it ubuntu-netvim:latest
```
💡 備註：
- --name myUbuntu-netvim：為這個 container 命名，方便後續管理。
- -it：啟動互動式終端機（interactive + TTY），可以直接進入 container 裡面操作。
- ubuntu-netvim:latest：指定要啟動的 image 及 tag（這裡用的是 ubuntu-netvim:latest）。

#### 🔔 小提醒
- 🆕 每次修改 Dockerfile 內容後，都需要重新執行 docker build 來產生新的 image。

---

### 🔧 範例4: 自製Docker Compose快速架設PostgreSQL + pgAdmin

#### 📁 專案目錄結構
```
📁 pg-postgres-pgadmin/
├── 📄 docker-compose.yml
```

#### 📄 docker-compose.yml
```yaml
version: "3.8"
services:
  postgres:
    container_name: postgres
    image: postgres:15
    environment:
      POSTGRES_USER: abel
      POSTGRES_PASSWORD: 123456
      POSTGRES_DB: mydatabase
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    restart: unless-stopped
    networks:
      - pgnetwork

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4
    depends_on:
      - postgres
    environment:
      PGADMIN_DEFAULT_EMAIL: abel@example.com
      PGADMIN_DEFAULT_PASSWORD: 123456
    ports:
      - "8080:80"
    volumes:
      - pgadmin_data:/var/lib/pgadmin
    restart: unless-stopped
    networks:
      - pgnetwork

volumes:
  pgdata:
  pgadmin_data:

networks:
  pgnetwork:
```
#### 🛠️ Docker Compose 啟動範例
```bash
# 啟動所有服務（背景執行）
docker compose up -d

# 停止並移除服務
docker compose down
```
#### 🔔 小提醒
- docker compose up -d 會在背景啟動 PostgreSQL 與 pgAdmin 服務。
- 你可以用瀏覽器打開 http://localhost:8080 進入 pgAdmin，帳號密碼就是 PGADMIN_DEFAULT_EMAIL 和 PGADMIN_DEFAULT_PASSWORD。
- PostgreSQL 預設連接埠是 5432。
- docker compose down 會停止並刪除服務容器，但不會刪除 volume 裡的資料。
- 每次修改完 docker-compose.yml 設定後，記得重新執行 docker compose up -d。
- volume 讓資料持久化，即使容器刪除資料仍保留。



# Docker安裝Graylog
參考文章
https://www.reddit.com/r/graylog/comments/159j058/dockercompose_example_for_graylog_5x_install/
https://go2docs.graylog.org/5-1/downloading_and_installing_graylog/docker_installation.htm


### 複製產生 .env
```bash
cp .env.examlpe .env
```

### 修改 .env 內容
```bash
GRAYLOG_ROOT_USERNAME=系統登入帳號
GRAYLOG_PASSWORD_SECRET=系統登入密碼
GRAYLOG_ROOT_PASSWORD_SHA2=SHA256(系統登入密碼)
GRAYLOG_HTTP_EXTERNAL_URI=http://要提供WEB服務的IP:$WEB_PORT/
WEB_PORT=80
```

### 確保資料夾權限
```bash
Ai4dt_admin@log-server-fg100d-7851-0:~$ cd /storage/graylog5.1/
Ai4dt_admin@log-server-fg100d-7851-0:/storage/graylog5.1$ ll
total 28
drwxr-xr-x  5 root   root  4096 Nov  6 06:39 ./
drwxr-xr-x 13 root   root  4096 Nov  6 06:29 ../
drwxr-xr-x  3 ubuntu root  4096 Nov  6 06:42 es_data/
drwxr-xr-x  8   1100 1100  4096 Nov  6 06:47 graylog_data/
drwxr-xr-x  4    999 root 12288 Nov  7 08:45 mongo_data/
```
若起初建立遇到服務無法正常建立，可以看一下docker logs，通常會看到permission deny，基本上就是遇到資料夾權限不正確的問題。
* elasticsearch 的資料夾，權限須為 ubuntu:root
  - /storage/graylog5.1/es_data 
* graylog 的資料夾，權限須為 1100:1100
  - /storage/graylog5.1/graylog_data 
  - ./graylog/config    (相對於docker-compose.yml的位置)

可以使用`sudo chown -R 1100:1100 /storage/graylog5.1/graylog_data`的方式嘗試解決，並`sudo docker-compose up -d`重新啟動服務。

### 啟動服務
```bash
sudo docker-compose up -d
```

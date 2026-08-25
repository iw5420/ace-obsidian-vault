# Hicloud Windows Nginx + Let's Encrypt 自動展期解決方案

## 架構概述

本方案採用 `win-acme` (WACS) 搭配 HTTP-01 驗證機制。透過將 Let's Encrypt 驗證路由與前端 SPA 靜態檔案 (`dist`) 解耦，確保部署時不會覆蓋驗證目錄。同時建立自動化腳本，使憑證展期與 Nginx 記憶體重載 (Reload) 能在背景全自動完成。

## 前置作業 (DNS 與防火牆)

1. **No-IP 設定**：登入 No-IP 控制台，將網域 (如 `macuhau.ddns.net`) 的紀錄類型設定為 **DNS Host (A)**，並指向 Hicloud 伺服器 IP (不可使用 URL 轉址功能)。
    
2. **防火牆設定**：確保 Hicloud 網路安全性群組及 Windows 內建防火牆皆已放行 Port 80 (HTTP) 與 Port 443 (HTTPS)。
    

## 步驟一：Nginx 驗證路由設定 (解耦設計)

為了避免前端路由攔截 ACME 驗證請求，需在 Nginx 的 HTTP (80) 與 HTTPS (443) 區塊中，同時加入獨立的 `.well-known` 攔截規則。

1. 開啟 Nginx 設定檔：`C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0\conf\nginx.conf`
    
2. 修改設定檔，確保包含以下結構：
    

Nginx

```
http {
    # HTTP (80) 區塊
    server {
        listen       80;
        server_name  macuhau.ddns.net;

        # 攔截 Let's Encrypt 驗證請求
        location ^~ /.well-known/acme-challenge/ {
            root C:/Users/Administrator/Desktop/exam-platform/nginx/nginx-1.26.0/html; 
            allow all;
        }

        # 一般請求自動轉 HTTPS
        location / {
            return 301 https://$host$request_uri;
        }
    }

    # HTTPS (443) 區塊
    server {
        listen       443 ssl;
        server_name  macuhau.ddns.net;

        # 憑證路徑 (稍後會替換為 Let's Encrypt 憑證)
        ssl_certificate      C:/Users/Administrator/Desktop/macu-portal/ssl/server.crt;
        ssl_certificate_key  C:/Users/Administrator/Desktop/macu-portal/ssl/server.key;

        # 確保 HTTPS 同樣攔截驗證請求
        location ^~ /.well-known/acme-challenge/ {
            root C:/Users/Administrator/Desktop/exam-platform/nginx/nginx-1.26.0/html; 
            allow all;
        }

        # 前端靜態檔案
        location / {
            root   C:/Users/Administrator/Desktop/exam-platform/frontend/dist; 
            index  index.html;
            try_files $uri $uri/ /index.html;
        }
        
        # 後端 API 代理 (略)
    }
}
```

3. 存檔後，開啟命令提示字元 (CMD) 執行重載：
    

DOS

```
cd C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0
nginx.exe -s reload
```

## 步驟二：環境建置與連動腳本配置

### 1. 目錄建置

在 Nginx 主程式目錄下，建立兩個新資料夾：

- `html`：用於存放 Let's Encrypt 驗證檔案。
    
- `ssl`：用於存放輸出的 `.pem` 憑證檔。
    

### 2. 下載 win-acme

1. 前往 GitHub Release 頁面：`[https://github.com/win-acme/win-acme/releases](https://github.com/win-acme/win-acme/releases)`
    
2. 下載無相依性的裁剪版：`win-acme.v2.2.9.1701.x64.trimmed.zip`
    
3. 解壓縮至獨立目錄：`C:\win-acme`
    

### 3. 建立 Nginx 自動重載腳本

在 Nginx 主目錄 `C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0\` 下建立名為 `reload_nginx.bat` 的檔案，內容如下：

DOS

```
@echo off
cd /d C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0
nginx.exe -s reload
```

## 步驟三：執行憑證申請與自動化連動

以**系統管理員權限**開啟 CMD，執行以下指令完成憑證申請，並自動綁定重載腳本：

DOS

```
cd C:\win-acme
wacs.exe --target manual --host macuhau.ddns.net --validation filesystem --webroot C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0\html --store pemfiles --pemfilespath C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0\ssl --installation script --script "C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0\reload_nginx.bat"
```

- **互動提示處理**：
    
    - Open in default application? 輸入 `n`
        
    - Do you agree with the terms? 輸入 `y`
        
    - Enter email(s) for notifications: 輸入管理者信箱 (如 `iw5420@gmail.com`)
        

## 步驟四：掛載 Let's Encrypt 憑證至 Nginx

確認 `ssl` 目錄已產出 `.pem` 檔案後，回到 `nginx.conf`，將 HTTPS 區塊的憑證路徑更新為新生成的 Let's Encrypt 檔案：

Nginx

```
ssl_certificate      C:/Users/Administrator/Desktop/exam-platform/nginx/nginx-1.26.0/ssl/macuhau.ddns.net-chain.pem;
ssl_certificate_key  C:/Users/Administrator/Desktop/exam-platform/nginx/nginx-1.26.0/ssl/macuhau.ddns.net-key.pem;
```

存檔並再次重載 Nginx：

DOS

```
cd C:\Users\Administrator\Desktop\exam-platform\nginx\nginx-1.26.0
nginx.exe -s reload
```

此時透過瀏覽器存取 `[https://macuhau.ddns.net](https://macuhau.ddns.net)`，安全警告即已解除。

## 步驟五：排程修復與驗證 (確保自動展期生效)

為確保系統排程 (Task Scheduler) 以正確的權限 (SYSTEM) 運行，且路徑無誤，須進行最終確認：

### 1. 透過互動式選單重建排程

以系統管理員權限開啟 CMD，執行主程式進入選單：

DOS

```
cd C:\win-acme
wacs.exe
```

依序輸入以下按鍵：

- 輸入 `O` (More options...)
    
- 輸入 `S` (Scheduled task)
    
- 輸入 `1` (Create, update or replace scheduled task)
    
- 提示 `Do you want to specify the user the task will run as?` 輸入 `n` (預設使用 SYSTEM 帳戶)
    
- 輸入 `Q` 退出。
    

### 2. 驗證排程狀態

在 CMD 執行以下指令，確認排程狀態為 `Ready` 且具備正確的 Next Run Time：

DOS

```
schtasks /query /tn "win-acme renew (acme-v02.api.letsencrypt.org)"
```

### 申请域名和证书

- 1.安装certbot

      apt install certbot

- 2.申请证书

      certbot certonly -d yourdomain.com   --standalone

- 3.更新证书

      certbot renew
### 安装tuic
- 1.下载程序

        wget https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-x86_64-unknown-linux-gnu && mv tuic-server-1.0.0-x86_64-unknown-linux-gnu /usr/local/bin/tuic && chmod +x /usr/local/bin/tuic

- 2.下载配置文件

- 3.下载service文件

### 开启tuic
    systemctl start tuic
### 设置tuic开机自启
    systemctl enable tuic
### 重启tuic
    systemctl restart tuic
### 查看tuic状态
    systemctl status tuic

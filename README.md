### 安装tuic
- 1.下载程序

        wget https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-x86_64-unknown-linux-gnu && mv tuic-server-1.0.0-x86_64-unknown-linux-gnu /usr/local/bin/tuic && chmod +x /usr/local/bin/tuic

- 2.下载配置文件

        mkdir /usr/local/etc/tuic && curl -Lo /usr/local/etc/tuic/config.json https://raw.githubusercontent.com/MHY2233/TUIC/main/config.json
- 3.下载service文件

        curl -Lo /etc/systemd/system/tuic-server.service https://raw.githubusercontent.com/chika0801/tuic-install/main/tuic-server.service && systemctl daemon-reload

### 开启tuic
    systemctl start tuic
### 设置tuic开机自启
    systemctl enable tuic
### 重启tuic
    systemctl restart tuic
### 查看tuic状态
    systemctl status tuic


- 下载程序

        wget https://github.com/EAimTY/tuic/releases/download/tuic-server-1.0.0/tuic-server-1.0.0-x86_64-unknown-linux-gnu && mv tuic-server-1.0.0-x86_64-unknown-linux-gnu /usr/local/bin/tuic && chmod +x /usr/local/bin/tuic

     或者

        curl -Lo tuic https://github.com/EAimTY/tuic/releases/latest/download/tuic-server-1.0.0-x86_64-unknown-linux-gnu && chmod +x tuic&& mv -f tuic /usr/local/bin/

- 下载配置文件

        mkdir /usr/local/etc/tuic && curl -Lo /usr/local/etc/tuic/config.json https://raw.githubusercontent.com/MHY2233/TUIC/main/config.json

- 下载service文件

        curl -Lo /etc/systemd/system/tuic-server.service https://raw.githubusercontent.com/MHY2233/TUIC/main/tuic.service && systemctl daemon-reload

- 启动tuic

        systemctl enable --now tuic-server

- 重启tuic

        systemctl restart tuic

- 查看tuic状态

        systemctl status tuic



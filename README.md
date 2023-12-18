
### 申请域名和证书
- 1.因为QUIC是基于tls的，我们可以先去申请一个域名，可以去cloudflare或者namecheap上面买一个最便宜的，或者去https://freenom.com 注册一个免费域名。然后将域名解析到你的服务器ip上面。

- 2.安装certbot, 这个是可以免费申请https证书的工具, 在最新的debian11上面可以用命令`apt install certbot`安装。

- 3.申请证书, 在你的服务器上面运行下面的命令，前提是要把80和443的端口占用程序先暂停掉，否则会报错。运行下面的命令会让你输入邮箱，主要用来通知过期时间的。
```bash
certbot certonly -d yourdomain.com   --standalone

 - Congratulations! Your certificate and chain have been saved at:
  /etc/letsencrypt/live/yourdomain.com/fullchain.pem
  Your key file has been saved at:
  /etc/letsencrypt/live/yourdomain.com/privkey.pem
  Your certificate will expire on 2023-07-15. To obtain a new or
  tweaked version of this certificate in the future, simply run
  certbot again. To non-interactively renew *all* of your
  certificates, run "certbot renew"
```
### 安装tuic
- 1.从github(https://github.com/EAimTY/tuic/releases) 下载对应操作系统和cpu架构的tuic二进制文件，然后放到服务器上面，比如/usr/local/bin/tuic

- 2.添加配置文件/etc/tuic/config.json，内容如下：

```bash
{
    "server": "[::]:8443",
    "users": {
"1339b0ae-8025-4c76-9b8c-34a6cf63bdbf": "5NbI4QytgInFGX2Q"
    },
    "certificate": "/etc/letsencrypt/live/yourdomain.com/fullchain.pem",
    "private_key": "/etc/letsencrypt/live/yourdomain.com/private.pem",
    "congestion_control": "bbr",
    "alpn": ["h3", "spdy/3.1"],
    "udp_relay_ipv6": false,
    "zero_rtt_handshake": true,
    "dual_stack": true,
    "auth_timeout": "3s",
    "task_negotiation_timeout": "3s",
    "max_idle_time": "10s",
    "max_external_packet_size": 1500,
    "send_window": 16777216,
    "receive_window": 8388608,
    "gc_interval": "3s",
    "gc_lifetime": "15s",
    "log_level": "info"
}
```
- 3.添加service文件/etc/systemd/system/tuic.service，内容如下：
```bash
[Unit]
After=network.target nss-lookup.target

[Service]
User=root
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE CAP_NET_RAW
ExecStart=/usr/local/bin/tuic -c /etc/tuic/config.json
Restart=on-failure
RestartSec=10
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
```
### 开启tuic
    systemctl start tuic
### 设置tuic开机自启
    systemctl enable tuic
### 重启tuic
    systemctl restart tuic
### 查看tuic状态
    systemctl status tuic

# 文章转载自 
## [Payne Fu](https://beyondkmp.com/about)  

## [原文链接](https://beyondkmp.com/2023/04/16/tuic-config/)
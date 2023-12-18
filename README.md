
### 申请域名和证书

- 1.安装certbot

      apt install certbot

- 2.申请证书

      certbot certonly -d yourdomain.com   --standalone

- 3.更新证书

      certbot renew
### 安装tuic
> 1.从github(https://github.com/EAimTY/tuic/releases) 下载对应操作系统和cpu架构的tuic二进制文件，然后放到服务器上面，比如/usr/local/bin/tuic

> 2.添加配置文件/etc/tuic/config.json，内容如下：

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

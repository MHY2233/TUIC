# 介绍

> QUIC 协议汲取了大量人们给 TCP 糊墙的经验教训，把连接结构优化到（目前来看）极致。TUIC是一个基于 QUIC 协议的新代理工具: TUIC。这个工具可以实现的功能如下:
- 1.1-RTT TCP 中继

- 2.0-RTT UDP 中继，且 NAT 类型为 FullCone

- 3.在用户空间的拥塞控制，也就是说可以在任何系统平台实现双向的 BBR

- 4.两种 UDP 中继模式: native （原生 UDP 特性，数据仍被 TLS 加密）和 quic (100% 送达率，每个包单独单独作为一个 QUIC “流”，一个包的确认重传不会阻塞其它包)

- 5.完全多路复用，服务器和客户端之间始终只需要一条 QUIC 连接，所有任务作为这个连接中的 “流” 进行传输（一个流的暂时阻塞不会影响其它流），所以除连接第一个中继任务外的其它任务都不需要经过 QUIC 握手和 TUIC 的鉴权

- 6.网络切换时的会话平滑转移，例如在从 Wi-Fi 切换到移动数据时连接不会像 TCP 一样直接断开

- 7.0-RTT 、与中继任务并行的鉴权

- 8.支持 QUIC 的 0-RTT 握手（开启之后能达到 真・ 1 -RTT TCP 和 0-RTT UDP ，但是就算不开启，多路复用的特性也能保证在绝大多数情况下 1-RTT 和 0-RTT ）
> TUIC 的设计介绍在仓库中 Design 一节有说明。TUIC 协议的详细内容在 这里。简单来说，TUIC 的设计核心就是减少握手造成的网络往返时延（ rtt ），毕竟对于网络程序这是最大的瓶颈。
最后说说安全性和协议特征。TUIC 现在基于原生 QUIC ，不支持 obfs ，但 QUIC 连接本身就是 TLS 加密的，每个 QUIC 连接从外面看都是一样的。国内的各大厂也慢慢开始使用 QUIC 了，所以QUIC的特征其实也没有那么明显了。

# 安装
## 服务器端
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
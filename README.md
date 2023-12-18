# 介绍

> QUIC 协议汲取了大量人们给 TCP 糊墙的经验教训，把连接结构优化到（目前来看）极致。TUIC是一个基于 QUIC 协议的新代理工具: TUIC。这个工具可以实现的功能如下:

> 1.1-RTT TCP 中继

>> 2.0-RTT UDP 中继，且 NAT 类型为 FullCone

>> 3.在用户空间的拥塞控制，也就是说可以在任何系统平台实现双向的 BBR

>> 4.两种 UDP 中继模式: native （原生 UDP 特性，数据仍被 TLS 加密）和 quic (100% 送达率，每个包单独单独作为一个 QUIC “流”，一个包的确认重传不会阻塞其它包)

>> 5.完全多路复用，服务器和客户端之间始终只需要一条 QUIC 连接，所有任务作为这个连接中的 “流” 进行传输（一个流的暂时阻塞不会影响其它流），所以除连接第一个中继任务外的其它任务都不需要经过 QUIC 握手和 TUIC 的鉴权

>> 6.网络切换时的会话平滑转移，例如在从 Wi-Fi 切换到移动数据时连接不会像 TCP 一样直接断开

>> 7.0-RTT 、与中继任务并行的鉴权

>> 8.支持 QUIC 的 0-RTT 握手（开启之后能达到 真・ 1 -RTT TCP 和 0-RTT UDP ，但是就算不开启，多路复用的特性也能保证在绝大多数情况下 1-RTT 和 0-RTT ）
TUIC 的设计介绍在仓库中 Design 一节有说明。TUIC 协议的详细内容在 这里。简单来说，TUIC 的设计核心就是减少握手造成的网络往返时延（ rtt ），毕竟对于网络程序这是最大的瓶颈。
最后说说安全性和协议特征。TUIC 现在基于原生 QUIC ，不支持 obfs ，但 QUIC 连接本身就是 TLS 加密的，每个 QUIC 连接从外面看都是一样的。国内的各大厂也慢慢开始使用 QUIC 了，所以QUIC的特征其实也没有那么明显了。


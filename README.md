## 代理服务
拓扑结构
LAN clients
  192.168.101.0/24
     |
     v
[ Router / Devices ] -> 默认网关设为 192.168.101.112 (旁路服务器)
     |
     v
[ 旁路服务器 192.168.101.112 ]
  - ip_forward = 1
  - WARP client (cloudflare-warp)  -> external internet (Cloudflare WARP egress)
  - 3proxy (HTTP 8888 / SOCKS5 1080)
  - redsocks (127.0.0.1:12345)   # 将被 iptables NAT 重定向到本地
  - iptables/nftables rules:
      PREROUTING (LAN -> REDIRECT tcp -> redsocks)
      POSTROUTING (-o cloudflare-warp -> MASQUERADE)
     |
     v
[Cloudflare WARP egress IP] -> Internet

### 详细说明见 proxy_warp_full_tutorial.html文件

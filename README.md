# WireGuard + Clash 配合使用指南

将 WireGuard 隧道与 Clash 代理结合，实现更灵活的组网方案。

## 应用场景

- 家庭组网: 多设备通过 WireGuard 连接到 VPS，再通过 Clash 出海
- 企业内网: 分支机构通过 WireGuard 加入统一内网
- 移动端优化: WireGuard 建立隧道，Clash 处理分流

## 架构设计

```
手机/电脑 > --WireGuard隧道-- > VPS > --Clash-- > 互联网
   ^                             |
   + --------- VPN 内网 ---------+
```

## 方案一：Clash 处理所有流量

VPS 配置 WireGuard 接收连接，Clash 处理出站：

```bash
# VPS 安装 WireGuard
apt install wireguard -y

# VPS wg0.conf
[Interface]
PrivateKey = <vps-private-key>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <client-public-key>
AllowedIPs = 10.0.0.2/32

# 开启 IP 转发
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -A FORWARD -i wg0 -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

## 方案二：分流模式

只有特定流量走代理，其他走 WireGuard 直连：

```yaml
rules:
  # WireGuard 内网直连
  - IP-CIDR,10.0.0.0/24,DIRECT
  # 国内直连
  - GEOIP,CN,DIRECT
  # 其他走代理
  - MATCH,Proxy
```

## 常见问题

**WireGuard 连不上？** 检查服务端防火墙是否放行 UDP 51820 端口。

**和 Clash TUN 冲突？** Clash TUN 接管所有流量，建议分开使用或禁用其中一个的 TUN。

---

推荐工具：

- [Clash for Windows](https://clashforwindows.site/)
- [ClashMI](https://clashmi.site/)
- [FlClash](https://flclash.us/)

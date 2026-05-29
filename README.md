[English] Deploy Clash on OpenWrt router for whole-network transparent proxy.

## Requirements

- OpenWrt 21.02 or higher
- At least 8MB Flash + 128MB RAM

## Installation

```bash
opkg update
wget https://github.com/frainzy1477/clash/releases/...
mv clash-linux-arm64 /usr/bin/clash
chmod +x /usr/bin/clash
mkdir -p /etc/clash
```

Recommended Tools

- [Clash for Windows](https://clashforwindows.site/) - Most popular Clash GUI for Windows
- [ClashMI](https://clashmi.site/) - Lightweight multi-platform Clash client
- [FlClash](https://flclash.us/) - Modern proxy tool with multi-protocol support
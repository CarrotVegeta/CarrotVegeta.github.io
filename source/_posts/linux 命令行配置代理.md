---
title: linux 命令行配置代理
date: 2022-02-16 15:52:21.277
updated: 2022-02-16 15:52:21.277
url: /archives/linuxming-ling-xing-pei-zhi-dai-li
categories: 
- linux
tags: 
- 代理
- linux
- bash
---

# 命令行配置代理

```bash
export http_proxy=http://127.0.0.1:1087

export https_proxy=$http_proxy
```

## 此处的IP和端口需要根据具体你代理的实际信息填写，可查看VPN客户端的config.json

```json
"inbounds": [
 {
 "listen": "127.0.0.1",
 "protocol": "socks",
 "settings": {
 "udp": false,
 "auth": "noauth"
 },
 "port": "1080"
 },
 {
 "listen": "127.0.0.1",
 "protocol": "http",
 "settings": {
 "timeout": 360
 },
 "port": "1087"
 }
```

检查命令行配置是否完成

```bash
curl -i www.google.com
```

收到200响应表示成功
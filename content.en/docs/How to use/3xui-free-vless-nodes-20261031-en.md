---
title: "3x-ui Free VLESS Nodes Share (Last Updated October 31, 2026)"
description: "Risk analysis of free VLESS nodes, filtering recommendations, and the latest node list. Also includes recommended traffic-based paid airport services for long-term stable use."
keywords:
  - free VLESS nodes
  - 3x-ui free nodes
  - VLESS node share
  - free proxy risks
  - airport recommendation
  - traffic-based airport
weight: 10
---

# 3x-ui Free VLESS Nodes Share (Last Updated October 31, 2026)

## 1. Can You Use Free VLESS Nodes?

| Risk | Specific Manifestation | Severity |
| --- | --- | --- |
| Privacy leakage | Operator may record connection IP, time, target domain and other metadata | High |
| Traffic monitoring | Operator can observe unencrypted network communications | High |
| Malicious DNS | Domain names may be resolved to phishing or malicious websites | High |
| Man-in-the-middle attack | Insecure TLS configuration may allow communication to be intercepted or tampered with | High |
| Sudden node failure | Free nodes frequently change IP, port or password | Medium |
| Account security | May lure you to visit fake websites and enter account credentials | High |
| Unstable lines | Congestion, rate limiting and disconnections during peak hours | Medium |

**Special Reminder:**

The fact that VLESS uses encryption does **not** mean that free node operators cannot observe your network activities.

For example:

- The specific page content of HTTPS websites is usually protected by TLS.
- Node operators can still see connection time, traffic volume, target IP and other information.
- If DNS is not properly encrypted, domain queries may also be exposed.
- Plain HTTP traffic may be directly read or tampered with.

## 2. How Should You Filter Free VLESS Nodes?

It is recommended to check at least the following points:

| Check Item | Recommendation |
| --- | --- |
| security | Must have TLS or Reality |
| TLS certificate | Normal verification; do not skip verification |
| allowInsecure | Must be false |
| Node source | Prefer projects with maintenance records |
| DNS | Use trusted encrypted DNS |
| Client | Use official Shadowrocket |
| Permissions | Do not install unknown certificates or configuration profiles required by the node provider |

**Special Notes:**

Do not assume a node is safe just because it can connect.  
Also, do not install root certificates, unknown VPN configuration files, or mobile management profiles required by free node providers.

The following nodes are collected from the internet. Please use them with caution:

```text
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.130.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443%3Fed%3D2560#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.17.148.22:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%F0%9F%87%BA%F0%9F%87%B8%20%E7%BE%8E%E5%9B%BD%20%7C%20USA
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTELEGRAM-TOOTFFARANGI#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTelegram---TEHRANARGO#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@203.88.117.190:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@45.61.149.205:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443fp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%23TELEGRAM--MARAMBASHI--MARAMBASHI--MARAMBASHI--MARAMBASHI%3Fed#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@2.28.42.209:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443fp%3Dchrome#%E5%BE%B7%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@saas.sin.fan:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40vpn748359672.opengw.net%3A1458%3Fed%3D2560#%E7%BE%8E%E5%9B%BD23
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@45.61.149.205:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%F0%9F%87%BA%F0%9F%87%B8%20%E7%BE%8E%E5%9B%BD%20%7C%20USA
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@2.28.42.209:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E5%BE%B7%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTelegram---bored_vpn#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.194.140.26:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E8%8B%B1%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E5%B7%B4%E8%A5%BF%E5%9C%A3%E4%BF%9D%E7%BD%97%2BCloudFlare%E8%8A%82%E7%82%B9
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@saas.sin.fan:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.245.245.16:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E6%B3%A2%E5%85%B062
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@144.31.181.177:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E5%BE%B7%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.17.148.22:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTELEGRAM--i10VPN%3Fed#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@130.78.190.134:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E8%8D%B7%E5%85%B062
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdipfp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@103.39.78.118:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E9%A6%99%E6%B8%AF62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD%2BCloudFlare%E8%8A%82%E7%82%B9
```

## 3. Using Airport Proxy Services

The following airports charge by traffic. The websites include software usage and installation tutorials.  
After purchasing traffic, there is no time limit — traffic lasts until it is used up.  
If a website becomes inaccessible, it means it has been blocked; switch to another site.

| Name | Price | Traffic | Nodes |
| :--- | :--- | :--- | :--- |
| [魔戒](https://1.jnk.ink/L4q20S) | 1 CNY | 1G | 30 |
| [网际快车](https://1.jnk.ink/ad2RVl) | 7 CNY | 20G | 54 |
| [牛逼](https://1.jnk.ink/LYet7x) | 14 CNY | 200G | 31 |
| [飞鸟](https://1.jnk.ink/i7OhaC) | 10 CNY | 200G | 25 |
| [happy猫](https://1.jnk.ink/5KiTxY) | 20 CNY | 200G | 27 |
| [农夫山泉](https://1.jnk.ink/i1fXTMYk) | 45 CNY | 200G | 40 |
| [宝贝云](https://1.jnk.ink/xxPwfy) | 55 CNY | 600G | 64 |
| [飞兔](https://1.jnk.ink/bbXkiN) | 30 CNY | 100G | 80 |
| [fscloud](https://1.jnk.ink/nKXcqQ) | 14 CNY/year | 100G/month | 30 |
| [冲上云霄](https://1.jnk.ink/gXM8T) | 7 CNY/month | 50G | 28 |

---
title: "3x-ui Setup Guide: Build Your Own Clash / v2ray Proxy Node (VLESS + Reality)"
description: "3x-ui is an open-source web panel for Xray-core, supporting VLESS, VMess, Trojan, Shadowsocks, Reality, XHTTP and other mainstream protocols. This step-by-step tutorial walks you through buying a VPS, connecting to the server, installing the 3x-ui panel, configuring a VLESS + Reality node, importing it into Clash / v2ray clients, and enabling BBR acceleration."
keywords:
  - 3x-ui
  - 3x-ui tutorial
  - 3x-ui setup
  - Xray
  - Xray panel
  - VLESS
  - Reality
  - XHTTP
  - self-hosted proxy
  - build proxy node
  - Clash node
  - v2ray node
  - VPN setup
  - BBR acceleration
  - VPS tutorial
type: docs
---

Building your own Clash or v2ray node with 3x-ui is easier than it looks. This guide takes you all the way from **buying a VPS** to **importing the node** into a proxy client, covering the full 3x-ui panel installation, VLESS + Reality node configuration, and BBR acceleration. It's aimed at beginners who want to self-host a proxy node without hand-editing Xray config files.

## What is the 3x-ui panel?

3x-ui is an open-source **Xray panel** that manages Xray-core (a full-featured proxy core) through a web interface. It's an enhanced fork built on the earlier x-ui project and is currently one of the most actively maintained and feature-complete options in this family.

![3xui-1002.jpg](https://3x-ui.pro/img/3xui-1002.jpg)

Its core features are roughly as follows:

**Multi-protocol support** — mainstream protocols such as VLESS, VMess, Trojan, Shadowsocks, and Wireguard can all be configured, along with newer anti-blocking transports like XTLS/Reality.

**Web management panel** — no need to hand-write Xray JSON configs; you can add or remove inbounds, generate users, and configure transport-layer parameters directly in the browser.

**Multi-user and traffic management** — you can set per-user traffic caps, expiry dates, and concurrent IP limits, making it suitable both for personal use and for distributing to multiple people.

**Monitoring and operations** — the panel shows traffic statistics and system load, supports Telegram bot alerts, and comes with built-in certificate issuance and subscription-link generation.

**Simple deployment** — a single install script or Docker gets it running, which pairs well with common VPS options like Debian/Ubuntu.

It wraps Xray's originally low-level, config-file-editing workflow into a visual dashboard, lowering the barrier to building and managing proxy nodes.

Below, step by step, you'll learn how to build your own VPN node — from buying the server all the way to importing the node into your proxy software!

## 1. Buy a VPS

### 1.1 Using akile as an example (Hong Kong and US servers)

- akile's plans are as follows:

| Series / Node | CPU | RAM | Disk | Bandwidth | Monthly Traffic | Ref. Price | Route / Notes |
|---|---|---|---|---|---|---|---|
| [**HKLite-One** (Hong Kong Lite)](https://akile.ai/shop/server?type=traffic&areaId=3&nodeId=1&planId=811&aff_code=cbb004e0-a47c-440e-a767-2ede890c4e4f) | 1 core | 1 GB | 10 GB SSD | 1 Gbps | 2500 GB | ¥24.99/mo | Hong Kong NTT/PCCW international, built-in Akile DNS unlock, fairly oversold |
| [**LAX-Lite** (Los Angeles International)](https://akile.ai/shop/server?type=traffic&areaId=2&nodeId=37&planId=1009&aff_code=34c72b4e-7d9b-41cd-bee4-be72d273fecf) | 1 core | 1 GB | 5 GB SSD | 5 Gbps port | 1 TB | ¥8.88/mo | Pure overseas international route, no mainland optimization; throttles to shared 10 Mbps unlimited after the cap |
| [**LAX4837-ISP** (Los Angeles Dual-ISP Residential)](https://akile.ai/shop/server?type=traffic&areaId=2&nodeId=37&planId=1009&aff_code=cbb004e0-a47c-440e-a767-2ede890c4e4f) | 1 core | 2 GB | 10 GB SSD | 1 Gbps | 1 TB | ¥49.99/mo | Native static IP, dual-ISP residential attributes, three-network CUVIP(4837) return route, EPYC host |

- Here we'll buy the ¥8.88/month plan.
- Note: choose **Debian 11 or Debian 12** as the operating system.

![3xui-1001.jpg](https://3x-ui.pro/img/3xui-1001.jpg)

### 1.2 Start the server

- After a successful payment, wait about 3 minutes for the server to finish provisioning.
- Click the cloud server list and open the server we just created.
- If the server hasn't started, follow the arrow prompts to start it.

![3xui-1003.jpg](https://3x-ui.pro/img/3xui-1003.jpg)

## 2. Connect to the Server

### 2.1 Download the server connection tool: FinalShell

- [Official download page](https://www.hostbuf.com/t/988.html)
- [Cloud drive download](https://pan1.mene.lol/s/WvoFP)

The software interface is shown below:

![3xui-1004.jpg](https://3x-ui.pro/img/3xui-1004.jpg)

### 2.2 Use the software to connect to the server

- Click the folder shown in the picture.

![3xui-1005.jpg](https://3x-ui.pro/img/3xui-1005.jpg)

- Select the first icon in the top-left corner to create a Linux server connection.

![3xui-1006.jpg](https://3x-ui.pro/img/3xui-1006.jpg)

- Once opened, fill in fields 1–4 as shown.

![3xui-1007.jpg](https://3x-ui.pro/img/3xui-1007.jpg)

- Field 1: enter a server name — any name you like will do.
- Field 2: enter the host's IP address. Open the [server dashboard](https://akile.ai/console/server) we bought earlier, find the IP address, and fill it in.
- After opening the page, click Cloud Server, copy the server address as shown, and paste it into field 2.

![3xui-1008.jpg](https://3x-ui.pro/img/3xui-1008.jpg)

- Field 3: enter the server username — the default is usually `root`.
- Field 4: scroll down the page, find Access Control, click View Password, and enter the password into field 4.
- Finally, click OK to save the server details.

![3xui-1009.jpg](https://3x-ui.pro/img/3xui-1009.jpg)

- After saving, you'll return to the connection window. Click the server to connect.

![3xui-10091.jpg](https://3x-ui.pro/img/3xui-10091.jpg)

- On the first connection, a security warning pops up — click Accept and Save.
- Under normal circumstances, you'll then see the "connection successful" screen.

![3xui-10092.jpg](https://3x-ui.pro/img/3xui-10092.jpg)

- If the connection doesn't succeed, open the server page and check whether the server is powered on.
- If it's off, click here to power it on, wait 3 minutes, and connect again.

![3xui-1003.jpg](https://3x-ui.pro/img/3xui-1003.jpg)

- At this point, our server is successfully connected.
- In the following steps, I'll show you how to set up 3x-ui.

![3xui-10093.jpg](https://3x-ui.pro/img/3xui-10093.jpg)

## 3. Install the 3x-ui Panel

### 3.1 Update the software

- Copy the command below:

```
apt update
```

- Paste it into FinalShell, as shown:

![3xui-10094.jpg](https://3x-ui.pro/img/3xui-10094.jpg)

- Then press the Enter key.
- That completes running one command.
- To install 3x-ui, we also need to run another command.
- Copy and run the command below to install curl:

```
apt install curl -y
```

### 3.2 Install 3x-ui

- Copy and run the command below.
- Here we install version 3.4.2, because it's currently the most stable release with the fewest bugs.

```
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) v3.4.2
```

- In the image below, for the prompt to choose a database, select `1` and press Enter.

![3xui-10095.jpg](https://3x-ui.pro/img/3xui-10095.jpg)

- In the image below, for this step, just press Enter.

![3xui-10096.jpg](https://3x-ui.pro/img/3xui-10096.jpg)

- In the image below, at this step: if you have a domain, choose `1` and press Enter.
- If you don't have a domain, choose `2` and press Enter — for personal users there's no difference.
- Here we choose `2` and press Enter; if you buy a domain later, you can add it then.

![3xui-10097.jpg](https://3x-ui.pro/img/3xui-10097.jpg)

- In the image below, when asked whether to use IPv6, just press Enter.

![3xui-10098.jpg](https://3x-ui.pro/img/3xui-10098.jpg)

- In the image below, when asked about using port 80 to request a certificate, just press Enter.

![3xui-10099.jpg](https://3x-ui.pro/img/3xui-10099.jpg)

- In the image below, seeing this information means the certificate request succeeded.
- Left-click to select this text, then right-click to copy and save it.
- This is your password and login URL information, which we'll need shortly.

![3xui-100991.jpg](https://3x-ui.pro/img/3xui-100991.jpg)

## 4. Configure 3x-ui: Add a VLESS + Reality Node

### 4.1 Log in to the 3x-ui web admin panel

- In the image below, copy the login URL we just saved and open it in a browser.

![3xui-100992.jpg](https://3x-ui.pro/img/3xui-100992.jpg)

- The login information we saved earlier contains the username and password.
- Enter them and log in to access the 3x-ui backend.

![3xui-100993.jpg](https://3x-ui.pro/img/3xui-100993.jpg)

### 4.2 Add and configure a VLESS node

- Click Inbounds in the top-left corner --- Add Inbound, which brings up the screen below.

![3xui-100994.jpg](https://3x-ui.pro/img/3xui-100994.jpg)

- Remark — enter any name here, e.g. US--vless--001.
- Protocol — select `vless`.
- Leave the other options unchanged, then click the Transmission tab (shown below).

![3xui-100995.jpg](https://3x-ui.pro/img/3xui-100995.jpg)

- Transmission — select `xhttp`.
- Path — Custom — enter: `xhttp1`.
- Keep the rest at their defaults, then click the Security tab (shown below).

![3xui-100996.jpg](https://3x-ui.pro/img/3xui-100996.jpg)

- Security — select `reality`.
- Dest — click Search Dest and click the first result.
- Click Create to finish adding the inbound, as shown below.

![3xui-100997.jpg](https://3x-ui.pro/img/3xui-100997.jpg)

- Click Clients in the top-left corner --- Add Client.
- Click the "Associated Inbound" input box and select the inbound we just created, as shown below.
- Click Create.

![3xui-100998.jpg](https://3x-ui.pro/img/3xui-100998.jpg)

- At this point, we've finished creating the VLESS node, as shown below.

![3xui-100999.jpg](https://3x-ui.pro/img/3xui-100999.jpg)

- Click the QR code in the image — be sure to select the VLESS node info, not the subscription info.
- You'll then see the VLESS node QR code, as shown below.

![3xui-1009991.jpg](https://3x-ui.pro/img/3xui-1009991.jpg)

- On iPhone, use [Shadowrocket](https://shadowrocket.ink/zh) or [Karing](https://karing.biz/zh) to import the node.
- On Android, use [v2rayNG](https://v2rayng.4566.lol/zh) or Karing to import the node.
- On Windows, use [v2rayN](https://v2rayn.4566.lol/zh) or Hiddify to import the node.
- On Mac, use [Karing](https://karing.biz/zh) to import the node.

## 5. Connect to the Node

### 5.1 Import the node on Android

- We'll use Android as the example to demonstrate importing the node.
- If you're using an Android phone, download and install the software below.
- v2rayNG download: https://pan1.mene.lol/s/JQjC0
- Open the app, tap the plus sign in the top-right corner --- Scan QR code.
- Scan the QR code above to import the node.

![3xui-1009992.jpg](https://3x-ui.pro/img/3xui-1009992.jpg)

- Tap the button in the bottom-right corner to connect to the node's network.
- Tap Test to check whether the connection is working.

![3xui-1009993.jpg](https://3x-ui.pro/img/3xui-1009993.jpg)

### 5.2 Enable BBR acceleration

- In FinalShell, type the command `x-ui`.
- You'll see the panel management menu, as shown below.

![3xui-1009994.jpg](https://3x-ui.pro/img/3xui-1009994.jpg)

- Select `26` and press Enter, as shown below.
- Then select `1` and press Enter to enable BBR acceleration.

![3xui-1009995.jpg](https://3x-ui.pro/img/3xui-1009995.jpg)

- You can see the download latency dropped to 308 ms.

![3xui-1009996.jpg](https://3x-ui.pro/img/3xui-1009996.jpg)

### 5.3 Recommended proxy providers ("airports")

- If a single IP address isn't enough for you,
- you can buy cheap "airport" (proxy subscription) nodes.
- Here I recommend the WangJiKuaiChe airport — [click to open it](https://1.jnk.ink/ad2RVl).
- Register an account with your own email.

![3xui-1009997.jpg](https://3x-ui.pro/img/3xui-1009997.jpg)

- After logging in, click Store --- choose the no-time-limit plan.
- It pairs very conveniently with the node we built ourselves.

![3xui-1009998.jpg](https://3x-ui.pro/img/3xui-1009998.jpg)

* The airports below are pay-per-traffic; each site has software usage and installation tutorials.
* Once you buy traffic, there's no time limit — it lasts until the traffic runs out.
* If a site is unreachable, it has been blocked; just switch to another one.

| Name | Price | Traffic | Nodes |
| :--- | :--- | :--- | :--- |
| [MoJie](https://1.jnk.ink/L4q20S) | ¥1 | 1G | 30 |
| [WangJiKuaiChe](https://1.jnk.ink/ad2RVl) | ¥7 | 20G | 54 |
| [NiuBi](https://1.jnk.ink/LYet7x) | ¥14 | 200G | 31 |
| [FeiNiao](https://1.jnk.ink/i7OhaC) | ¥10 | 200G | 25 |
| [Pikachu](https://1.jnk.ink/d07dCA) | ¥15 | 20G | 40 |
| [Happy Cat](https://1.jnk.ink/5KiTxY) | ¥20 | 200G | 27 |
| [NongFuShanQuan](https://1.jnk.ink/i1fXTMYk) | ¥45 | 200G | 40 |
| [BaoBeiYun](https://1.jnk.ink/xxPwfy) | ¥55 | 600G | 64 |
| [ZiYouMao](https://1.jnk.ink/haO8Dr) | ¥89 | 200G | 71 |
| [FeiTu](https://1.jnk.ink/bbXkiN) | ¥30 | 100G | 80 |

## Frequently Asked Questions (FAQ)

**Which protocols does 3x-ui support?** 3x-ui is built on Xray-core and supports VLESS, VMess, Trojan, Shadowsocks, and Wireguard, along with anti-blocking transports such as XTLS/Reality and XHTTP.

**Do I need a domain to set up 3x-ui?** No. For personal users, VLESS + Reality delivers strong anti-blocking without a domain; if you buy a domain later, you can add it in the panel at any time.

**Why install version 3.4.2?** This tutorial uses 3.4.2 because it's currently a stable release with few known bugs, making it a safe choice for beginners.

**What does BBR acceleration do?** Enabling BBR optimizes TCP congestion control and usually noticeably lowers latency while improving the download and upload speed of your proxy node.

---
title: "Build Residential IP Proxy Nodes with 3X-UI Panel: Complete Guide"
description: "A step-by-step tutorial on using the 3X-UI panel to import residential IPs (SOCKS5), set up proxy chaining, and create self-hosted residential IP proxy nodes. Ideal for TikTok account nurturing, matrix operations, live streaming, and more."
keywords: ["3X-UI", "Residential IP", "Self-hosted Node", "Proxy Chaining", "SOCKS5", "TikTok", "Proxy Node"]
weight: 8
---

**After the [3x-ui](https://3x-ui.pro/zh) panel is set up, you will see many protocols on the panel, such as [vless](https://vless.app/zh), Trojan, HY2, SOCKS5, etc. You will also encounter many types of IP addresses, such as data center IPs, native IPs, residential IPs, high-risk IPs, and more. If you haven’t installed 3x-ui yet, [please read this article](https://3x-ui.pro/zh).**  
**For mainland Chinese users running TikTok operations (especially account nurturing, matrix accounts, live streaming, and TikTok Shop), it is strongly recommended to use residential IPs instead of ordinary data center / colocation IPs.**

### What is a Residential IP?
A Residential IP (also called home broadband IP or ISP IP) is an IP address assigned by local Internet Service Providers (such as AT&T, Verizon, Comcast in the United States) to real home broadband users.

- **Characteristics**: It looks like an ordinary home user browsing the internet at home. The ASN belongs to the carrier, and the geographic location and routing information are closer to real users.
- **Compared to Data Center IP**: Data center IPs come from cloud servers / data centers (AWS, Alibaba Cloud, Vultr, etc.). They are fast and cheap, but platforms can easily identify them as “non-human / bulk operations,” which often triggers rate limiting, verification, or even account bans.

**Why prioritize residential IPs for TikTok operations?**  
TikTok’s risk control comprehensively evaluates IP type, historical reputation, geographic consistency, device fingerprints, and behavioral trajectories. Data center IPs are often marked as suspicious, leading to 0 views, rate limiting, or associated account bans. Residential IPs have higher trust and are more suitable for registration, account nurturing, posting, live streaming, and opening shops. Static residential IPs (fixed and exclusive) are best for main accounts and long-term operations; dynamic residential IPs are suitable for bulk registration or testing.

In practice, high-quality static residential IPs are relatively expensive (ranging from dozens to hundreds of RMB per month per IP), but they are more cost-effective than losing accounts. Note that there are “pseudo-residential / dual-ISP” resources on the market with varying purity levels. Use tools (such as ping0.cc) to check whether they truly have ISP attributes and whether the risk score is low.

### What is Proxy Chaining?
Proxy Chaining (also called multi-level proxy or proxy chain) means that network requests **pass through multiple proxy nodes in sequence**, rather than just one.

Simple path example:  
Your device → Proxy A (e.g., domestic or transit node) → Proxy B (relay) → Proxy C (residential IP in the target country) → TikTok servers

Each node only knows the adjacent nodes before and after it. The target website ultimately only sees the IP of the last hop. The real IP is isolated by multiple layers.

### Why learn Proxy Chaining?
In TikTok / cross-border operations, a single proxy is sometimes not stable enough or lacks sufficient anonymity. The main value of proxy chaining includes:

1. **Higher anonymity and anti-association**: Multi-layer isolation makes it harder for the platform to trace the real source or associate multiple accounts, suitable for matrix operations.
2. **Bypassing complex restrictions**: Direct connections from mainland China to overseas are sometimes unstable or interfered with. Using a transit node (e.g., first going through a high-quality dedicated line / airport, then exiting through a residential IP in the target country) can improve connectivity and stability.
3. **Redundancy and fault tolerance**: If one layer has a problem, you can switch, reducing the risk of single-point failure.
4. **More flexible disguise**: Combining different types of IPs (e.g., data center transit + residential exit). The exit must be a clean residential / ISP IP to truly reduce risk control.

### 1. Purchase a Residential IP Address

- Here we will purchase a US residential IP from RareCloud, about $5 per month
- [Click to open the RareCloud official website](https://rarecloud.io/clients/aff_redirect.php?aff=738&to=https%3A%2F%2Frarecloud.io%2Fresidential-proxy%2F&sig=d9f005ec886586a2c137ff75bb8e9bd9f72cb62ae1dc9c334f8f1ec03ae17f79)
- Select static residential IP address proxy and choose monthly payment


![3xui-zhuzhai-1008.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1008.jpg)


- After successful purchase, leave a message to customer service to obtain the IP address, username, and password


### 2. Import the SOCKS5 Residential IP Proxy into the 3X-UI Panel

- Log in to the 3X-UI panel, then click Xray Configuration, and click Outbound


![3xui-zhuzhai-1001.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1001.jpg)

- Select the protocol as SOCKS, write any tag (here we write racknerd)
- Fill in the address with the SOCKS proxy IP address we just purchased
- Fill in any port, and enter the username and password we just bought. Finally click Create

![3xui-zhuzhai-1002.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1002.jpg)



### 3. Set Up Proxy Chaining

- Below we set up proxy chaining
- Click Routing Rules, then click Inbound Tags, and select all
- For Outbound Tags, select the previously created racknerd


![3xui-zhuzhai-1003.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1003.jpg)

![3xui-zhuzhai-1004.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1004.jpg)

![3xui-zhuzhai-1005.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1005.jpg)

### 4. Test the Residential IP Proxy

- Since we selected all inbound tags
- That means all nodes will go through the SOCKS proxy

- Below we randomly select a node for testing
- First, restart Xray
- Then, in [v2rayN](https://vrayn.4566.lol/zh), right-click the node and click Test Real Latency
- If there is latency, it means our proxy chaining setup was successful


![3xui-zhuzhai-1006.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1006.jpg)

![3xui-zhuzhai-1007.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1007.jpg)

![3xui-zhuzhai-1009.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1009.jpg)

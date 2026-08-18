---
title: "用 3X-UI 自建 Hysteria2 节点：从买 VPS 到端口跳跃的完整流程"
description: "手把手记录用 3X-UI 面板在自己的 VPS 上搭建 Hysteria2（HY2）节点、开启端口跳跃，并在 Clash 系客户端里导入使用的整个过程。"
keywords: ["3X-UI", "Hysteria2", "HY2", "自建节点", "端口跳跃", "VPS"]
weight: 8
---

如果你已经用惯了 Shadowrocket 之类的客户端，早晚会碰到"想自己搭一个节点"的念头——
要么是不想依赖别人提供的服务，要么是想按自己的需求调参数。这篇记录的是用 [3X-UI](https://3x-ui.pro/zh)
这个开源面板，在一台自己的 VPS 上搭建 Hysteria2（简称 HY2）协议节点、并开启端口跳跃
的完整流程，从买机器一路写到客户端能正常连上。

## 一、自己建节点的优势

跟直接用别人提供的现成服务比，自己搭节点主要有这么几点好处：	

- **参数完全自己说了算**：协议、端口、混淆方式、证书、流量限额都能自己配置，
  不用迁就别人定好的规则。
- **不用担心节点突然被别人限速或者停用**：机器和面板都在自己手里，稳定性只取决于
  VPS 本身和自己的维护，不存在"服务商说停就停"的情况。
- **可以按需要横向扩展**：一台面板可以同时管理多个入站、多个客户端，方便按用途
  拆分节点，也方便自己排查问题。

当然，自建也有成本——需要自己买机器、自己维护面板和证书续期，出问题了也得自己
排查。适不适合，取决于你愿不愿意花这份精力。

## 二、Hysteria2 协议的优势

Hysteria2（HY2）是这几年比较受欢迎的一个协议，简单说说它的几个特点：

- **基于 QUIC，天然抗丢包**：底层用的是 UDP + QUIC，在弱网、丢包率高的线路上，
  体验通常比传统 TCP 类协议更稳。
- **速度上限高**：协议本身针对高带宽场景做了优化，跑满带宽、传大文件的时候
  表现比较好。
- **支持端口跳跃（Port Hopping）**：客户端和服务端可以在一段端口范围内动态切换
  实际使用的端口，能在一定程度上降低被针对性限速、封锁的概率，这也是本篇后面
  要重点配置的部分。

## 三、准备工作：VPS 和域名

### 3.1 买一台 VPS

自建节点第一步是要有一台海外 VPS。下面这几款是我自己实际用过、感觉配置和价格
还算合适的机型，附上的是我的推广链接（用这几个链接下单，我会拿到一点返佣，
链接本身经过跳转解析，最终落地在 VPS 服务商 akile.ai 的官方购买页面）：

| 系列 / 节点 | CPU | 内存 | 硬盘 | 带宽 | 月流量 | 参考价 | 线路 / 特点 |
|---|---|---|---|---|---|---|---|
| [**HKLite-One**（香港轻量）](https://1.jnk.ink/ks2wX) | 1 核 | 1 GB | 10 GB SSD | 1 Gbps | 2500 GB | ¥24.99/月 | 香港 NTT/PCCW 国际线路，内置 DNS 解锁，超售较重 |
| [**LAX-Lite**（洛杉矶国际）](https://1.jnk.ink/fnSih) | 1 核 | 1 GB | 5 GB SSD | 5 Gbps 端口 | 1 TB | ¥8.88/月 | 纯海外国际线路，无大陆优化，超限速后转共享 10Mbps 不限量 |
| [**LAX4837-ISP**（洛杉矶双 ISP 住宅）](https://1.jnk.ink/SEwxN) | 1 核 | 2 GB | 10 GB SSD | 1 Gbps | 1 TB | ¥49.99/月 | 原生静态 IP、双 ISP 家宽属性、回程线路较优、宿主机为 EPYC |

新手建议先从便宜的机型练手，把整套流程走通了，再考虑要不要换更贵的线路。

### 3.2 买一个域名

给节点配证书用得上一个域名，我自己是在 [Porkbun](https://porkbun.com/) 上买的，
界面简单、续费也不算贵，你也可以换成自己习惯的域名商。

### 3.3 用 Cloudflare 把域名解析到 VPS 的 IP

买完域名后，去 Cloudflare 加一条 A 记录，把域名指向你 VPS 的 IPv4 地址就行，
类型选 `A`，名称填子域名前缀（比如下图里的 `toto`），"代理状态"先关掉（只走 DNS，
不走 Cloudflare 代理），这样后面申请 Let's Encrypt 证书的时候不会被挡住。

![在 Cloudflare 添加 A 记录，把域名解析到 VPS 的 IP](https://shadowrocket.ink/img/3x-ui.jpg)

### 3.4 Ping 一下，确认 IP 是通的

解析生效之后，先在本地用 `ping` 测一下这台 VPS 的 IP，确认延迟和丢包率在正常范围，
心里有个底再往下走。

![用命令行 ping 一下 VPS 的 IP，确认网络是通的](https://shadowrocket.ink/img/3x-ui-11002.jpg)

### 3.5 用远程连接工具连上 VPS

接下来用 FinalShell（或者 Xshell、iTerm 之类你习惯的工具）新建一个 SSH 连接，
主机填 VPS 的 IP，端口默认 22，用户名 `root`，密码用你在购买页面拿到的初始密码，
连上之后就可以开始装环境了。

![用 FinalShell 新建 SSH 连接，连接到 VPS](https://shadowrocket.ink/img/3x-ui-11003.jpg)

## 四、安装 3X-UI

3X-UI 是一个开源的 Web 控制面板，用来管理 Xray-core 服务，界面比较友好、支持
多语言，能在一个面板里管理多个协议、多个入站，比纯命令行操作方便很多。

![3X-UI 项目介绍页面](https://shadowrocket.ink/img/3x-ui-11004.jpg)

### 4.1 安装命令

SSH 连上 VPS 之后，先更新系统、装好依赖，再跑官方安装脚本：

```bash
apt update -y
apt install -y curl socat
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

安装脚本会依次问几个问题：

- **数据库类型**：客户端数量不多的话选默认的 SQLite 就够用。

![安装脚本询问数据库类型，默认选 SQLite](https://shadowrocket.ink/img/3x-ui-11005.jpg)

- **是否自定义面板端口**：不填的话会随机生成一个端口，出于安全考虑，建议自己
  设置一个不容易被猜到的端口。

![选好数据库后，脚本询问是否自定义面板端口](https://shadowrocket.ink/img/3x-ui-11006.jpg)

- **SSL 证书设置**：这里有几种方式，因为前面已经把域名解析好了，选"用域名申请
  Let's Encrypt 证书"这个选项最省事，证书会自动续期。

![SSL 证书设置菜单，几种申请方式可选](https://shadowrocket.ink/img/3x-ui-11007.jpg)

选完之后输入你自己解析好的域名，脚本会自动去申请证书：

![选择用域名申请证书后，输入自己的域名](https://shadowrocket.ink/img/3x-ui-11008.jpg)

证书申请成功后，脚本会把证书和私钥的存放路径打印出来，同时问你要不要修改
`--reloadcmd`（证书续期后自动重启面板的命令），一般直接用默认值即可：

![证书申请成功，脚本打印出证书路径](https://shadowrocket.ink/img/3x-ui-11009.jpg)

装完之后，脚本会顺带把 fail2ban（防暴力破解）也配置好，并给出一个管理菜单，
里面能看到启动、停止、重启、查看当前设置等常用操作：

![安装完成后弹出的管理菜单，包含启动/停止/重启等选项](https://shadowrocket.ink/img/3x-ui-110091.jpg)

菜单往下翻还有证书管理、防火墙管理、开启 BBR 加速、测速等选项，面板和 Xray
的运行状态也会显示在最下面：

![管理菜单的后半部分，包含证书管理、防火墙、BBR、测速等](https://shadowrocket.ink/img/3x-ui-110092.jpg)

选"查看当前设置"能看到面板端口、数据库路径、访问地址这些信息，自己记一下，
后面登录面板要用：

![查看当前面板设置，包含端口、数据库路径、访问地址](https://shadowrocket.ink/img/3x-ui-110093.jpg)

### 4.2 登录面板，打开订阅功能

用上一步拿到的访问地址打开面板，输入安装时设置的用户名密码登录：

![3X-UI 登录页面](https://shadowrocket.ink/img/3x-ui-110094.jpg)

登录进去之后，首页能看到 CPU、内存占用，以及 Xray、面板自身的运行状态，
先确认这些都是正常的：

![登录后的系统状态仪表盘，显示 CPU、内存、Xray 运行状态](https://shadowrocket.ink/img/3x-ui-110095.jpg)

接着去"面板设置 → 常规"，把"启用订阅服务"和"Clash / Mihomo 订阅"都打开，
监听端口按自己的习惯设置（比如下图里的 2096），这样后面客户端才能通过一条
订阅链接批量拿到节点配置，不用每次手动填参数：

![面板设置中开启订阅服务和 Clash/Mihomo 订阅](https://shadowrocket.ink/img/3x-ui-110096.jpg)

在 Clash / Mihomo 这个子选项卡里，还可以选择是否在生成的订阅里附带一套全局
路由规则，按自己的使用习惯决定开不开：

![Clash / Mihomo 订阅设置里的全局路由规则开关](https://shadowrocket.ink/img/3x-ui-110097.jpg)

### 4.3 新建 HY2 入站

回到面板左侧的"入站"，点"添加入站"，协议选 `hysteria`（也就是 Hysteria2），
备注随便取一个方便自己识别的名字，端口自己定一个：

![添加入站对话框，协议选择 hysteria，设置备注和端口](https://shadowrocket.ink/img/3x-ui-110098.jpg)

在传输设置里可以加一个 UDP Masks（流量混淆），类型选 Salamander（Hysteria2
自带的混淆方式），密码随机生成一个复杂的即可，拥塞控制算法选 BBR：

![UDP Masks 混淆设置，类型选 Salamander，拥塞控制选 BBR](https://shadowrocket.ink/img/3x-ui-110099.jpg)

这里最关键的是往下翻到的**端口跳跃**开关：打开"UDP Hop"，填一个端口范围
（比如 20000-50000）作为跳跃区间，再设置跳跃间隔（比如 5-10 秒），客户端和
服务端会在这个区间内按间隔动态切换实际通信端口：

![开启 UDP Hop 端口跳跃，设置跳跃端口范围和间隔](https://shadowrocket.ink/img/3x-ui-1100991.jpg)

安全选项卡里把"安全"设为 TLS，SNI 填你自己的域名，ALPN 选 `h3`（因为
Hysteria2 是基于 QUIC/HTTP3 的），其余保持默认就行：

![安全设置选项卡，TLS、SNI、ALPN 参数](https://shadowrocket.ink/img/3x-ui-1100992.jpg)

数字证书部分，直接引用前面安装脚本申请到的证书和私钥路径（公钥、私钥各填一行），
保存即可：

![数字证书设置，填入证书公钥、私钥文件路径](https://shadowrocket.ink/img/3x-ui-1100993.jpg)

### 4.4 添加客户端

入站建好之后，点进去添加一个客户端：邮箱/备注随便填一个能识别自己的标识，
按需要设置流量上限、IP 限制、到期时间，"关联入站"选刚才建好的这个 HY2 入站，
确认启用状态是打开的，点创建：

![添加客户端对话框，设置流量、IP 限制、关联入站等参数](https://shadowrocket.ink/img/3x-ui-1100994.jpg)

创建成功后，面板会给这个客户端生成一个专属的订阅二维码，扫码或者复制订阅链接
都能导入到客户端里（这里的订阅信息是每个人自己的凭证，不要发给别人，我这边
截图上也做了打码处理）：

![客户端创建成功后生成的专属订阅二维码](https://shadowrocket.ink/img/3x-ui-1100995.jpg)

## 五、把节点导入客户端使用

### 5.1 导入到 v2rayN 验证

先拿 Windows 上的 v2rayN 做个连通性测试，把刚才拿到的节点信息导入进去，
能看到一条协议为 Hysteria、开了 TLS 的记录：

![v2rayN 客户端列表里显示导入的 Hysteria 节点](https://shadowrocket.ink/img/3x-ui-1100996.jpg)

右键测一下延迟和速度，能出结果说明服务端配置基本没问题：

![在 v2rayN 里测试节点延迟和速度](https://shadowrocket.ink/img/3x-ui-1100997.jpg)

### 5.2 在 VPS 上开启端口跳跃的转发规则

光在面板里开了 UDP Hop 还不够，服务器的防火墙/NAT 层还需要一条规则，把跳跃
端口范围的流量转发到 Hysteria2 实际监听的端口上。用 `nft`（nftables）配置一下：

```bash
modprobe nf_tables
modprobe nf_conntrack
modprobe nf_nat
modprobe nft_chain_nat
modprobe nft_redir

nft delete table ip hy2jump 2>/dev/null || true

nft add table ip hy2jump
nft 'add chain ip hy2jump prerouting { type nat hook prerouting priority dstnat; policy accept; }'
nft add rule ip hy2jump prerouting udp dport 20000-50000 redirect to :21327

nft list table ip hy2jump
```

这里的端口范围要跟面板里 UDP Hop 设置的一致，`redirect to` 后面的端口则是
入站实际监听的端口（也就是新建入站时填的那个端口）。

### 5.3 用播放器的统计信息看看带宽表现

节点连通之后，可以拿一个对带宽比较敏感的场景做个简单验证，比如在 YouTube
里打开"统计信息"面板（Stats for nerds），看一下实际的连接速度和缓冲情况，
数值稳定说明节点在满带宽场景下表现正常：

![打开 YouTube 播放器的统计信息面板，查看连接速度](https://shadowrocket.ink/img/3x-ui-1100998.jpg)

客户端详情页里也能随时找回这个订阅二维码，方便换设备的时候重新扫码导入
（同样把订阅信息打了码，实际使用时这是你自己的私密链接）：

![客户端详情页可以随时重新查看订阅二维码](https://shadowrocket.ink/img/3x-ui-1100999.jpg)

面板里的客户端详情页，还会分别列出"SUB"和"CLASH"两种格式的订阅链接，
以及一条"复制全部配置"，按你用的客户端类型选对应格式复制就行：

![客户端详情页列出 SUB 和 CLASH 两种订阅链接格式](https://shadowrocket.ink/img/3x-ui-11009991.jpg)

### 5.4 用 Clash Verge 测试订阅是否可用

除了单条节点导入，也可以直接测一下"订阅"这条路径是否走得通。打开 Clash Verge，
在"订阅"页面粘贴刚才拿到的 Clash 格式订阅链接，点新建，订阅会拉取成功并显示
在列表里：

![在 Clash Verge 的订阅页面导入刚生成的订阅链接](https://shadowrocket.ink/img/3x-ui-11009992.jpg)

切到"代理"页面，能看到刚才建的 HY2 节点已经出现在代理组里，选中它：

![Clash Verge 代理组页面，选中刚导入的 HY2 节点](https://shadowrocket.ink/img/3x-ui-11009993.jpg)

最后在"网络设置"里打开"系统代理"开关，让系统流量走这条代理，切换生效后
就可以正常上网了：

![Clash Verge 网络设置页面，打开系统代理开关](https://shadowrocket.ink/img/3x-ui-11009994.jpg)

同样用 YouTube 的统计信息面板做个复核，确认切到 Clash Verge 之后带宽表现
依然正常：

![切换到 Clash Verge 代理后，再次查看 YouTube 播放统计信息](https://shadowrocket.ink/img/3x-ui-11009995.jpg)

到这里，从买 VPS、解析域名、装 3X-UI 面板，到建 Hysteria2 入站、开端口跳跃、
最后在 v2rayN 和 Clash Verge 里分别验证可用性，整套自建节点的流程就走完了。
后续如果要新增客户端或者调整参数，直接回到 3X-UI 面板里操作即可，不需要
重新走一遍安装流程。

### 5.5、机场推荐

* 以下机场按照流量付费，网站里有软件的使用和安装教程
* 购买流量以后，不限制时间，流量用完为止
* 如果网站无法访问，则说明已经被墙，更换其他网站即可

| 名 称 | 价 格 | 流 量 | 节点数 |
| :--- | :--- | :--- | :--- |
| [魔戒](https://1.jnk.ink/L4q20S) | 1元 | 1G | 30个 |
| [网际快车](https://1.jnk.ink/ad2RVl) | 7元 | 20G | 54个 |
| [牛逼](https://1.jnk.ink/LYet7x) | 14元 | 200G | 31个 |
| [飞鸟](https://1.jnk.ink/i7OhaC) | 10元 | 200G | 25个 |
| [皮卡丘](https://1.jnk.ink/d07dCA) | 15元 | 20G | 40个 |
| [happy猫](https://1.jnk.ink/5KiTxY) | 20元 | 200G | 27个 |
| [农夫山泉](https://1.jnk.ink/i1fXTMYk)  | 45元   | 200G |40个|
| [宝贝云](https://1.jnk.ink/xxPwfy) | 55元 | 600G | 64个 |
| [自由猫](https://1.jnk.ink/haO8Dr) | 89元 | 200G | 71个 |
| [飞兔](https://1.jnk.ink/bbXkiN) | 30元 | 100G | 80个 |

### 5.6、软件下载



| 设备 | 软件名称 | 软件名称 | 软件名称 |
| :--: | :------ | :------- | :------ |
| 安卓    | [v2rayNG](https://getfreevpn.info/zh/docs/vpn%E6%95%99%E7%A8%8B/%E4%B8%8B%E8%BD%BD%E5%92%8C%E4%BD%BF%E7%94%A8v2rayNG-VPN/) | [flclash](https://flclash.xyz/zh/)|  [v2box](https://v2box.pro/zh/)  |
| windows |  [v2rayN](https://getfreevpn.info/zh/docs/vpn%E6%95%99%E7%A8%8B/%E4%B8%8B%E8%BD%BD%E5%B9%B6%E4%BD%BF%E7%94%A8v2rayN%E8%BD%AF%E4%BB%B6/) | [flclash](https://flclash.xyz/zh/) | [hiddify next](https://hiddify.me/zh/) |
| iphone  | [shadowrocket](https://shadowrocket.ink/zh) | [v2box](https://v2box.pro/zh/)  |  [stash](https://apps.apple.com/us/app/stash-rule-based-proxy/id1596063349)  |
| mac    |  [v2box](https://v2box.pro/zh/) |  [clash verge](https://clashverge.net/) |  [hiddify next](https://hiddify.me/zh/)   |
| linux  |   [flclash](https://flclash.xyz/zh/)  |  [hiddify next](https://hiddify.me/zh/) |  [clash verge](https://clashverge.net/)  |

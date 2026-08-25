---
title: 3x-ui 搭建教程：手把手自建 Clash / v2ray 代理节点（VLESS + Reality）
description: 3x-ui 是基于 Xray-core 的开源网页代理面板，支持 VLESS、VMess、Trojan、Shadowsocks、Reality、XHTTP 等主流协议。本教程从购买 VPS 服务器、连接服务器、安装 3x-ui 面板，到配置 VLESS + Reality 节点、导入 Clash / v2ray 客户端并开启 BBR 加速，一步一步教你搭建属于自己的代理节点。
keywords:
  - 3x-ui
  - 3x-ui 教程
  - 3x-ui 搭建
  - Xray
  - Xray 面板
  - VLESS
  - Reality
  - XHTTP
  - 自建节点
  - 搭建代理节点
  - Clash 节点
  - v2ray 节点
  - VPN 搭建
  - BBR 加速
  - VPS 搭建教程
type: docs
---

用 3x-ui 自己动手搭建一个 Clash 或 v2ray 节点，其实并不难。本文从**购买 VPS 服务器**开始，到在代理客户端中**导入节点**为止，完整演示 3x-ui 面板的安装、VLESS + Reality 节点配置和 BBR 加速开启，适合想要自建代理节点、不想手写 Xray 配置文件的新手用户。

## 什么是 3x-ui 面板？

3x-ui 是一个开源的 **Xray 面板**，用网页界面来管理 Xray-core（一个功能很全的代理内核）。它是在早期 x-ui 项目基础上做的增强分支，目前是这一系列里维护比较活跃、功能比较完整的一个。

![3xui-1002.jpg](https://3x-ui.pro/img/3xui-1002.jpg)

核心特点大致是：

**多协议支持**——[VLESS](https://vless.app/zh)、VMess、Trojan、Shadowsocks、Wireguard 等主流协议都能配，也支持 XTLS/Reality 这类较新的抗封锁传输方式。

**Web 管理面板**——不用手写 Xray 的 JSON 配置，直接在浏览器里增删入站（inbound）、生成用户、配置传输层参数。

**多用户与流量管理**——可以按用户设置流量上限、到期时间、IP 并发限制，适合自己用或分发给多人。

**监控与运维**——面板里能看流量统计、系统负载，支持 Telegram 机器人推送告警，也内置了证书申请、订阅链接生成等功能。

**部署简单**——一条安装脚本或 Docker 就能跑起来，配合 Debian/Ubuntu 这类 VPS 很常见。

它把 Xray 原本偏底层、要手动改配置文件的使用方式，包装成了一个可视化后台，降低了搭建和管理代理节点的门槛。

以下从购买服务器到在代理软件中导入节点，一步一个脚印教你如何搭建属于自己的 VPN 节点！

## 一、购买 VPS 服务器

### 1、以 akile 为例，包含香港和美国服务器

- akile 的套餐如下

| 系列 / 节点 | CPU | 内存 | 硬盘 | 带宽 | 月流量 | 参考价 | 线路 / 特点 |
|---|---|---|---|---|---|---|---|
| [**HKLite-One**（香港轻量）](https://akile.ai/shop/server?type=traffic&areaId=3&nodeId=1&planId=811&aff_code=cbb004e0-a47c-440e-a767-2ede890c4e4f) | 1 核 | 1 GB | 10 GB SSD | 1 Gbps | 2500 GB | ¥24.99/月 | 香港 NTT/PCCW 国际，内置 Akile DNS 解锁，超售较重 |
| [**LAX-Lite**（洛杉矶国际）](https://akile.ai/shop/server?type=traffic&areaId=2&nodeId=37&planId=1009&aff_code=34c72b4e-7d9b-41cd-bee4-be72d273fecf) | 1 核 | 1 GB | 5 GB SSD | 5 Gbps 端口 | 1 TB | ¥8.88/月 | 纯海外国际线路，无大陆优化，超限速后转共享 10Mbps 不限量 |
| [**LAX4837-ISP**（洛杉矶双 ISP 住宅）](https://akile.ai/shop/server?type=traffic&areaId=2&nodeId=37&planId=1009&aff_code=cbb004e0-a47c-440e-a767-2ede890c4e4f) | 1 核 | 2 GB | 10 GB SSD | 1 Gbps | 1 TB | ¥49.99/月 | 原生静态 IP、双 ISP 家宽属性、三网回程 CUVIP(4837)、EPYC 宿主 |

- 这里我们购买 8.88 元/月的这个套餐
- 注意，系统选择 **debian11 或者 debian12**

![3xui-1001.jpg](https://3x-ui.pro/img/3xui-1001.jpg)

### 2、启动服务器

- 付款成功以后，等待 3 分钟，使服务器安装完毕
- 点击云服务器列表，打开刚才我们创建的服务器
- 如果服务器没有启动，按照箭头的提示，启动服务器

![3xui-1003.jpg](https://3x-ui.pro/img/3xui-1003.jpg)

## 二、连接服务器

### 1、下载连接服务器的软件：FinalShell

- [官网下载地址](https://www.hostbuf.com/t/988.html)
- [网盘下载地址](https://pan1.mene.lol/s/WvoFP)

软件界面如图

![3xui-1004.jpg](https://3x-ui.pro/img/3xui-1004.jpg)

### 2、使用软件，连接服务器

- 点击图中的文件夹

![3xui-1005.jpg](https://3x-ui.pro/img/3xui-1005.jpg)

- 选择左上角第一个图标，进行 linux 的服务器连接

![3xui-1006.jpg](https://3x-ui.pro/img/3xui-1006.jpg)

- 打开以后，如图，在表格 1234 中填写内容

![3xui-1007.jpg](https://3x-ui.pro/img/3xui-1007.jpg)

- 表格 1 填写服务器名称，这里可以随便写一个名字即可
- 表格 2 填写主机的 ip 地址，我们打开刚才我们购买的[服务器网址](https://akile.ai/console/server)，找到 ip 地址填写进去即可
- 打开网址以后，点击云服务器，如图复制服务器地址，粘贴到表格 2

![3xui-1008.jpg](https://3x-ui.pro/img/3xui-1008.jpg)

- 表格 3 填写服务器用户名，一般默认是填写：root
- 表格 4 我们将网页拉下来，找到访问控制，点击查看密码，将密码填入表格 4
- 最后点击确定，保存服务器信息

![3xui-1009.jpg](https://3x-ui.pro/img/3xui-1009.jpg)

- 保存以后，画面来到连接窗口，点击服务器，即可连接

![3xui-10091.jpg](https://3x-ui.pro/img/3xui-10091.jpg)

- 第一次连接，弹出安全警告，这里我们点击接受并保存
- 正常情况下就可以看到连接成功的界面

![3xui-10092.jpg](https://3x-ui.pro/img/3xui-10092.jpg)

- 这里如果没有显示连接成功，我们打开服务器页面看看这里服务器是否开机
- 如果没有开机，点击这里把服务器打开，等 3 分钟，再次连接即可

![3xui-1003.jpg](https://3x-ui.pro/img/3xui-1003.jpg)

- 到这里我们的服务器，就连接成功了
- 下面的步骤，我将教你如何进行 3x-ui 的搭建

![3xui-10093.jpg](https://3x-ui.pro/img/3xui-10093.jpg)

## 三、安装 3x-ui 面板

### 1、更新软件

- 复制下面这条命令

```
apt update
```

- 粘贴到 finalshell 软件中，如图

![3xui-10094.jpg](https://3x-ui.pro/img/3xui-10094.jpg)

- 然后，按键盘上的 enter 键
- 至此，一条命令执行完成了
- 想要安装 3x-ui 还要执行另外一条命令
- 复制并且执行下面的这条命令，安装 curl

```
apt install curl -y
```

### 2、安装 3x-ui

- 复制并且执行下面的命令
- 这里我们安装的版本是 3.4.2，因为目前这个版本最稳定，bug 最少

```
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh) v3.4.2
```

- 下图，下面的这个提示选择数据库，我们选择 1，然后按 enter 键

![3xui-10095.jpg](https://3x-ui.pro/img/3xui-10095.jpg)

- 下图，这一步我们直接回车，即可

![3xui-10096.jpg](https://3x-ui.pro/img/3xui-10096.jpg)

- 下图，到这一步，如果有域名，选择 1，按 enter 键
- 如果没有域名，选择 2，按 enter 键，对于个人用户，没有任何区别
- 这里我们选择 2，按 enter，如果后期买了域名以后，再添加也可以

![3xui-10097.jpg](https://3x-ui.pro/img/3xui-10097.jpg)

- 下图，询问是否使用 ipv6，我们直接按 enter

![3xui-10098.jpg](https://3x-ui.pro/img/3xui-10098.jpg)

- 下图，询问采用 80 端口，申请证书，直接按 enter

![3xui-10099.jpg](https://3x-ui.pro/img/3xui-10099.jpg)

- 下图，看到这些信息，表明申请证书成功
- 我们鼠标左键选择，右键复制并且保存这些信息
- 因为，这些是密码和登录网址的信息，一会要用到

![3xui-100991.jpg](https://3x-ui.pro/img/3xui-100991.jpg)

## 四、配置 3x-ui：添加 VLESS + Reality 节点

### 1、登录 3x-ui 网页后台面板

- 下图，复制我们刚才保存的登录网址，在浏览器打开

![3xui-100992.jpg](https://3x-ui.pro/img/3xui-100992.jpg)

- 在我们刚才保存的登录信息中，有用户名和密码
- 我们输入并且登录，进入 3x-ui 的后台

![3xui-100993.jpg](https://3x-ui.pro/img/3xui-100993.jpg)

### 2、添加和配置 VLESS 节点

- 点击左上角入站---添加入站，来到下图

![3xui-100994.jpg](https://3x-ui.pro/img/3xui-100994.jpg)

- 备注--这里我们随便填写名称，美国--vless--001
- 协议--我们选择 vless
- 其他选项不动，我们点击传输标签，下图

![3xui-100995.jpg](https://3x-ui.pro/img/3xui-100995.jpg)

- 传输--选择 xhttp
- 路径--自定义--我们输入：xhttp1
- 后面的保持默认，然后，点击安全标签，下图

![3xui-100996.jpg](https://3x-ui.pro/img/3xui-100996.jpg)

- 安全--选择 reality
- 目标--点击查找目标，点击第一个
- 点击创建，完成添加入站，如下图

![3xui-100997.jpg](https://3x-ui.pro/img/3xui-100997.jpg)

- 点击左上角，客户端--添加客户端
- 点击关联入站的输入框，选择刚才我们创建的入站，如下图
- 点击创建

![3xui-100998.jpg](https://3x-ui.pro/img/3xui-100998.jpg)

- 至此，我们已经完成创建 VLESS 节点，如下图

![3xui-100999.jpg](https://3x-ui.pro/img/3xui-100999.jpg)

- 点击图中的二维码，这里要选择 vless 节点信息，不要选择订阅信息
- 可以看到 vless 节点二维码，如下图

![3xui-1009991.jpg](https://3x-ui.pro/img/3xui-1009991.jpg)

- 苹果手机使用 [shadowrocket](https://shadowrocket.ink/zh) 或者 [karing](https://karing.biz/zh) 导入该节点
- 安卓手机使用 [v2rayng](https://v2rayng.4566.lol/zh) 或者 karing 导入该节点
- windows 电脑使用 [v2rayn](https://v2rayn.4566.lol/zh) 或者 hiddify 导入该节点
- mac 电脑使用 [karing](https://karing.biz/zh) 导入该节点

## 五、连接节点

### 1、安卓手机导入节点

- 以安卓手机为例，演示导入节点
- 如果你使用的是安卓手机，请你下载和安装下面的软件
- v2rayng 下载地址：https://pan1.mene.lol/s/JQjC0
- 打开软件，点击右上角的加号--扫描二维码
- 扫描上方的二维码即可导入节点

![3xui-1009992.jpg](https://3x-ui.pro/img/3xui-1009992.jpg)

- 点击右下角按钮，即可连接节点网络
- 点击测试，即可测试网络是否通畅

![3xui-1009993.jpg](https://3x-ui.pro/img/3xui-1009993.jpg)

### 2、开启 BBR 加速

- 在 finalshell 输入命令 x-ui
- 可以看到面板管理信息，如下图

![3xui-1009994.jpg](https://3x-ui.pro/img/3xui-1009994.jpg)

- 选择 26，按 enter 键，如下图
- 再选择 1，按 enter 键，开启 BBR 加速

![3xui-1009995.jpg](https://3x-ui.pro/img/3xui-1009995.jpg)

- 可以看到，下载的延时变成了 308 毫秒

![3xui-1009996.jpg](https://3x-ui.pro/img/3xui-1009996.jpg)

### 3、机场推荐

- 如果觉得一个 ip 地址不够用
- 可以购买便宜的机场节点
- 这里我推荐使用网际快车机场，[点击打开网际快车机场](https://1.jnk.ink/ad2RVl)
- 用自己的邮箱注册账号

![3xui-1009997.jpg](https://3x-ui.pro/img/3xui-1009997.jpg)

- 登录以后，点击商店--选择不限时套餐
- 搭配我们自己建立的节点使用，非常方便

![3xui-1009998.jpg](https://3x-ui.pro/img/3xui-1009998.jpg)

* 以下机场按照流量付费，网站里有软件的使用和安装教程
* 购买流量以后，不限制时间，流量用完为止
* 如果网站无法访问，则说明已经被墙，更换其他网站即可

| 名 称 | 价 格 | 流 量 | 节点数 |
| :--- | :--- | :--- | :--- |
| [魔戒](https://1.jnk.ink/L4q20S) | 1 元 | 1G | 30 个 |
| [网际快车](https://1.jnk.ink/ad2RVl) | 7 元 | 20G | 54 个 |
| [牛逼](https://1.jnk.ink/LYet7x) | 14 元 | 200G | 31 个 |
| [飞鸟](https://1.jnk.ink/i7OhaC) | 10 元 | 200G | 25 个 |
| [皮卡丘](https://1.jnk.ink/d07dCA) | 15 元 | 20G | 40 个 |
| [happy猫](https://1.jnk.ink/5KiTxY) | 20 元 | 200G | 27 个 |
| [农夫山泉](https://1.jnk.ink/i1fXTMYk) | 45 元 | 200G | 40 个 |
| [宝贝云](https://1.jnk.ink/xxPwfy) | 55 元 | 600G | 64 个 |
| [自由猫](https://1.jnk.ink/haO8Dr) | 89 元 | 200G | 71 个 |
| [飞兔](https://1.jnk.ink/bbXkiN) | 30 元 | 100G | 80 个 |

## 常见问题（FAQ）

**3x-ui 支持哪些协议？** 3x-ui 基于 Xray-core，支持 VLESS、VMess、Trojan、Shadowsocks、Wireguard，以及 XTLS/Reality、XHTTP 等抗封锁传输方式。

**搭建 3x-ui 需要域名吗？** 不需要。对于个人用户，使用 VLESS + Reality 无需域名即可获得较好的抗封锁效果；后期如果购买了域名，也可以随时在面板里补充配置。

**为什么建议安装 3.4.2 版本？** 本教程选用 3.4.2 是因为该版本目前较为稳定、已知 bug 较少，适合新手部署，避免踩坑。

**BBR 加速有什么用？** 开启 BBR 后可以优化 TCP 拥塞控制，通常能明显降低延迟、提升代理节点的下载和上传速度。

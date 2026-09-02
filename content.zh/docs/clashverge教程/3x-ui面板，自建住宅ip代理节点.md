---
title: "用 3X-UI 自建 Hysteria2 节点：从买 VPS 到端口跳跃的完整流程"
description: "手把手记录用 3X-UI 面板在自己的 VPS 上搭建 Hysteria2（HY2）节点、开启端口跳跃，并在 Clash 系客户端里导入使用的整个过程。"
keywords: ["3X-UI", "Hysteria2", "HY2", "自建节点", "端口跳跃", "VPS"]
weight: 8
---

**[3x-ui](https://3x-ui.pro/zh)面板搭建完成以后，会看到面板上有很多的协议，比如[vless](https://vless.app/zh),trojan,hy2,socks5等。也会接触很多的ip地址，比如机房ip，原生ip，住宅ip，高风险ip等。如果你还不会安装3x-ui，[请看这篇文章](https://3x-ui.pro/zh/)。
大陆用户做TikTok运营（尤其是养号、矩阵、直播、TikTok Shop）时，强烈建议使用住宅IP，而不是普通机房/数据中心IP。**

### 什么是住宅IP？
住宅IP（Residential IP，也叫家宽IP或ISP IP）是由当地互联网服务提供商（如美国的AT&T、Verizon、Comcast等）分配给真实家庭宽带用户的IP地址。

- **特点**：看起来就像普通家庭用户在家上网，ASN归属运营商，地理位置和路由信息更贴近真实用户。
- **对比机房IP（Data Center IP）**：机房IP来自云服务器/数据中心（AWS、阿里云、Vultr等），速度快、便宜，但平台很容易识别为“非真人/批量操作”，容易触发限流、验证甚至封号。

**为什么TikTok运营优先用住宅IP？**  
TikTok风控会综合判断IP类型、历史信誉、地理位置一致性、设备指纹和行为轨迹。机房IP常被标记为可疑，导致0播放、限流或关联封号；住宅IP信任度更高，更适合注册、养号、发布、直播和开店。静态住宅IP（固定不变、独享）最适合主账号和长期运营；动态住宅IP适合批量注册或测试。

实际中，优质静态住宅IP价格较高（单个每月几十到几百人民币不等），但比账号损失划算。注意市面上有“伪住宅/双ISP”资源，纯净度参差不齐，需用工具（如ping0.cc）检测是否为真正的ISP属性、风险分是否低。

### 什么是链式代理？
链式代理（Proxy Chaining / 多级代理 / 代理链）是指让网络请求**依次经过多个代理节点**，而不是只经过一个。

简单路径示例：  
你的设备 → 代理A（如国内或中转节点） → 代理B（中继） → 代理C（目标国家的住宅IP） → TikTok服务器

每个节点只知道前后相邻的节点，目标网站最终只看到最后一跳的IP。真实IP被多层隔离。

### 为什么要学会链式代理？
在TikTok/跨境运营中，单一代理有时不够稳或匿名性不足，链式代理的主要价值包括：

1. **更高匿名性和防关联**：多层隔离让平台更难追溯真实来源或关联多个账号，适合矩阵运营。
2. **绕过复杂限制**：从大陆直连海外有时不稳定或被干扰，可通过中转节点（例如先走优质专线/机场，再出口到目标国家的住宅IP）提升连通性和稳定性。
3. **冗余与容错**：某一层出问题可切换，降低单点故障风险。
4. **更灵活的伪装**：组合不同类型IP（如机房中转 + 住宅出口），出口必须是干净的住宅/ISP IP，才能真正降低风控。

### 一、购买住宅ip地址

- 这里我们来购买rarecloud的美国住宅ip，一个月5美金左右
- [点击打开rarecloud官方网站](https://rarecloud.io/clients/aff_redirect.php?aff=738&to=https%3A%2F%2Frarecloud.io%2Fresidential-proxy%2F&sig=d9f005ec886586a2c137ff75bb8e9bd9f72cb62ae1dc9c334f8f1ec03ae17f79)
- 选择静态住宅ip地址代理，选择按月付费


![3xui-zhuzhai-1008.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1008.jpg)


- 购买成功以后，给客服留言，获取ip地址和账号密码


### 二、将socks5住宅ip代理导入3x-ui面板

- 登录3x-ui面板，然后点击xray配置，点击出站


![3xui-zhuzhai-1001.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1001.jpg)

- 协议选择socks,标签随便写，这里写racknerd
- 地址填写，刚才我们购买的，socks代理的ip地址
- 端口随便写，用户名和密码填写刚才买的，最后点击创建

![3xui-zhuzhai-1002.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1002.jpg)



### 三、设置链式代理

- 下面我们设置链式代理
- 点击路由规则，然后点击入站标签，全部选上
- 出站标签，我们选择之前创建的racknerk


![3xui-zhuzhai-1003.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1003.jpg)

![3xui-zhuzhai-1004.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1004.jpg)

![3xui-zhuzhai-1005.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1005.jpg)

### 四、测试住宅ip代理

- 由于我们在如站标签，全部选上
- 也就是说所有的节点，都会走socks代理

- 下面我们随便选择一个节点进行测试
- 首先要重启xray
- 然后，在[v2rayn](https://vrayn.4566.lol/zh)中，右键选择节点，点击测试真链接
- 如果有延时，则说明我们的链式代理设置成功


![3xui-zhuzhai-1006.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1006.jpg)

![3xui-zhuzhai-1007.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1007.jpg)

![3xui-zhuzhai-1009.jpg](https://3x-ui.pro/img/3xui-zhuzhai-1009.jpg)
---
title: "Self-Hosting a Hysteria2 Node with 3X-UI: From Buying a VPS to Port Hopping"
description: "A step-by-step record of setting up a Hysteria2 (HY2) node on your own VPS with the 3X-UI panel, enabling port hopping, and importing it into Clash-based clients."
keywords: ["3X-UI", "Hysteria2", "HY2", "self-hosted node", "port hopping", "VPS"]
weight: 8
---

If you're already comfortable with clients like Shadowrocket, sooner or later the idea
of "running your own node" comes up — either because you don't want to depend on
someone else's service, or because you want to tune every parameter yourself. This is
a record of the full process of building a Hysteria2 (HY2) node on your own VPS using
the open-source panel [3X-UI](https://3x-ui.pro/en), and enabling port hopping — from buying the machine all
the way to a working client connection.

## 1. Why run your own node

Compared with using a ready-made service someone else provides, self-hosting has a
few clear advantages:

- **You control every parameter**: protocol, port, obfuscation method, certificate,
  traffic limits — all configured to your own needs instead of following someone
  else's rules.
- **No worrying about being throttled or cut off**: the machine and the panel are
  both yours, so stability depends only on the VPS itself and your own maintenance —
  there's no "provider decides to stop the service" scenario.
- **Easy to scale horizontally as needed**: one panel can manage multiple inbounds
  and multiple clients at the same time, making it easy to split nodes by purpose
  and to troubleshoot on your own.

Of course self-hosting has a cost too — you need to buy the machine yourself, maintain
the panel and certificate renewal yourself, and troubleshoot any issues yourself.
Whether it's worth it depends on how much effort you're willing to put in.

## 2. Advantages of the Hysteria2 protocol

Hysteria2 (HY2) has become quite popular in recent years. A few of its key traits:

- **Built on QUIC, naturally resistant to packet loss**: it runs over UDP + QUIC, so
  on unstable connections with high packet loss it's usually more stable than
  traditional TCP-based protocols.
- **High speed ceiling**: the protocol is optimized for high-bandwidth scenarios, so
  it performs well when you're maxing out bandwidth or transferring large files.
- **Supports port hopping**: the client and server can dynamically switch the actual
  port they use within a defined port range, which helps reduce the odds of being
  specifically throttled or blocked. This is the part we'll focus on configuring
  later in this guide.

## 3. Preparation: VPS and domain

### 3.1 Buy a VPS

The first step of self-hosting a node is having an overseas VPS. The following are a
few plans I've actually used myself and found reasonably priced for the specs. The
links below are my referral links (ordering through them earns me a small commission;
the links go through a redirect and ultimately land on the official purchase page of
the VPS provider akile.ai):

| Plan | CPU | RAM | Disk | Bandwidth | Monthly Traffic | Reference Price | Line / Features |
|---|---|---|---|---|---|---|---|
| [**HKLite-One** (Hong Kong, light)](https://1.jnk.ink/ks2wX) | 1 core | 1 GB | 10 GB SSD | 1 Gbps | 2500 GB | ¥24.99/mo | Hong Kong NTT/PCCW international route, built-in DNS unlock, fairly oversold |
| [**LAX-Lite** (Los Angeles, international)](https://1.jnk.ink/fnSih) | 1 core | 1 GB | 5 GB SSD | 5 Gbps port | 1 TB | ¥8.88/mo | Pure international route, no mainland China optimization, drops to shared unmetered 10Mbps after the cap |
| [**LAX4837-ISP** (Los Angeles, dual-ISP residential)](https://1.jnk.ink/SEwxN) | 1 core | 2 GB | 10 GB SSD | 1 Gbps | 1 TB | ¥49.99/mo | Native static IP, dual-ISP residential characteristics, better return routing, EPYC host |

If you're new to this, start with a cheaper plan to get the whole process working
first, then decide whether to upgrade to a pricier line later.

### 3.2 Buy a domain

You'll need a domain to get a certificate for the node. I bought mine on
[Porkbun](https://porkbun.com/) — the interface is simple and renewal isn't
expensive, but feel free to use whichever registrar you're used to.

### 3.3 Point the domain at your VPS's IP with Cloudflare

After buying the domain, go to Cloudflare and add an A record pointing the domain to
your VPS's IPv4 address. Set the type to `A`, fill in the subdomain prefix as the name
(like `toto` in the screenshot below), and leave the proxy status turned off (DNS
only, not proxied through Cloudflare) — otherwise it'll get in the way when you
request a Let's Encrypt certificate later.

![Add an A record in Cloudflare pointing the domain to the VPS's IP](https://shadowrocket.ink/img/3x-ui.jpg)

### 3.4 Ping it to confirm the IP is reachable

Once the DNS record has propagated, `ping` the VPS's IP locally first to confirm the
latency and packet loss are within a normal range before moving on.

![Ping the VPS's IP from the command line to confirm connectivity](https://shadowrocket.ink/img/3x-ui-11002.jpg)

### 3.5 Connect to the VPS with a remote tool

Next, use FinalShell (or Xshell, iTerm, or whatever tool you prefer) to set up a new
SSH connection: host is the VPS's IP, port defaults to 22, username `root`, and the
password is the initial one from the purchase page. Once connected you can start
setting up the environment.

![Set up a new SSH connection in FinalShell to connect to the VPS](https://shadowrocket.ink/img/3x-ui-11003.jpg)

## 4. Install 3X-UI

3X-UI is an open-source web control panel for managing an Xray-core service. It has a
friendly, multi-language interface and can manage multiple protocols and inbounds
from a single panel — much more convenient than pure command-line operation.

![3X-UI project introduction page](https://shadowrocket.ink/img/3x-ui-11004.jpg)

### 4.1 Install command

After SSHing into the VPS, update the system, install the dependencies, then run the
official install script:

```bash
apt update -y
apt install -y curl socat
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

The install script will ask you a few questions in turn:

- **Database type**: if you won't have many clients, the default SQLite is enough.

![The install script asks for the database type, default SQLite](https://shadowrocket.ink/img/3x-ui-11005.jpg)

- **Whether to customize the panel port**: leaving it blank generates a random port;
  for security, it's better to set one that's harder to guess.

![After choosing the database, the script asks whether to customize the panel port](https://shadowrocket.ink/img/3x-ui-11006.jpg)

- **SSL certificate setup**: there are several options here. Since we already have the
  domain resolved, choosing "request a Let's Encrypt certificate for a domain" is the
  most convenient — the certificate renews automatically.

![The SSL certificate setup menu with several request methods](https://shadowrocket.ink/img/3x-ui-11007.jpg)

After choosing that option, enter the domain you resolved earlier and the script will
request the certificate automatically:

![After choosing the domain certificate option, enter your own domain](https://shadowrocket.ink/img/3x-ui-11008.jpg)

Once the certificate is issued, the script prints out the certificate and private key
paths and asks whether you want to modify `--reloadcmd` (the command that restarts
the panel automatically after certificate renewal) — the default is fine in most
cases:

![Certificate issued successfully; the script prints out the certificate path](https://shadowrocket.ink/img/3x-ui-11009.jpg)

After that, the script also sets up fail2ban (brute-force protection) and shows a
management menu with common operations like start, stop, restart, and viewing current
settings:

![The management menu shown after installation finishes, with start/stop/restart options](https://shadowrocket.ink/img/3x-ui-110091.jpg)

Scrolling further down the menu you'll find certificate management, firewall
management, enabling BBR, speed testing, and more, along with the running status of
the panel and Xray at the bottom:

![The rest of the management menu, including certificate management, firewall, BBR, and speed test](https://shadowrocket.ink/img/3x-ui-110092.jpg)

Choosing "view current settings" shows the panel port, database path, and access URL
— jot these down, you'll need them to log into the panel next:

![Viewing current panel settings, including port, database path, and access URL](https://shadowrocket.ink/img/3x-ui-110093.jpg)

### 4.2 Log in and enable the subscription feature

Open the panel using the access URL from the previous step, and log in with the
username and password you set during installation:

![3X-UI login page](https://shadowrocket.ink/img/3x-ui-110094.jpg)

Once logged in, the home page shows CPU and memory usage as well as the running
status of Xray and the panel itself — confirm everything looks normal first:

![The system status dashboard after login, showing CPU, memory, and Xray status](https://shadowrocket.ink/img/3x-ui-110095.jpg)

Next, go to "Panel Settings → General" and turn on both "Enable Subscription Service"
and "Clash / Mihomo Subscription." Set the listening port to whatever you like (2096
in the screenshot below) — this lets clients later pull all node configs through a
single subscription link instead of filling in every parameter by hand:

![Enabling the subscription service and Clash/Mihomo subscription in panel settings](https://shadowrocket.ink/img/3x-ui-110096.jpg)

In the Clash / Mihomo sub-tab, you can also choose whether to bundle a set of global
routing rules into the generated subscription, depending on your own usage habits:

![The global routing rule toggle under Clash/Mihomo subscription settings](https://shadowrocket.ink/img/3x-ui-110097.jpg)

### 4.3 Create the HY2 inbound

Go back to "Inbounds" in the panel's sidebar and click "Add Inbound." Choose
`hysteria` (i.e. Hysteria2) as the protocol, pick any remark you'll recognize, and set
a port of your choice:

![The Add Inbound dialog, protocol set to hysteria, with remark and port](https://shadowrocket.ink/img/3x-ui-110098.jpg)

In the transport settings you can add UDP Masks (traffic obfuscation) — set the type
to Salamander (Hysteria2's built-in obfuscation), generate a random, complex password,
and set the congestion control algorithm to BBR:

![UDP Masks obfuscation settings, type set to Salamander, congestion control set to BBR](https://shadowrocket.ink/img/3x-ui-110099.jpg)

The key part here is the **port hopping** toggle further down: turn on "UDP Hop," fill
in a port range (e.g. 20000-50000) as the hop range, and set a hop interval (e.g.
5-10 seconds). The client and server will dynamically switch the actual port they use
within this range at that interval:

![Enabling UDP Hop port hopping, with hop port range and interval](https://shadowrocket.ink/img/3x-ui-1100991.jpg)

In the security tab, set "Security" to TLS, fill in your own domain as the SNI, and
choose `h3` for ALPN (since Hysteria2 is built on QUIC/HTTP3) — leave everything else
at the default:

![The security settings tab, with TLS, SNI, and ALPN parameters](https://shadowrocket.ink/img/3x-ui-1100992.jpg)

For the certificate section, just reference the certificate and private key paths
issued by the install script earlier (one line each for the public key and private
key), then save:

![Digital certificate settings, entering the certificate and private key file paths](https://shadowrocket.ink/img/3x-ui-1100993.jpg)

### 4.4 Add a client

Once the inbound is created, click into it to add a client: enter any email/remark you
can identify yourself with, set a traffic limit, IP limit, and expiration date as
needed, select the HY2 inbound you just created for "Associated Inbounds," make sure
it's enabled, and click Create:

![The Add Client dialog, setting traffic, IP limit, and associated inbound](https://shadowrocket.ink/img/3x-ui-1100994.jpg)

Once created, the panel generates a dedicated subscription QR code for this client —
scan it or copy the subscription link to import it into a client (this subscription
info is your own personal credential, don't share it with anyone; I've also blurred
it out in the screenshots):

![The dedicated subscription QR code generated after the client is created](https://shadowrocket.ink/img/3x-ui-1100995.jpg)

## 5. Import the node into a client

### 5.1 Import into v2rayN for a connectivity check

First, do a connectivity test with v2rayN on Windows. Import the node info you just
got, and you'll see an entry with the Hysteria protocol and TLS enabled:

![v2rayN's client list showing the imported Hysteria node](https://shadowrocket.ink/img/3x-ui-1100996.jpg)

Right-click to test latency and speed — getting a result back means the server-side
config is basically working:

![Testing the node's latency and speed in v2rayN](https://shadowrocket.ink/img/3x-ui-1100997.jpg)

### 5.2 Enable the port-hopping forwarding rule on the VPS

Turning on UDP Hop in the panel alone isn't enough — the server's firewall/NAT layer
also needs a rule to forward traffic in the hop port range to the port Hysteria2 is
actually listening on. Set this up with `nft` (nftables):

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

The port range here needs to match what you set for UDP Hop in the panel, and the
port after `redirect to` is the port the inbound is actually listening on (the one
you filled in when creating the inbound).

### 5.3 Check bandwidth with a player's stats overlay

Once the node is connected, you can do a quick sanity check with a bandwidth-sensitive
scenario — for example, open the "Stats for nerds" panel in YouTube and check the
actual connection speed and buffering. Stable numbers mean the node performs normally
under full-bandwidth load:

![Opening YouTube's stats-for-nerds panel to check connection speed](https://shadowrocket.ink/img/3x-ui-1100998.jpg)

You can also always find this subscription QR code again from the client detail page,
which is handy for re-scanning on a new device (also blurred here — in real use this
is your own private link):

![The client detail page lets you view the subscription QR code again at any time](https://shadowrocket.ink/img/3x-ui-1100999.jpg)

The client detail page in the panel also lists "SUB" and "CLASH" format subscription
links separately, plus a "copy all configs" option — pick the format that matches
whichever client you're using:

![The client detail page listing SUB and CLASH format subscription links](https://shadowrocket.ink/img/3x-ui-11009991.jpg)

### 5.4 Test the subscription in Clash Verge

Besides importing a single node, you can also directly test whether the "subscription"
path works. Open Clash Verge, paste the Clash-format subscription link you just got
into the "Profiles" page, click new, and the subscription should be fetched
successfully and show up in the list:

![Importing the newly generated subscription link on Clash Verge's Profiles page](https://shadowrocket.ink/img/3x-ui-11009992.jpg)

Switch to the "Proxies" page, and you'll see the HY2 node you just created show up in
the proxy group — select it:

![Clash Verge's proxy group page, selecting the newly imported HY2 node](https://shadowrocket.ink/img/3x-ui-11009993.jpg)

Finally, turn on the "System Proxy" toggle in "Network Settings" so system traffic
routes through this proxy — once it takes effect, you should be able to browse
normally:

![Clash Verge's network settings page, turning on the system proxy toggle](https://shadowrocket.ink/img/3x-ui-11009994.jpg)

Do another check with YouTube's stats overlay to confirm bandwidth still looks normal
after switching to Clash Verge:

![Checking YouTube's playback stats again after switching to the Clash Verge proxy](https://shadowrocket.ink/img/3x-ui-11009995.jpg)

That covers the whole flow — from buying the VPS, resolving the domain, and installing
the 3X-UI panel, to creating the Hysteria2 inbound, enabling port hopping, and finally
verifying it works in both v2rayN and Clash Verge. If you need to add more clients or
tweak parameters later, just go back into the 3X-UI panel — there's no need to redo
the installation process.

### 5.5. Recommended VPN Services

* The VPN services below charge based on traffic usage. Their websites also provide tutorials for installing and using the corresponding software.
* After purchasing traffic, there is no time limit. The traffic remains valid until it is used up.
* If a website cannot be accessed, it may have been blocked. Simply switch to another website.

| Name | Price | Traffic | Nodes |
| :--- | :--- | :--- | :--- |
| [MoJie](https://1.jnk.ink/L4q20S) | ¥1 | 1 GB | 30 |
| [Wangji Kuaiche](https://1.jnk.ink/ad2RVl) | ¥7 | 20 GB | 54 |
| [NiuBi](https://1.jnk.ink/LYet7x) | ¥14 | 200 GB | 31 |
| [Feiniao](https://1.jnk.ink/i7OhaC) | ¥10 | 200 GB | 25 |
| [Pikachu](https://1.jnk.ink/d07dCA) | ¥15 | 20 GB | 40 |
| [Happy Cat](https://1.jnk.ink/5KiTxY) | ¥20 | 200 GB | 27 |
| [Nongfu Spring](https://1.jnk.ink/i1fXTMYk) | ¥45 | 200 GB | 40 |
| [Baobei Cloud](https://1.jnk.ink/xxPwfy) | ¥55 | 600 GB | 64 |
| [Free Cat](https://1.jnk.ink/haO8Dr) | ¥89 | 200 GB | 71 |
| [Feitu](https://1.jnk.ink/bbXkiN) | ¥30 | 100 GB | 80 |

### 5.6. Software Downloads

| Device | Software | Software | Software |
| :--: | :------ | :------- | :------ |
| Android | [v2rayNG](https://getfreevpn.info/zh/docs/vpn%E6%95%99%E7%A8%8B/%E4%B8%8B%E8%BD%BD%E5%92%8C%E4%BD%BF%E7%94%A8v2rayNG-VPN/) | [FlClash](https://flclash.xyz/zh/) | [V2Box](https://v2box.pro/zh/) |
| Windows | [v2rayN](https://getfreevpn.info/zh/docs/vpn%E6%95%99%E7%A8%8B/%E4%B8%8B%E8%BD%BD%E5%B9%B6%E4%BD%BF%E7%94%A8v2rayN%E8%BD%AF%E4%BB%B6/) | [FlClash](https://flclash.xyz/zh/) | [Hiddify Next](https://hiddify.me/zh/) |
| iPhone | [Shadowrocket](https://shadowrocket.ink/zh) | [V2Box](https://v2box.pro/zh/) | [Stash](https://apps.apple.com/us/app/stash-rule-based-proxy/id1596063349) |
| Mac | [V2Box](https://v2box.pro/zh/) | [Clash Verge](https://clashverge.net/) | [Hiddify Next](https://hiddify.me/zh/) |
| Linux | [FlClash](https://flclash.xyz/zh/) | [Hiddify Next](https://hiddify.me/zh/) | [Clash Verge](https://clashverge.net/) |

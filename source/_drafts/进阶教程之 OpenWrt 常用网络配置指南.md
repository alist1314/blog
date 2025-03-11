---
abbrlink: ''
categories:
- - 技术
date: 2024-4-2
tags:
- OpenWrt
title: 进阶教程之 OpenWrt 常用网络配置指南
updated: '2025-03-11T15:39:21.282+08:00'
---
当你学会了如何使用 OpenWrt 实现最基础的上网功能，那么是时候更进一步了，接下来学习使用 OpenWrt 实现常用的其它网络配置，比如强化无线网络安全等，以便更好的应对日常使用场景。

## 修改路由器 LAN 口地址（改变网段）

打开“网络”-“接口”，进入 LAN 口配置界面，在“常规设置”选项卡，将“IPv4 地址” 改为需要的IP即可。

LAN 口地址是路由器自身作为一个上网设备所使用的 IP 地址，可以按需设为任意局域网 IP，并不是只能设为 .1

**注意：网页界面有防错机制，每当应用了新的修改后，有 90 秒倒计时，当你修改了 LAN 口 IP 地址后，需要使用修改后的 IP 再次成功访问管理界面，否则超时后会被自动回滚修改。**

例如：你将 LAN 口地址修改为 192.168.2.1，在点击了“保存并应用”按钮后，应当重新与路由器连接，获得新的局域网 IP 后，再访问新网关地址 192.168.2.1 打开管理界面，以完成本次修改，如果你不这样做，则 90 秒后会被自动回滚修改，即 LAN 地址将重新变回修改前的 192.168.9.1

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806710.png)

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806711.png)

界面有防错机制，为了防止错误的操作导致无法访问管理界面。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806713.png)

如果无法成功访问管理界面，则超时后会自动回滚修改。

注意：路由器 IP 地址不可盲目更改，需要遵循网络地址规范。

### 正确的局域网 IP 地址范围

10.x.x.x
172.16.x.x 至 172.31.x.x
192.168.x.x
x 表示可按需修改，其它数字则不可变更。如果不遵循标准网络地址规范，将导致网络异常。

**详情参阅：**[RFC 1918](https://datatracker.ietf.org/doc/html/rfc4193)

## 配置静态 IP上网

在一些上网环境，可能需要手动配置静态 IP 才能上网，那么按需修改 WAN 口的配置即可。
先将 “协议” 选项改为 “静态地址”，然后再按需配置即可。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806714.png)

默认使用网关 IP 作为 DNS 服务器，如果网关不提供 DNS 服务，或者你想使用其它 DNS 服务器，则前往 “高级设置” 选项卡，按需添加自定义 DNS 服务器。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806715.png)

## 彻底禁用路由器上的 IPv6 网络

OpenWrt 系统默认已启用 IPv6 网络，如果你需要彻底禁用 IPv6 网络，则参照如下操作即可。

### 禁用 WAN 接口的 IPv6 网络

- 如果是二级路由（网关），则需要“停止” WAN6 接口，并取消它的“开机自动运行”，或者删除 WAN6 接口。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806716.png)

- 如果是自动拨号的一级路由，则需要修改 WAN 接口，在“高级设置”选项卡，将“获取 IPv6 地址”设为“已禁用”。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806717.png)

### 禁用 LAN 口的 IPv6 服务

如果你需要禁用 LAN 口的 IPv6 服务，则需要前往“网络”-“接口”-“设备”选项卡，按需关闭对应接口的 IPv6 功能即可，默认局域网设备为 “br-lan”，则参照下图设置。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806718.png)

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806719.png)

至此路由器已彻底禁用所有 IPv6 服务，位于 “br-lan” 接口下的上网设备，重新连接路由器后将不再获得任何 IPv6 地址。

**注意：如果将安装了 OpenWrt 系统的无线路由器作为无线 AP 使用，也必须关闭 LAN 口的 IPv6 服务，否则会导致一些网络设备无法正常联网，例如各类新款安卓系统的 POS 机，因为获得了多个局域网临时 IPv6 地址，而系统会自动优先使用 IPv6 网络的特性，最终会导致经常联网失败。**

## 关闭 OpenWrt 的 DHCP 服务器

如果你要使用局域网内的其它 DHCP 服务器，则必须关闭多余的 DHCP 服务器，因为通常局域网禁止同时存在多个 DHCP 服务器。
打开“网络”-“DHCP/DNS”，在“常规设置”选项卡，取消勾选“启用”即可。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806720.png)

**注意：如果要作为旁路网关（旁路由）使用时，则不可使用此方式停用 Dnsmasq 服务器。**
详情参阅：[https://iyzm.net/openwrt/545.html](https://iyzm.net/openwrt/545.html) 其中关于“旁路网关模式（旁路由）”的段落。

你想要进一步提高无线网络安全？那么你需要了解常用的基础强化功能。

隐藏无线网络名称，使得上网设备无法看到无线网络名称，连接时需要手动输入无线网络名称。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806721.png)

2.4G无线，选择 “强制 CCMP (AES)” 算法。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806722.png)

开启无线网络黑名单/白名单，以限制允许连接到此 AP 的无线设备。

“仅允许列表内”为白名单，只有加入此列表的 MAC 地址才能连接，
“仅允许列表外”为黑名单，加入此列表的 MAC 地址将无法连接。

**注意：现在大多数新式操作系统均默认使用随机 MAC 地址连接无线网络，所以 MAC 黑名单一般情况下已无效。**

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806723.png)

## 提高无线网络性能

无线网络因其物理属性，所有无线路由器均使用同样的无线频率，极易产生干扰，导致无线速度下降，延迟变高，在人员密集区尤其明显，为了改善无线网络性能，你需要更换合适的无线信道，尽力降低干扰所造成的负面影响。

可以使用无线路由器自带的”状态“-”信道分析“功能，来分析判断周围的无线状况，根据实际情况调整无线参数。

无线网络根据频率分为两种，一种是2.4G，一种是5G；
2.4G 干扰最强速度最慢（但穿墙能力强）。
5G 传输带宽高，延迟低（但穿墙能力弱）。

模式：无线传输标准，一般不用改，类似于 USB2.0、USB3.0 的意思。
信道：相当于公路上的多条车道，选择人最少的那条即可，能获得更快的无线速度。
频宽：无线传输所使用的频率宽度，频宽越高，传输速度就越高。
160Mhz 频宽：高端无线路由器上才支持此频宽，如果你要启用 160Mhz 频宽，请确保无线路由器与上网设备间没有任何阻挡物，且距离应小于 10米，否则传输速度将大打折扣。注意：160Mhz 频宽需要无线路由器和上网客户端同时支持才能发挥效果。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806724.png)

按需调整“工作频率”参数，以改善无线网络性能。

## 添加无线网络（简化版访客网络）

例如有时家里来客人了，但是不想告诉别人密码，那么可以在无线路由器上添加一个新的无线接入点，专门给客人使用。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806725.png)

按需选择对应的无线网卡，然后点击“添加”按钮开始添加无线网络。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806726.png)

默认是无密码状态，如果要添加密码，请按需修改即可。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806727.png)

按需调整，阻止连接到此无线网络的上网设备互相通信。

![](https://gcore.jsdelivr.net/gh/skyboy520/picture/picture/202403290806728.png)

至此添加完毕，如果不用了可以禁用或移除。



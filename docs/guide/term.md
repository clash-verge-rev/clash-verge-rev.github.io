## 系统代理 / Tun 模式

> 代理模式决定本机网络请求程序发出的流量如何抵达监听在本地的代理程序，即入站。

| 代理模式 | 说明                                                                                                                                                                                                                                                                                                                                                         | 特点                                                                                                                          |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| 系统代理 | 1. 代理程序会在系统**“约定”**的特定位置（如注册表、系统变量等）设置好代理程序监听请求的端口信息，进行网络请求的应用会**自发性**地尝试读取这部分信息，并将请求发送至代理程序。不同操作系统的**“约定”**方式各异。<br />2. 系统代理更像是一种行业内的**“约定”**，并非所有程序都遵守这种非强制性的**“约定”**，最终采取哪种方式发生请求往往取决于开发人员的意愿。 | 1. 具有自发性，网络请求程序尝试使用”约定“配置或使用网络请求程序里额外指定的配置。<br />2. 不能代理`UDP`流量（如游戏数据包）。 |
| Tun 模式 | 代理程序会创建一张虚拟网卡，通过配置操作系统的路由将网络请求**重定向**到这张虚拟网卡，代理程序从虚拟网卡中读取并处理这些网络请求。<br />与系统代理不同的是，该步骤发生在网络请求程序发出请求之后，因此这种方法不依赖开发人员的意愿。                                                                                                                         | 1. 拦截和处理所有流量(`TCP`/`UDP`)重定向到本地的代理程序。<br />2.网络请求程序无需额外配置。                                  |

## 规则/ 全局 / 直连模式

> 路由模式决定监听在本地的代理程序的流量如何出去，即出站。

| 路由模式 | 说明                                                                              |
| -------- | --------------------------------------------------------------------------------- |
| 规则模式 | 根据配置文件中的规则(集)进行**条件匹配**，决定流量是从哪个代理节点/本地网络出去。 |
| 全局模式 | 所有流量均从**手工选定**的代理节点/本地网络出去。                                 |
| 直连模式 | 所有流量均从**本地网络**出去。                                                    |

## 服务模式

- 在操作系统中，子进程通常继承其父进程的权限级别，管理员身份启动的服务所“拉起”的子进程也会具有管理员权限。
- 以管理员身份安装并启动服务模式后，由服务进程“拉起”代理内核程序。代理内核程序便成为服务进程的子进程，运行在管理员权限下。
- 因此，服务模式的用途是能够以非管理员身份启动 TUN 模式。**2.x**版本起服务模式会在首次启动时自动安装。
- TUN 模式的默认堆栈是 `GVisor` ，**使用TUN模式需要自行对系统防火墙放行内核**`verge-mihomo`和`verge-mihomo-alpha`。不同堆栈的区别详见[TUN Stack](https://wiki.metacubex.one/config/inbound/tun/#stack)。
- `clash-verge-service`是独立于CVR应用程序之外的进程，给与管理员授权后用于无感拉起其他服务，不会随CVR关闭。当退出CVR后，服务模式进程`clash-verge-service`会继续在后台运行。不会影响系统。

## UA

- 指 `User Agent`，是HTTP请求标头中的一个字段。用于服务端判断客户端类型，从而做出不同的响应。
- 你的订阅服务商通过请求头中的`UA`字段判断你所使用的代理工具，从而下发不同的配置文件。

如何验证订阅是否正确?

**Windows Powershell**

```PowerShell
# 第一行粘贴订阅链接
$sub='在单引号内粘贴订阅链接,input subscription link'
# 第二行执行HTTP请求
Invoke-RestMethod -UserAgent 'clash-verge/v2.4.0' -Method get  -FollowRelLink -uri $sub
```

**Curl**

```shell
curl -A clash-verge/v2.4.0 ‘引号内粘贴订阅链接’
```

若以上命令返回响应正常的以YAML格式的mihomo配置文件即验证正确。

## Meta 内核

- 一般指[Clash Meta](https://github.com/MetaCubeX/mihomo/releases/latest)，也称 `Meta`、 `Mihomo` 内核。区别于 `Clash Premium` 为闭源内核，`Mihomo` 为开源内核。

## Alpha 内核

- 一般指[Clash Meta Alpha](https://github.com/MetaCubeX/mihomo/releases/tag/Prerelease-Alpha)，是 `Mihomo` 内核的测试版本。更新频率高，通常可以得到最新的 bug 修复 或 新特性。

## CFW

- 一般指`Clash For Windows`，是一款的基于 `Clash Premium` 内核的全平台代理软件（虽然叫做 For Windows）。
- 2023 年 11 月 2 日宣布停止更新，并删除发布仓库。

## CFA

- 一般指`Clash For Andriod`，是 Andriod 平台的基于 `Clash Premium` 内核的代理软件。
- 受`Clash For Windows`删库事件影响，删除代码库并从 Google Play 下架。

## CMFA

- 一般指[Clash Meta For Andriod](https://github.com/MetaCubeX/ClashMetaForAndroid/releases/latest)，是 Andriod 平台的基于 `Mihomo` 内核的代理软件。Google Play 已下架。

## XD

- 一般指[metacubexd](https://github.com/MetaCubeX/metacubexd)，是一个基于 `Mihomo` 的 WEB UI 面板。更多面板详见[友情链接](../friendship.md#web-ui)。

## ZASH

- 一般指[ZashBoard](https://github.com/Zephyruso/zashboard)，是一个使用Clash API的WebUI面板。

## OC

- 一般指[Open Clash](https://github.com/vernesong/OpenClash)，是 嵌入式/Linux 平台的基于 `Clash` 内核的面板程序。

## SC

- 一般指[Shell Crash](https://github.com/juewuy/ShellCrash)，是 嵌入式/Linux 平台的兼容多种内核的面板程序。

## Nikki

- 一般指[OpenWrt-nikki](https://github.com/nikkinikki-org/OpenWrt-nikki)，是 嵌入式/Linux平台的，适用于`OpenWRT`的`Mihomo`内核的面板程序。原名是`Mihomo Tproxy`。

## WARP

- Cloudflare 旗下的一款免费 VPN 软件。与传统 VPN 不同的是，它结合了 Cloudflare 的全球边缘网络。
- 使用 Wireguard 协议，是否能直连到 WARP 节点受运营商影响。
- 对于托管在 Cloudflare 的网站，WARP 并不能隐藏 IP。


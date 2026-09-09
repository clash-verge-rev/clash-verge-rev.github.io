# 隐私政策

**适用范围：** Clash Verge Rev 桌面应用（Windows、macOS、Linux），及其发布于
<https://github.com/clash-verge-rev/clash-verge-rev/releases> 的官方安装包。

**最后更新：** 2026-09-09

英文版本（English）：[Privacy Policy](https://github.com/clash-verge-rev/clash-verge-rev/blob/main/PRIVACY.md)

Clash Verge Rev 是 [mihomo](https://github.com/MetaCubeX/mihomo) 内核的开源图形客户端，
由志愿者维护。本项目没有公司实体，没有账号体系，也没有任何由本项目运营、
供应用回传数据的服务器。

## 1. 概要

- 本项目**不收集任何数据**。没有遥测、没有统计分析、没有崩溃或使用情况上报、
  没有广告或追踪 SDK、无需注册。任何数据都不会发送给维护者或本项目的基础设施。
- 你的配置、凭据与日志**仅保存在你自己的设备上**，都是可随时查看、备份或删除的
  明文文件（见第 3 节）。
- 应用只会连接你自己配置的地址（代理服务器、订阅链接、备份服务器），
  或第 4 节列出的第三方服务。这些第三方服务均不由本项目运营。
- 默认开启、且不属于你自身代理配置的连接只有两项：**应用更新检查**（4.3）与
  首页的 **IP 信息卡片**（4.5）。关闭方式见第 6 节。

## 2. 责任主体

Clash Verge Rev 维护者，可通过项目 Issue 区联系：
<https://github.com/clash-verge-rev/clash-verge-rev/issues>。
由于应用不进行任何数据收集，不存在数据控制者关系，也没有可供查询或删除的数据。

## 3. 保存在你设备上的数据

应用数据全部位于同一个目录：

| 平台 | 位置 |
| --- | --- |
| Windows | `%APPDATA%\io.github.clash-verge-rev.clash-verge-rev` |
| macOS | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev` |
| Linux | `$XDG_DATA_HOME/io.github.clash-verge-rev.clash-verge-rev`（通常为 `~/.local/share/…`） |

其中包含：

- `verge.yaml` —— 应用设置。若你配置了 WebDAV 备份，服务器地址、用户名与密码
  **以明文形式**保存在此文件中。
- `config.yaml`、`profiles.yaml` 与 `profiles/` —— 你的代理配置与已下载的订阅内容，
  通常包含**订阅链接（往往内嵌个人 token）、节点地址、端口、密码与 UUID**。
- `logs/` 与 `service-logs/` —— 应用日志与特权服务日志。根据你选择的日志等级，
  内核日志可能包含请求的域名、IP 地址与连接元数据。
- 内核运行文件（例如 mihomo 的 `cache.db` 以及 GeoIP/GeoSite 数据库）。

除非你启用 WebDAV 备份（4.9）、自行导出备份，或把这些文件附在错误报告里，
否则它们不会离开你的设备。

应用日志会在你设定的保留期后自动清理：*设置 → 杂项设置 → 自动清理日志*
（默认 7 天；选择「不清理」则永久保留）。卸载应用并删除上述目录即可清除其余全部数据。

!!! warning "提交错误报告前请先脱敏"
    日志与配置文件可能暴露你的节点信息、订阅 token 以及访问过的域名，
    附上之前请先检查并删除敏感内容。

## 4. 应用发起的网络连接

### 4.1 代理流量

经由应用转发的流量由内置 mihomo 内核处理，只会发往**你自己配置的**代理服务器。
本项目不会检查、记录或转发这些流量。你的代理服务商能够看到的内容，
与任何代理运营方相同，适用其自身的隐私政策。

### 4.2 订阅（配置文件）更新

添加远程配置文件时，应用会从你提供的链接下载内容，默认发送
`clash-verge/v<版本号>` 作为 `User-Agent`（可按订阅单独修改）。
若你为该订阅启用了自动更新，此请求会按你设定的间隔重复。
订阅提供方会看到你的 IP 地址与该次请求——与浏览器访问同一链接无异。
请求可按订阅配置为直连、走系统代理或走应用自身的代理端口。

### 4.3 应用更新检查 —— *默认开启*

应用启动时会依次向以下地址查询是否有新版本：

- `https://update.hwdns.net/…` 与 `https://gh-proxy.org/…`
  （第三方 GitHub 镜像，用于在受限网络下保持可达）
- `https://github.com/clash-verge-rev/clash-verge-rev/releases/…`

该请求只携带任何 HTTP 请求都会携带的信息：IP 地址、`User-Agent` 与所请求的文件。
应用不会生成或发送任何标识符，也不会把结果回报给本项目。
下载到的更新包会用编译进应用的 Minisign 公钥校验签名。

关闭方式：*设置 → 杂项设置 → 自动检查更新*。

### 4.4 内核更新 —— *手动触发*

更新 mihomo 内核时才会从 GitHub Releases
（`https://github.com/MetaCubeX/mihomo/releases/…`）下载，不会自动进行。

### 4.5 IP 信息卡片 —— *默认开启*

首页会显示你当前的公网出口 IP 及其归属地。为此应用会从以下服务中随机选择一个查询：
`api.ip.sb`、`ipapi.co`、`api.ipapi.is`、`ipwho.is`、`ip.api.skk.moe`、`get.geojs.io`。
仅当卡片处于可见状态且窗口未隐藏时，才会约每 5 分钟刷新一次；
窗口隐藏或不在首页时不会发送请求。

这类服务的设计目的就是识别并返回请求来源 IP——这正是该功能所需。
若代理处于开启状态，它们看到的是代理出口地址。
每个服务都由独立第三方按其自身隐私政策运营，本项目与它们没有任何协议。
若不希望产生这些请求，请不要停留在首页，或将应用最小化。

### 4.6 延迟测试 —— *手动触发*

测试节点或策略组时，会经由该节点向 `http://cp.cloudflare.com/generate_204`
（或你自定义的测试地址）发送一次 HTTP 请求。

### 4.7 流媒体解锁检测 —— *手动触发*

解锁检测会经由本地代理端口，向被检测的流媒体与 AI 服务
（Netflix、Disney+、YouTube、Spotify、OpenAI 等）发送请求。
只有你主动发起时才会运行，且只走你当前选择的代理。

### 4.8 DNS

生成的内核配置默认使用公共 DoH 解析器（`doh.pub`、`dns.alidns.com`），
你的订阅配置也可能定义了其他解析器。解析器可以看到被解析的域名。
你可以在 DNS 设置以及自己的配置中修改或替换它们。

### 4.9 WebDAV 备份 —— *未配置时不启用*

配置备份后，应用会向你指定的 WebDAV 服务器上传 ZIP 压缩包，
内含 profiles、`profiles.yaml`、`config.yaml`、DNS 配置与 `verge.yaml`。
**上传前会从 `verge.yaml` 中剔除 WebDAV 凭据，但 profiles 内的节点地址、
密码与订阅链接不会被剔除。** 请将备份视为敏感文件，并只使用你信任的服务器。
备份只会传输到该服务器。

### 4.10 本地监听

代理端口与内部控制端口仅监听 `127.0.0.1`。除非你主动把内核配置为允许局域网访问，
否则不会暴露到网络中。

## 5. 第三方组件与服务

应用内置或依赖的以下软件有其自身的行为与政策：

- **mihomo（Clash.Meta）内核** —— <https://github.com/MetaCubeX/mihomo>，
  实际执行代理与 DNS 解析。
- **Tauri 与系统 WebView** —— 界面由操作系统的网页引擎渲染：Windows 上为
  Microsoft Edge WebView2（适用
  [Microsoft 隐私声明](https://privacy.microsoft.com/privacystatement)），
  macOS 上为 WKWebView，Linux 上为 WebKitGTK。
- **外部面板** —— 打开 Web 面板（metacubexd、Yacd、Zash Board）会在浏览器中加载
  第三方站点，再由其与你本地的内核通信。
- **GitHub** —— 托管源码、发布包与更新元数据，可见下载请求；
  你在 Issue 中发布的内容是公开的。
- **你自行配置的代理/订阅服务商、WebDAV 与 DNS 提供方**独立于本项目，
  由其自行决定数据处理方式。

## 6. 你可以控制的开关

- *设置 → 杂项设置 → 自动检查更新* —— 关闭更新检查。
- *设置 → 杂项设置 → 自动清理日志* —— 设置日志保留期，或选择「不清理」。
- 每个订阅的更新间隔 —— 关闭订阅自动刷新。
- WebDAV 备份在你填入服务器信息前不会启用，且可随时清除。
- IP 信息卡片仅在首页可见时才会发起查询（见 4.5）。
- 其余数据都只是磁盘上的普通文件：删除应用数据目录即可清除全部存储内容。

## 7. 未成年人

本应用是网络工具，不含面向未成年人的内容，也不进行任何数据收集。

## 8. 政策变更

本政策的修改均在仓库中进行，可在 Git 历史中查看。页首日期为最近一次修改时间。

## 9. 联系方式

请在 <https://github.com/clash-verge-rev/clash-verge-rev/issues> 提交 Issue。
请勿在公开 Issue 中附带含有凭据的日志或配置文件。

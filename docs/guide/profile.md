## 远程订阅

> 远程订阅支持三种方式导入: 订阅链接导入、订阅链接配置、URL Schemes。

### 订阅链接导入

<!-- prettier-ignore -->
!!! warning
    如果通过`订阅链接导入`的方式提示 `client error(Connect)`，请尝试其他导入方式。

<video controls>
  <source src="../assets/guide/profile/remote_url.webm">
</video>

### 订阅链接配置

<!-- prettier-ignore -->
!!! warning
    如果通过`订阅链接配置`的方式仍然提示 `client error(Connect)`，请尝试勾选 `允许无效证书（危险）`，并保存重试。

<video controls>
  <source src="../assets/guide/profile/remote_config.webm">
</video>

### URL Schemes

<!-- prettier-ignore -->
!!! warning
    - Verge 2.0版以后，`macOS` 已支持通过 `URL Schemes` 的方式导入。
    - `Windows` 如果不能通过 `URL Schemes` 的方式导入，请查看[URL Schemes 功能失效解决办法](./url_schemes.md#_3)。

<video controls>
  <source src="../assets/guide/profile/remote_url_schemes.webm">
</video>

## 本地配置

> 本地配置支持两种方式导入: 新建配置文件导入、拖拽配置文件导入。

### 新建配置文件导入

<!-- prettier-ignore -->
!!! info
    - 选择文件不是必须的，直接保存会生成一份空配置文件。
    - 选择使用的文件会被复制一份到 `profiles` 目录，原文件被移动或删除不会有影响。

<video controls>
  <source src="../assets/guide/profile/local_config.webm">
</video>

### 拖拽配置文件导入

<!-- prettier-ignore -->
!!! warning
    要求 Clash Verge Rev 版本至少为 `v1.6.2`。

<video controls>
  <source src="../assets/guide/profile/local_drag.webm">
</video>

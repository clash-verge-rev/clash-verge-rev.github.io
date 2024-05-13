## 图标配置

需要配置代理组的 [icon](https://wiki.metacubex.one/config/proxy-groups/#icon) 字段。

```yaml
proxy-groups:
- name: 谷歌服务
  type: select
  interval: 300
  timeout: 3000
  url: https://www.google.com/generate_204
  lazy: true
  proxies: [省略]
  icon: https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/google.svg
```

### 支持类型

<!-- prettier-ignore -->
!!! tip
    - [附录](#_4)有图标集(彩色、亮色、暗色)、常见 `SVG` 图标。
    - 右键图片可以选择 `复制图片地址` 。

> 支持 `url图片地址` 、`Base64 编码字符串`、`svg文件内容` 三种类型。

- 使用 url 指定图片地址（支持常见图片格式，例如: `png`、`jpeg`、`gif` 以及 `svg`）。

```yaml
icon: https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/google.svg
```

- 使用 Base64 编码后的图片。[图片转 Base64 编码](https://www.jyshare.com/front-end/59/)。

```yaml
icon: data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4cHgiIGhlaWdodD0iNDhweCI+DQogICAgPHBhdGggZmlsbD0iI2ZiYzAyZCINCiAgICAgICAgZD0iTTQzLjYxMSwyMC4wODNINDJWMjBIMjR2OGgxMS4zMDNjLTEuNjQ5LDQuNjU3LTYuMDgsOC0xMS4zMDMsOGMtNi42MjcsMC0xMi01LjM3My0xMi0xMglzNS4zNzMtMTIsMTItMTJjMy4wNTksMCw1Ljg0MiwxLjE1NCw3Ljk2MSwzLjAzOWw1LjY1Ny01LjY1N0MzNC4wNDYsNi4wNTMsMjkuMjY4LDQsMjQsNEMxMi45NTUsNCw0LDEyLjk1NSw0LDI0czguOTU1LDIwLDIwLDIwCXMyMC04Ljk1NSwyMC0yMEM0NCwyMi42NTksNDMuODYyLDIxLjM1LDQzLjYxMSwyMC4wODN6IiAvPg0KICAgIDxwYXRoIGZpbGw9IiNlNTM5MzUiDQogICAgICAgIGQ9Ik02LjMwNiwxNC42OTFsNi41NzEsNC44MTlDMTQuNjU1LDE1LjEwOCwxOC45NjEsMTIsMjQsMTJjMy4wNTksMCw1Ljg0MiwxLjE1NCw3Ljk2MSwzLjAzOQlsNS42NTctNS42NTdDMzQuMDQ2LDYuMDUzLDI5LjI2OCw0LDI0LDRDMTYuMzE4LDQsOS42NTYsOC4zMzcsNi4zMDYsMTQuNjkxeiIgLz4NCiAgICA8cGF0aCBmaWxsPSIjNGNhZjUwIg0KICAgICAgICBkPSJNMjQsNDRjNS4xNjYsMCw5Ljg2LTEuOTc3LDEzLjQwOS01LjE5MmwtNi4xOS01LjIzOEMyOS4yMTEsMzUuMDkxLDI2LjcxNSwzNiwyNCwzNgljLTUuMjAyLDAtOS42MTktMy4zMTctMTEuMjgzLTcuOTQ2bC02LjUyMiw1LjAyNUM5LjUwNSwzOS41NTYsMTYuMjI3LDQ0LDI0LDQ0eiIgLz4NCiAgICA8cGF0aCBmaWxsPSIjMTU2NWMwIg0KICAgICAgICBkPSJNNDMuNjExLDIwLjA4M0w0My41OTUsMjBMNDIsMjBIMjR2OGgxMS4zMDNjLTAuNzkyLDIuMjM3LTIuMjMxLDQuMTY2LTQuMDg3LDUuNTcxCWMwLjAwMS0wLjAwMSwwLjAwMi0wLjAwMSwwLjAwMy0wLjAwMmw2LjE5LDUuMjM4QzM2Ljk3MSwzOS4yMDUsNDQsMzQsNDQsMjRDNDQsMjIuNjU5LDQzLjg2MiwyMS4zNSw0My42MTEsMjAuMDgzeiIgLz4NCjwvc3ZnPg==
```

- 使用 `svg` 文件的内容（需要对引号进行转义）。

```yaml
icon: "<svg xmlns=\"http://www.w3.org/2000/svg\" viewBox=\"0 0 48 48\" width=\"48px\" height=\"48px\"><path fill=\"#fbc02d\"
        d=\"M43.611,20.083H42V20H24v8h11.303c-1.649,4.657-6.08,8-11.303,8c-6.627,0-12-5.373-12-12	s5.373-12,12-12c3.059,0,5.842,1.154,7.961,3.039l5.657-5.657C34.046,6.053,29.268,4,24,4C12.955,4,4,12.955,4,24s8.955,20,20,20	s20-8.955,20-20C44,22.659,43.862,21.35,43.611,20.083z\" /><path fill=\"#e53935\"
        d=\"M6.306,14.691l6.571,4.819C14.655,15.108,18.961,12,24,12c3.059,0,5.842,1.154,7.961,3.039	l5.657-5.657C34.046,6.053,29.268,4,24,4C16.318,4,9.656,8.337,6.306,14.691z\" /><path fill=\"#4caf50\"
        d=\"M24,44c5.166,0,9.86-1.977,13.409-5.192l-6.19-5.238C29.211,35.091,26.715,36,24,36	c-5.202,0-9.619-3.317-11.283-7.946l-6.522,5.025C9.505,39.556,16.227,44,24,44z\" /><path fill=\"#1565c0\"
        d=\"M43.611,20.083L43.595,20L42,20H24v8h11.303c-0.792,2.237-2.231,4.166-4.087,5.571	c0.001-0.001,0.002-0.001,0.003-0.002l6.19,5.238C36.971,39.205,44,34,44,24C44,22.659,43.862,21.35,43.611,20.083z\" /></svg>"
```

### 图标缓存

> 程序会对下载的 icon 进行缓存。

缓存的目录为`应用目录`\icons\cache 。

## 附录

### 图标集

> 网络上较为流行的图标集。

<!-- prettier-ignore -->
!!! tip
    Clash Verge 的代理组图标大小为 **`32 * 32`** 像素，并会为图标添加**圆角样式**。

| 图标集                                      | 仓库地址                                                                                                                                               | 图标格式 | 图标集信息           | 图标预览                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | -------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Qure](./icon_sets/Qure.md)                 | <a href="https://github.com/Koolson/Qure" target="_blank"><img src="https://img.shields.io/github/stars/Koolson/Qure"></a>                             | `png`    | 彩色、亮色、暗色图标 | <img src="https://fastly.jsdelivr.net/gh/Koolson/Qure@master/IconSet/Color/Google_Search.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/Koolson/Qure@master/IconSet/Color/Hong_Kong.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/Koolson/Qure@master/IconSet/Color/United_States.png" width="48"/>                                                                                   |
| [mini](./icon_sets/mini.md)                 | <a href="https://github.com/Orz-3/mini" target="_blank"><img src="https://img.shields.io/github/stars/Orz-3/mini"></a>                                 | `png`    | 彩色、暗色图标       | <img src="https://fastly.jsdelivr.net/gh/Orz-3/mini@master/Color/Google.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/Orz-3/mini@master/Color/HK.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/Orz-3/mini@master/Color/US.png" width="48"/>                                                                                                                                          |
| [flag-icons](./icon_sets/flag-icons.md)     | <a href="https://github.com/lipis/flag-icons" target="_blank"><img src="https://img.shields.io/github/stars/lipis/flag-icons"></a>                     | `svg`    | 标准 3:2 国旗图标    | <img src="https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/hk.svg" width="48"/> <img src="https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/us.svg" width="48"/> <img src="https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/flags/tw.svg" width="48"/> |
| [WHATSINStash](./icon_sets/WHATSINStash.md) | <a href="https://github.com/shindgewongxj/WHATSINStash" target="_blank"><img src="https://img.shields.io/github/stars/shindgewongxj/WHATSINStash"></a> | `png`    | 彩色图标             | <img src="https://fastly.jsdelivr.net/gh/shindgewongxj/WHATSINStash@master/icon/google.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/shindgewongxj/WHATSINStash@master/icon/youtube.png" width="48"/> <img src="https://fastly.jsdelivr.net/gh/shindgewongxj/WHATSINStash@master/icon/telegram.png" width="48"/>                                                                                  |

### 常见图标

> 一些常见 `SVG` 图标。

|                                                                              |                                                                                  |                                                                             |                                                                                  |                                                                                 |                                                                                  |
| ---------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | --------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| <img src="../../assets/icons/adjust.svg" width="48"/><div>节点选择</div>     | <img src="../../assets/icons/speed.svg" width="48"/><div>延迟选优</div>          | <img src="../../assets/icons/ambulance.svg" width="48"/><div>故障转移</div> | <img src="../../assets/icons/merry_go.svg" width="48"/><div>负载均衡(散列)</div> | <img src="../../assets/icons/balance.svg" width="48"/><div>负载均衡(轮询)</div> | <img src="../../assets/icons/link.svg" width="48"/><div>全局直连</div>           |
| <img src="../../assets/icons/block.svg" width="48"/><div>全局拦截</div>      | <img src="../../assets/icons/bug.svg" width="48"/><div>广告过滤</div>            | <img src="../../assets/icons/guard.svg" width="48"/><div>应用净化</div>     | <img src="../../assets/icons/cloudflare.svg" width="48"/><div>Cloudflare</div>   | <img src="../../assets/icons/warp.svg" width="48"/><div>WARP</div>              | <img src="../../assets/icons/fish.svg" width="48"/><div>漏网之鱼</div>           |
| <img src="../../assets/icons/microsoft.svg" width="48"/><div>Microsoft</div> | <img src="../../assets/icons/linux.svg" width="48"/><div>Linux</div>             | <img src="../../assets/icons/macos.svg" width="48"/><div>MacOS</div>        | <img src="../../assets/icons/android.svg" width="48"/><div>Andriod</div>         | <img src="../../assets/icons/apple.svg" width="48"/><div>Apple</div>            | <img src="../../assets/icons/google_play.svg" width="48"/><div>Google Play</div> |
| <img src="../../assets/icons/google.svg" width="48"/><div>Google</div>       | <img src="../../assets/icons/youtube.svg" width="48"/><div>Youtube</div>         | <img src="../../assets/icons/telegram.svg" width="48"/><div>Telegram</div>  | <img src="../../assets/icons/twitter.svg" width="48"/><div>Twitter</div>         | <img src="../../assets/icons/instagram.svg" width="48"/><div>Instagram</div>    | <img src="../../assets/icons/facebook.svg" width="48"/><div>Facebook</div>       |
| <img src="../../assets/icons/netflix.svg" width="48"/><div>Netflix</div>     | <img src="../../assets/icons/disney_plus.svg" width="48"/><div>Disney Plus</div> | <img src="../../assets/icons/tiktok.svg" width="48"/><div>Tiktok</div>      | <img src="../../assets/icons/chatgpt.svg" width="48"/><div>ChatGPT</div>         | <img src="../../assets/icons/claude.svg" width="48"/><div>Claude</div>          | <img src="../../assets/icons/bing.svg" width="48"/><div>Bing</div>               |
| <img src="../../assets/icons/discord.svg" width="48"/><div>Discord</div>     | <img src="../../assets/icons/reddit.svg" width="48"/><div>Reddit</div>           | <img src="../../assets/icons/whatsapp.svg" width="48"/><div>WhatsApp</div>  | <img src="../../assets/icons/onedrive.svg" width="48"/><div>OneDrive</div>       | <img src="../../assets/icons/github.svg" width="48"/><div>Github</div>          | <img src="../../assets/icons/amazon.svg" width="48"/><div>Amazon</div>           |
| <img src="../../assets/icons/steam.svg" width="48"/><div>Steam</div>         | <img src="../../assets/icons/epic.svg" width="48"/><div>Epic</div>               | <img src="../../assets/icons/xbox.svg" width="48"/><div>XBox</div>          |                                                                                  |                                                                                 |                                                                                  |

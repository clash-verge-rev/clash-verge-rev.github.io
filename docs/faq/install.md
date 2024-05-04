## Verge 无法正常启动

> 提示异常代码: 0xc0000409

修复、卸载、重新安装 [WebView2](https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download)。

## WebView2 无法正常安装

可能是你的 Windows 系统关闭了自动更新，请打开自动更新。

## Ubuntu 24.04 版本无法正常安装

Ubuntu `24.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，根据架构下载对应版本并安装。

| 架构 | 下载地址                                                                                                                                                                           |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| x64  | [libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb)               |
| x64  | [libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb) |

```bash
sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb
```

| 架构  | 下载地址                                                                                                                                                                           |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| arm64 | [libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb)               |
| arm64 | [libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb) |

```bash
sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb
```

| 架构  | 下载地址                                                                                                                                                                           |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| armv7 | [libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb)               |
| armv7 | [libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb) |

```bash
sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb
```

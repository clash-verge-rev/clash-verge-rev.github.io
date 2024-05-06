## Verge 无法正常启动

> 提示异常代码: 0xc0000409

修复、卸载、重新安装 [WebView2](https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download)。

## WebView2 无法正常安装

可能是你的 Windows 系统关闭了自动更新，请打开自动更新。

## Ubuntu 24.04 版本无法正常安装

Ubuntu `24.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，根据架构下载对应版本并安装。
=== "amd64"

    | 依赖                 | 下载地址                                                                                                                                                                           |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | `libwebkit2gtk`  | [libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb)               |
    | `libjavascriptcoregtk`  | [libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb) |

    ```bash
    sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb
    ```

=== "arm64"

    | 依赖                 | 下载地址                                                                                                                                                                           |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | `libwebkit2gtk` | [libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb)               |
    | `libjavascriptcoregtk` | [libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb) |

    ```bash
    sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_arm64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_arm64.deb
    ```

=== "armv7"

    | 依赖                 | 下载地址                                                                                                                                                                           |
    | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | `libwebkit2gtk`        | [libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb)               |
    | `libjavascriptcoregtk` | [libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb](https://github.com/clash-verge-rev/clash-verge-rev/releases/download/dependencies/libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb) |

    ```bash
    sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_armhf.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_armhf.deb
    ```

## 点击托盘菜单没有反应

> Windows 禁用或删除了系统 WebView2

问题原因

- Tauri 框架依赖于 `WebView2`。如果卸载或禁用了 `WebView2` ,将无法显示界面。具体表现为: 程序可以启动，但是点击托盘菜单没有反应。

解决方案

- 如果是利用第三方软件禁用了 `Edge`，请检查是否同时禁用了 `WebView2`，将 `WebView2` 取消禁用。
- 如果是卸载了 `WebView2`，可以[下载 WebView2 安装包](https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download)，重新安装 WebView2。
- 如果是企业版系统不方便安装 WebView2，请尝试在 [Release](https://github.com/clash-verge-rev/clash-verge-rev/releases/latest) 下载内置了 `WebView2` 的版本（带有 `fix_webview2` 字样的安装包）。
- 若问题仍然存在，请尝试使用 **Windows 7 兼容模式**启动。

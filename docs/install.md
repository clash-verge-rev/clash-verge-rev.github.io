## 下载地址

> Clash Verge Rev 目前仅通过 GitHub Release 发布，请注意辨别。

| 下载方式                  | 下载次数                                                                                                          | 下载地址                                                                                                                                                                                |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Github Release **正式版** | <img src="https://img.shields.io/github/downloads/clash-verge-rev/clash-verge-rev/latest/total?label=@latest">    | <a href='https://github.com/clash-verge-rev/clash-verge-rev/releases/latest' target="_blank"><img src="https://img.shields.io/github/v/release/clash-verge-rev/clash-verge-rev"></a>    |
| Github Release **测试版** | <img src="https://img.shields.io/github/downloads-pre/clash-verge-rev/clash-verge-rev/latest/total?label=@alpha"> | <a href='https://github.com/clash-verge-rev/clash-verge-rev/releases/tag/alpha' target="_blank"><img src="https://img.shields.io/github/v/release/clash-verge-rev/clash-verge-rev"></a> |

## 版本选择

=== ":material-microsoft-windows: Windows"

    <!-- prettier-ignore -->
    !!! warning
        - **Windows 7** 用户请先查看相关[FAQ](./faq/windows.md#windows-7)。
        - 带有 `fix_webview2` 字样的安装包为内置 `Webview2` 环境版本（该文件体积比普通安装包大，仅用于当系统缺少且[无法安装WebView2](./faq/windows.md#webview2)环境时使用）。

    ### 安装版
    <!-- prettier-ignore -->
    !!! tip
        如果你不清楚你的电脑系统架构，请下载 `x64` 架构文件（目前多数 Windows 电脑使用该架构）。

    | 系统架构 | 文件名称                                 |
    | -------- | ---------------------------------------- |
    | x86      | Clash.Verge`_版本号_`**x86-setup.exe**   |
    | x64      | Clash.Verge`_版本号_`**x64-setup.exe**   |
    | arm64    | Clash.Verge`_版本号_`**arm64-setup.exe** |

    ### 便携版
    <!-- prettier-ignore -->
    !!! warning
        - 带有 `portable` 字样的 zip 压缩包是便携版，下载后解压即可使用。
        - 便携版无法使用应用内更新功能，需要手动下载新版本的**便携版**，并解压**覆盖**旧版本。

    - 便携版通过检测目录下的 `.config/PORTABLE` 文件来判断是否为便携版。
    - 如果你想要和安装版使用相同的配置文件路径，删除 `.config/PORTABLE` 文件即可。

=== ":material-linux: Linux"

    #### 安装方式
    <!-- prettier-ignore -->
    !!! warning
        仅提供 `deb` 和 `AppImage` 两种安装包。

    | Linux 发行版本          | 安装方式                                                                       |
    | ----------------------- | ------------------------------------------------------------------------------ |
    | `Ubuntu` / `Debian`     | 下载 `deb` 安装包                                                              |
    | `ArchLinux` / `Manjaro` | 通过 `AUR` 安装                                                                |
    | 其他发行版              | 方式 1: 解压 `deb` 包得到可执行文件，重新打包<br />方式 2: 直接使用 `AppImage` |

    #### 文件选择

    | 系统架构 | 文件名称                                                                        |
    | -------- | ------------------------------------------------------------------------------- |
    | x64      | clash-verge`_版本号_`**amd64.AppImage**<br />clash-verge`_版本号_`**amd64.deb** |
    | arm64    | clash-verge`_版本号_`**arm64.deb**                                              |
    | armv7    | clash-verge`_版本号_`**armhf.deb**                                              |

=== ":material-apple: macOS"

    <!-- prettier-ignore -->
    !!! warning
        不支持 `macos` 10 操作系统，请升级 `macos` 到 11 或 更高版本。

    | 芯片类型     | 文件名称                             |
    | ------------ | ------------------------------------ |
    | Intel 芯片   | Clash.Verge`_版本号_`**x64.dmg**     |
    | Apple M 芯片 | Clash.Verge`_版本号_`**aarch64.dmg** |

## 安装问题

如果安装/使用过程中遇到了问题，请参考文档中的[常见问题](./faq/windows.md)。

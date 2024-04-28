## 下载地址

> Clash Verge Rev 目前仅通过 GitHub Release 发布，请注意辨别。

| 下载方式       | 下载地址                                                                                                                                                                                                  |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Github Release | <a href='https://github.com/clash-verge-rev/clash-verge-rev/releases/latest' target="_blank"><img alt="GitHub Release" src="https://img.shields.io/github/v/release/clash-verge-rev/clash-verge-rev"></a> |

## 版本选择

### Windows

> Clash Verge Rev 支持 `x64`、`arm64 `架构的 Windows 操作系统。

<!-- prettier-ignore -->
!!! warning
    不支持 Windows 7 操作系统。

| 系统架构 | 文件名称                             |
| -------- | ------------------------------------ |
| x64      | Clash.Verge\_版本号\_x64-setup.exe   |
| arm64    | Clash.Verge\_版本号\_arm64-setup.exe |

### Linux

> Clash Verge Rev 支持 `x64`、`arm64` 架构的 Linux 操作系统。

#### 发行版本

<!-- prettier-ignore -->
!!! warning
    仅提供 `deb` 和 `AppImage` 两种安装包。

| 发行版本          | 安装方式                                                                     |
| ----------------- | ---------------------------------------------------------------------------- |
| Ubuntu/Debian     | 下载 `deb` 安装包                                                            |
| ArchLinux/Manjaro | 通过 AUR 安装                                                                |
| 其他发行版        | 方式 1: 解压 `deb` 包得到可执行文件，重新打包<br />方式 2: 直接使用 AppImage |

#### 文件选择

| 系统架构 | 文件名称                                                                |
| -------- | ----------------------------------------------------------------------- |
| x64      | clash-verge\_版本号\_amd64.AppImage<br />clash-verge\_版本号\_amd64.deb |
| arm64    | clash-verge\_版本号\_arm64.deb                                          |
| armv7    | clash-verge\_版本号\_armhf.deb                                          |

### MacOS

> Clash Verge Rev 支持 `x64`、`arm64 `架构的 MacOS 系统。

<!-- prettier-ignore -->
!!! warning
    不支持 macos 10 操作系统，请升级 macos 到 11 或以上版本。

| 芯片类型     | 文件名称                         |
| ------------ | -------------------------------- |
| Intel 芯片   | Clash.Verge\_版本号\_x64.dmg     |
| Apple M 芯片 | Clash.Verge\_版本号\_aarch64.dmg |

### Portable(便携版)

> 带有 `portable` 字样的 zip 压缩包是便携版，下载后解压即可使用。

<!-- prettier-ignore -->
!!! warning
    便携版无法使用应用内更新功能，需要手动下载新版本的**便携版**，并解压**覆盖**旧版本。

便携版通过检测目录下的 `.config/PORTABLE` 文件来判断是否为便携版。

如果你想要和安装版使用相同的配置文件路径，删除 `.config/PORTABLE` 文件即可。

## Ubuntu 24.04 版本无法正常安装

Ubuntu `24.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，根据架构下载对应版本并安装。其他 Debian 系操作系统类似。
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

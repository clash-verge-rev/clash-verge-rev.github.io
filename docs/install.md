## 操作系统

=== ":material-microsoft-windows: Windows"

    <!-- prettier-ignore -->
    !!! warning
        - 如果你不清楚你的电脑系统架构，请下载 `x64` 架构文件（目前多数 Windows 电脑使用该架构）。
        - 2.0版开始，首次启动软件会自动尝试卸载和安装服务(uninstall-service和install-service)，必须同意安装，否则无法正常运行Clash内核。
        - **Windows 7** 用户请先查看相关[FAQ](./faq/windows.md#windows-7)。
        - 带有 `fix_webview2` 字样的安装包为内置 `Webview2` 环境版本（该文件体积比普通安装包大，仅用于当系统缺少且[无法安装WebView2](./faq/windows.md#webview2)环境时使用）。

    #### 安装版

    | 系统架构 | 下载地址 |
    | -------- | -------- |
    | x64 | <list class="download-list"><item logo="windows" label="安装包" content="x64" keyword="x64-setup.exe" color="0078D7">加载中...</item><item logo="microsoftedge"label="内置 Webview2 安装包" content="x64" keyword="x64_fixed_webview2-setup.exe" color="4CCE66"><item></list> |
    | x86 | <list class="download-list"><item logo="windows" label="安装包" content="x86" keyword="x86-setup.exe" color="539CDE">加载中...</item><item logo="microsoftedge"label="内置 Webview2 安装包" content="x86" keyword="x86_fixed_webview2-setup.exe" color="75DB87"><item></list> |
    | arm64 | <list class="download-list"><item logo="windows" label="安装包" content="arm64" keyword="arm64-setup.exe" color="8BB2E5">加载中...</item><item logo="microsoftedge"label="内置 Webview2 安装包" content="arm64" keyword="arm64_fixed_webview2-setup.exe" color="A4E4AD"><item></list> |

    #### 便携版(因为问题较多2.0后不再提供)
    <!-- prettier-ignore -->
    !!! warning
        - 带有 `portable` 字样的 zip 压缩包是便携版，下载后解压即可使用。
        - 便携版无法使用应用内更新功能，需要手动下载新版本的**便携版**，并解压**覆盖**旧版本。

    - 便携版通过检测目录下的 `.config/PORTABLE` 文件来判断是否为便携版。
    - 如果你想要和安装版使用相同的配置文件路径，删除 `.config/PORTABLE` 文件即可。

        | 系统架构 | 下载地址  |
        | ------- | ------- |
        | x64 | <list class="download-list"><item logo="windows" label="便携版" content="x64" keyword="x64_portable.zip" color="0078D7">加载中...</item><item logo="microsoftedge"label="内置 Webview2 便携版" content="x64" keyword="x64_fixed_webview2_portable.zip" color="4CCE66"><item></list> |
        | x86 | <list class="download-list"><item logo="windows" label="便携版" content="x86" keyword="x86_portable.zip" color="539CDE">加载中...</item><item logo="microsoftedge"label="内置 Webview2 便携版" content="x86" keyword="x86_fixed_webview2_portable.zip" color="75DB87"><item></list> |
        | arm64 | <list class="download-list"><item logo="windows" label="便携版" content="arm64" keyword="arm64_portable.zip" color="8BB2E5">加载中...</item><item logo="microsoftedge"label="内置 Webview2 便携版" content="arm64" keyword="arm64_fixed_webview2_portable.zip" color="A4E4AD"><item></list> |

=== ":material-linux: Linux"

    === ":material-debian: Debian/Ubuntu/Deepin"

        <!-- prettier-ignore -->
        !!! warning
            Ubuntu `24.04` 需要安装额外依赖，详见[常见问题](./faq/linux.md)。

        | 系统架构 | 下载地址 |
        | -------- | --------------------------------------------------------------------------------------------------------------------------------------- |
        | x64      | <list class="download-list"><item logo="debian" label="安装包" content="x64" keyword="amd64.deb" color="A80030">加载中...</item></list>   |
        | x86      | <list class="download-list"><item logo="debian" label="安装包" content="x86" keyword="i386.deb" color="C02B4A">加载中...</item></list>    |
        | arm64    | <list class="download-list"><item logo="debian" label="安装包" content="arm64" keyword="arm64.deb" color="D44E64">加载中...</item></list> |
        | armv7    | <list class="download-list"><item logo="debian" label="安装包" content="armv7" keyword="armhf.deb" color="E9717E">加载中...</item></list> |
     
        下载上方deb包后，使用apt安装：
        ```
        sudo apt install -y ./Clash.Verge_x.x.x-_xxx.deb
        ```

    === ":material-redhat: CentOS/Fedora/SUSE"

        | 系统架构 | 下载地址 |
        | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
        | x64      | <list class="download-list"><item logo="redhat" label="安装包" content="x64" keyword="x86_64.rpm" color="CC0000">加载中...</item></list>    |
        | x86      | <list class="download-list"><item logo="redhat" label="安装包" content="x86" keyword="i386.rpm" color="E63434">加载中...</item></list>      |
        | arm64    | <list class="download-list"><item logo="redhat" label="安装包" content="arm64" keyword="aarch64.rpm" color="F14D4D">加载中...</item></list> |
        | armv7    | <list class="download-list"><item logo="redhat" label="安装包" content="armv7" keyword="armhfp.rpm" color="FF6868">加载中...</item></list>  |
        
        下载上方rpm包后，使用dnf/yum安装：
        ```
        sudo dnf install ./Clash.Verge_x.x.x-_xxx.rpm
        sudo yum localinstall ./Clash.Verge_x.x.x-_xxx.rpm
        ```

    === ":material-arch: ArchLinux/Manjaro/SteamDeck"
        === "paru"

            1.  安装 `paru`。
            
                1.1.  在 `/etc/pacman.conf` 文件中写入下列内容。
            
                ```
                [archlinuxcn]
                Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
                Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
                Server = https://mirrors.hit.edu.cn/archlinuxcn/$arch
                Server = https://repo.huaweicloud.com/archlinuxcn/$arch
                ```
            
                1.2. 在终端运行下列命令。
            
                ```bash
                sudo pacman-key --lsign-key "farseerfc@archlinux.org"
                sudo pacman -S archlinuxcn-keyring
                sudo pacman -S paru
                ```
            
            2.  安装 `clash-verge-rev-bin`。
            
            ```bash
            paru -S clash-verge-rev-bin
            ```

        === "yay"
            3.  安装 `paru`。
            
                1.1.  在 `/etc/pacman.conf` 文件中写入下列内容。
            
                ```
                [archlinuxcn]
                Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
                Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
                Server = https://mirrors.hit.edu.cn/archlinuxcn/$arch
                Server = https://repo.huaweicloud.com/archlinuxcn/$arch
                ```
            
                1.2. 在终端运行下列命令。
            
                ```bash
                sudo pacman-key --lsign-key "farseerfc@archlinux.org"
                sudo pacman -S archlinuxcn-keyring
                sudo pacman -S yay
                ```
            
            4.  安装 `clash-verge-rev-bin`。
            
            ```bash
            yay -S clash-verge-rev-bin
            ```

=== ":material-apple: macOS"

    <!-- prettier-ignore -->
    !!! warning
        不支持 `macos` 10 操作系统，请升级 `macos` 到 11 或 更高版本。

    | 系统架构 | 下载地址 |
    | ------- | ------ |
    | Intel 芯片 | <list class="download-list"><item logo="apple" label="安装包" content="x64" keyword="x64.dmg" color="A8A8A8">加载中...</item></list> |
    | Apple M 芯片 | <list class="download-list"><item logo="apple" label="安装包" content="aarch64" keyword="aarch64.dmg" color="C4C4C4">加载中...</item></list> |

    ![mac_install](./assets/guide/quickstart/mac_install.png)

## 安装问题

如果安装/使用过程中遇到了问题，请参考文档中的[常见问题](./faq/windows.md)。

## 发布地址

> Clash Verge Rev 目前仅通过 GitHub Release 发布，请注意辨别。

| 发行版本 | 下载次数 | 下载地址  | 备注 |
| ------- | ------ | -------- | ---- |
| Github Release **正式版** | <img src="https://img.shields.io/github/downloads/clash-verge-rev/clash-verge-rev/latest/total?label=@latest"> | <a href='https://github.com/clash-verge-rev/clash-verge-rev/releases/latest' target="_blank"><img src="https://img.shields.io/github/v/release/clash-verge-rev/clash-verge-rev"></a>  | |
| Github Release **测试版** | <img src="https://img.shields.io/github/downloads-pre/clash-verge-rev/clash-verge-rev/latest/total?label=@alpha"> | <a href='https://github.com/clash-verge-rev/clash-verge-rev/releases/tag/alpha' target="_blank"><img src="https://img.shields.io/github/v/release/clash-verge-rev/clash-verge-rev"></a> | 无法通过应用内更新，升级到最新测试版 |

<script>
const fileList = [];
const divList = document.querySelectorAll("list item");
const githubLink = "https://github.com/clash-verge-rev/clash-verge-rev/releases";
(async () => {
  const link = "https://api.github.com/repos/clash-verge-rev/clash-verge-rev/releases/latest";
  const { assets } = await fetch(link).then((r) => r.json());
  for (const { name, browser_download_url: url } of assets) {
    fileList.push({ name, url });
  }
  for (const div of divList) {
    const logo = div.getAttribute("logo");
    const label = div.getAttribute("label");
    const keyword = div.getAttribute("keyword");
    const content = div.getAttribute("content");
    const color = div.getAttribute("color") ?? "44CC11";
    div.innerHTML = fileList.map(({ name, url }) => {
      if (name.endsWith(keyword)) {
        const a = document.createElement("a");
        a.href = url;
        const img = document.createElement("img");
        img.src = `https://img.shields.io/badge/${label}-${content}-${color}?logo=${logo}`;
        a.appendChild(img);
        return a.outerHTML;
      }
      return "";
    }).join("");
  }
})();
</script>
<style>
list{
  display: flex;
  gap: 8px;
}
</style>

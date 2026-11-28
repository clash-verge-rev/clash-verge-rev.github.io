## Ubuntu 20.04 版本无法正常安装

Ubuntu `20.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，但安装过程繁琐，依赖关系复杂，建议升级最新的`24.04`。其他 Debian 系操作系统类似。

## 无 UI 图形化界面能用吗

不能。Clash Verge Rev 是基于 `Webview` 的 GUI 程序，需要图形化界面支持。无图形化界面请下载 [MetaCubeX/Mihomo](https://github.com/MetaCubeX/mihomo/releases/latest) 内核二进制程序，通过命令行的方式使用（参数 `--help` 查看帮助）。

## 窗口标题栏按钮（最小化/最大化/关闭）失效

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/11856)，打开设置 -> 界面设置 -> 关闭「优先使用系统标题栏」。

## 进入代理页面后侧边栏无法切换，WebKit 网络进程崩溃

在 Ubuntu 22.04.5、GNOME X11、Clash Verge Rev 2.5.8 的一例[问题报告](https://github.com/clash-verge-rev/clash-verge-rev/issues/8510)中，进入包含远程代理组图标的「代理」页面后，侧边栏无法再切换到设置等页面；重启应用后仍可复现。相关日志包含：

```text
WebKitNetworkProcess / WebKitNetworkPr: segfault ... in libsoup-3.0.so.0.0.5
ERROR: WebKit encountered an internal error. This is a WebKit bug.
WebLoaderStrategy::internallyFailedLoadTimerFired()
```

遇到相同症状时，可尝试在设置中关闭「代理组图标」显示，然后重启 Clash Verge Rev。该报告中，关闭图标后，同一订阅下代理页面和侧边栏切换恢复正常。

如果界面已经无法操作：

1. 完全退出 Clash Verge Rev；仅关闭窗口可能仍会在后台运行。退出应用可能短暂中断代理连接。
2. 备份应用目录中的 `verge.yaml`。Linux 默认路径为 `~/.local/share/io.github.clash-verge-rev.clash-verge-rev/verge.yaml`；若使用了自定义应用目录，请以实际路径为准。
3. 将已有的 `enable_group_icon` 改为 `false`，保留其余设置，不要添加重复键：

    ```yaml
    enable_group_icon: false
    ```

4. 重新启动应用，进入「代理」页面并切换到其他页面，确认是否恢复。

此设置只关闭代理组图标显示，不修改订阅、节点或代理规则。需要恢复时，可在设置中重新开启图标显示。

!!! note "适用范围"
    这是上述环境中验证过的规避方法，不是已确认的底层修复；尚未确定具体图片请求或 WebKit/libsoup 缺陷，也未在 AutoBuild 上验证。不要将所有 Linux 界面卡顿都归因于图标加载。

    「代理组图标」与「代理组托盘显示模式」是两个不同设置。后者用于系统托盘菜单，参见 [GNOME 托盘菜单卡死问题](https://github.com/clash-verge-rev/clash-verge-rev/issues/7779)；在本例中，仅禁用托盘菜单不能解决代理页面卡住。

## 启动失败：Error 71 (Protocol error) dispatching to Wayland display. 

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/10702)，可能在 NVIDIA + Wayland 环境出现。可通过设置以下环境变量进行规避：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```

## AcceleratedSurface was unable to construct a complete framebuffer

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/9304)，可能在 NVIDIA + Wayland 环境出现。可通过设置以下环境变量进行规避：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```

## Failed to create GBM buffer of size xxx: Invalid argument

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/13493)，可能在 NVIDIA + Wayland 环境出现。可通过设置以下环境变量进行规避：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```
## UI 未响应 / UI 卡顿 / 崩溃：Failed to initialize GTK

尝试设置以下环境变量：
```
WEBKIT_DISABLE_DMABUF_RENDERER=0
```

## BadWindow (invalid Window parameter)

部分发行版可能出现的 XWayland 兼容层问题，可通过设置以下环境变量进行规避：
```
GDK_BACKEND=x11
```

## Truncating shared memory file failed: Invalid argument

这是一个已知的[上游问题](https://gitlab.gnome.org/GNOME/gtk/-/issues/2504)，可能在高分辨率或开启 HiDPI 缩放的 Wayland 环境下出现。可通过设置以下环境变量进行规避：
```
GDK_BACKEND=x11
```

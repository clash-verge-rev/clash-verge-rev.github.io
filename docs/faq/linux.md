## Ubuntu 20.04 版本无法正常安装

Ubuntu `20.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，但安装过程繁琐，依赖关系复杂，建议升级最新的`24.04`。其他 Debian 系操作系统类似。

## 无 UI 图形化界面能用吗

不能。Clash Verge Rev 是基于 `Webview` 的 GUI 程序，需要图形化界面支持。无图形化界面请下载 [MetaCubeX/Mihomo](https://github.com/MetaCubeX/mihomo/releases/latest) 内核二进制程序，通过命令行的方式使用（参数 `--help` 查看帮助）。

## 窗口标题栏按钮（最小化/最大化/关闭）失效

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/11856)，打开设置 -> 界面设置 -> 关闭「优先使用系统标题栏」。

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
## UI 未响应 / UI 卡顿

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
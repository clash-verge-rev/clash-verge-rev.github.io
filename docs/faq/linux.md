## Ubuntu 20.04 版本无法正常安装

Ubuntu `20.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk` 依赖，但安装过程繁琐，依赖关系复杂，建议升级最新的`24.04`。其他 Debian 系操作系统类似。

## 无 UI 图形化界面能用吗

不能。Clash Verge Rev 是基于 `Webview` 的 GUI 程序，需要图形化界面支持。无图形化界面请下载 [MetaCubeX/Mihomo](https://github.com/MetaCubeX/mihomo/releases/latest) 内核二进制程序，通过命令行的方式使用（参数 `--help` 查看帮助）。

## KDE 窗口标题栏按钮（最小化/最大化/关闭）失效

打开设置 -> 界面设置 -> 关闭「优先使用系统标题栏」。

## 启动失败：Error 71 (Protocol error) dispatching to Wayland display. 

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/10702)。 暂时的解决方法是手动设置环境变量：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```

## AcceleratedSurface was unable to construct a complete framebuffer

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/9304)。暂时的解决方法是手动设置环境变量：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```

## Failed to create GBM buffer of size xxx: Invalid argument

这是一个已知的[上游问题](https://github.com/tauri-apps/tauri/issues/13493)。暂时的解决方法是手动设置环境变量：
```
WEBKIT_DISABLE_DMABUF_RENDERER=1
```
## UI 未响应 / UI 卡顿

尝试手动设置环境变量：
```
WEBKIT_DISABLE_DMABUF_RENDERER=0
```

## BadWindow (invalid Window parameter)

部分发行版可能出现的 XWayland 兼容层问题（Wayland 下经由 XWayland 运行的 GTK / WebKitGTK 在窗口销毁 / 重建时仍访问了已失效的 X11 窗口句柄，触发 BadWindow 崩溃）。暂时的解决方法是手动设置环境变量：
```
GDK_BACKEND=x11
```
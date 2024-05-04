## Verge 无法正常启动

> 提示异常代码: 0xc0000409

修复、卸载、重新安装 [WebView2](https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download)。

## WebView2 无法正常安装

可能是你的 Windows 系统关闭了自动更新，请打开自动更新。

## Ubuntu 24.04 版本无法正常启动

Ubuntu `24.04` 需要额外安装 `libwebkit2gtk` 和 `libjavascriptcoregtk`。

```bash
sudo apt install ./libwebkit2gtk-4.0-37_2.43.3-1_amd64.deb ./libjavascriptcoregtk-4.0-18_2.43.3-1_amd64.deb
```

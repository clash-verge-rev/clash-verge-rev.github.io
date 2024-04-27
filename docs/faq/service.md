## TUN 模式无法开启

- Windows 用户请使用管理员模式运行，或者安装服务模式。
- Mac/Linux 用户在`设置`->`Clash 内核`,点击 ⚙️ 图标，点击"授权"。

## 安装服务模式报错

是旧版已知的兼容性 bug，实际不影响使用（最新版已经修复了）。服务模式的唯一作用就是为了可以不用管理员模式启用 TUN。

## 升级，卸载、重装、开启的时候“服务模式”相关报错

到`内核目录`中找到 `uninstall-service.exe`，执行卸载服务模式程序。

![uninstall-service](../assets/faq/service/uninstall-service.png)

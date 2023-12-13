# 常见问题

1. 报错找不到系统文件 os error
   有可能是文件损坏，没找到内核。解决方法：删除老配置。卸载老版本，重新安装。

2. 安装新版本后之前的配置、订阅不见了
   从 1.4.3 版本开始，我们修改了配置文件路径，建议删除老版本再安装。你可以在软件设置，“应用目录”，找到配置文件路径。

3. tun 模式无法开启
   windows 请使用管理员模式运行，或安装服务模式。mac 请在设置的 clash 内核点开 ⚙️，点"授权"

4. 安装服务模式报错
   是旧版已知的兼容性 bug，实际不影响使用，最新版已经修复了。服务模式的唯一作用就是为了可以不用管理员模式启用 tun。

5. 升级，卸载、重装、开启的时候“服务模式”相关报错
   到软件目录中运行 uninstall-service.exe 卸载服务模式
   ![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/e2b58ae9-3133-4948-9b3b-d0f1a7ad359f)

6. 延迟测试比之前低了（或高了）
   在 clash 字段中，勾选，或取消勾选 unified-delay

7. verge 无法正常启动
   修复、卸载，重新安装 webview2 https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download

8. 有系统图标，主界面点不开
   使用 win7 兼容模式，参考： https://github.com/zzzgydi/clash-verge/issues/717

9. mac 系统显示“文件已损坏”
   原因是 mac 软件包没有经过开发者签名，请运到终端行命令授权：
   `sudo xattr -r -d com.apple.quarantine /Applications/Clash\ Verge.app`

10. 日志过大，占满磁盘
    可以把日志等级设置为 silent 或者 error，并在“杂项设置”中设置自动清理时间。

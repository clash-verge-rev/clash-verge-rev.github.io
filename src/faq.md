1. 报错找不到系统文件os error
有可能是文件损坏，没找到内核。解决方法：删除老配置。卸载老版本，重新安装。

2. 安装新版本后之前的配置、订阅不见了
从1.4.3版本开始，我们修改了配置文件路径，建议删除老版本再安装。你可以在软件设置，“应用目录”，找到配置文件路径。

3. tun模式无法开启
windows请使用管理员模式运行，或安装服务模式。mac/linux请在设置的clash内核点开⚙️，点"授权"

4. 安装服务模式报错
是旧版已知的兼容性bug，实际不影响使用，最新版已经修复了。**服务模式的唯一作用就是为了可以不用管理员模式启用tun。**

5. 升级，卸载、重装、开启的时候“服务模式”相关报错
到软件目录中运行 uninstall-service.exe 卸载服务模式

![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/e2b58ae9-3133-4948-9b3b-d0f1a7ad359f)

6. 延迟测试比之前低了（或高了）
在clash字段中，勾选，或取消勾选unified-delay

7. verge无法正常启动
修复、卸载，重新安装webview2 https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download

8. 有系统图标，主界面点不开
使用win7兼容模式，参考： https://github.com/zzzgydi/clash-verge/issues/717

9. mac系统显示“文件已损坏”
原因是mac软件包没有经过开发者签名，请到终端执行命令授权：
`sudo xattr -r -d com.apple.quarantine /Applications/Clash\ Verge.app`

10. 日志过大，占满磁盘
可以把日志等级设置为silent或者error，并在“杂项设置”中设置自动清理时间。

11. 订阅不显示总流量（总流量显示为0）
32位版本下的已知bug，谁还用32位啊，请下载64位。

12. 我应该下载什么版本？
看项目首页Install章节。

---

其他奇怪异常：

- 无法选中订阅，日志报错 An attempt was made to access a socket in a way forbidden by its access permissions. 
系统服务没有开启：
```
net stop hns
net start hns
```
![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/47783265-4d3f-414d-b28a-a4fecd9079bf)

- Webview2无法正常安装
可能是你的Windows关闭了自动更新，请打开自动更新。

- 导入订阅报错：

![image_2023-12-15_11-51-02](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/8ba1e0c8-a711-4b78-97db-bc947e25f499)

卸载重装。
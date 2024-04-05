### 我应该下载什么版本？

- Windows: x64-setup.exe
- Windows Arm芯片: arm64-setup.exe
- MacOS Intel芯片: x64.dmg
- MacOS Apple M芯片: aarch64.dmg
- Linux x64架构: amd64.AppImage/amd64.deb
- Windows便携板: x64_portable.zip （不推荐使用，无法自动更新）

### 报错找不到系统文件os error

有可能是文件损坏，没找到内核。解决方法：删除老配置。卸载老版本，重新安装。

### 安装新版本后之前的配置、订阅不见了

从1.4.3版本开始，我们修改了配置文件路径，建议删除老版本再安装。你可以在软件设置，“应用目录”，找到配置文件路径。

### tun模式无法开启

windows请使用管理员模式运行，或安装服务模式。mac/linux请在设置的clash内核点开⚙️，点"授权"

### 安装服务模式报错

是旧版已知的兼容性bug，实际不影响使用，最新版已经修复了。**服务模式的唯一作用就是为了可以不用管理员模式启用tun。**

### 升级，卸载、重装、开启的时候“服务模式”相关报错

到软件目录中运行 uninstall-service.exe 卸载服务模式

![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/e2b58ae9-3133-4948-9b3b-d0f1a7ad359f)

### 延迟测试比之前低了（或高了）(已丢弃字段设置)

在clash字段中，勾选，或取消勾选unified-delay

### verge无法正常启动

修复、卸载，重新安装webview2 https://developer.microsoft.com/zh-cn/microsoft-edge/webview2/#download

### 有系统图标，主界面点不开

使用win7兼容模式，参考： https://github.com/zzzgydi/clash-verge/issues/717

### mac系统显示“文件已损坏”

原因是开发者没有Apple Developer Program会员资格，请到终端执行命令授权：

`sudo xattr -r -d com.apple.quarantine /Applications/Clash\ Verge.app`

![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/4974387f-7001-43ce-9e3e-a9a820e62a66)

### mac系统打开软件页面空白

请升级macos到11或以上版本，不支持macos 10。

### 修改GUI日志显示级别，用于debug

- 修改方式1：打开软件菜单“设置”->杂项设置->App日志级别->TRACE，重启软件生效。
- 修改方式2（当软件无法启动时）: 
Winddows配置文件目录：C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\verge.yaml，文本编辑器编辑verge.yaml文件
把app_log_level: null修改为app_log_level: trace （注意：请在关闭软件的情况下修改日志级别，启动软件生效）
- Windows log目录：
C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\log

### 日志过大，占满磁盘

可以把日志等级设置为silent或者error，并在“杂项设置”中设置app日志的自动清理时间。

### 订阅不显示总流量（总流量显示为0）

32位1.4.11及以前版本下的已知bug，请下载更新最新版。

---

## 其他奇怪异常：

### 无法选中订阅，日志报错 An attempt was made to access a socket in a way forbidden by its access permissions. 

系统服务没有开启：
```
net stop hns
net start hns
```
![image](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/47783265-4d3f-414d-b28a-a4fecd9079bf)

### Webview2无法正常安装

可能是你的Windows关闭了自动更新，请打开自动更新。

### 导入订阅报错：

![image_2023-12-15_11-51-02](https://github.com/clash-verge-rev/clash-verge-rev/assets/96291150/8ba1e0c8-a711-4b78-97db-bc947e25f499)

卸载重装。

### proxy-providers里的机场订阅,如果订阅内有HY2节点，如何获取
配置文件添加：
```global-ua: clash-verge/v1.5.11```
# 卸载软件

## windows下卸载删除service的方法
 方法 1. 在软件设置菜单，虚拟网卡旁边点删除图标
 方法 2. 手动删除：打开软件安装目录，进入 resource 文件夹，使用 cmd 运行 uninstall-service.exe

## macOS下卸载删除service的方法

1. 停止服务：
   ```bash
   sudo launchctl stop io.github.clash-verge-rev.clash-verge-rev.service
   sudo launchctl unload /Library/LaunchDaemons/io.github.clash-verge-rev.clash-verge-rev.service.plist
   ```

2. 删除tun服务的文件夹：
   ```bash
   sudo rm -rf /Library/PrivilegedHelperTools/io.github.clash-verge-rev.clash-verge-rev.service.bundle
   ```

   完成以上步骤后，tun服务将停止运行，并且在进程中将不再可见。为了确保后续安装其他版本时不会出现问题，请务必删除上述tun服务的文件夹。 

## Linux下卸载删除service的方法

 方法 1. 在软件设置菜单，虚拟网卡旁边点删除图标
 方法 2. 手动删除：命令行执行 `sudo /bin/uninstall-service` （当服务出问题时候，可以删除后再手动安装 `sudo /bin/install-service` ）

## 相关文件及目录

### Windows
* 窗口状态文件（记录窗口大小位置等，窗口出问题请删除此文件）：`C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\.window-state.json`
* Clash Verge配置文件：`C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\verge.yaml`
* 配置(软件工作)目录：`C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\`
* 订阅(配置)文件目录：`C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\profiles\`

### MacOS
* 窗口状态文件：`/Users/你的用户名/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/.window-state.json`
* Clash Verge配置文件：`/Users/你的用户名/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/verge.yaml`
* 配置(软件工作)目录：`/Users/你的用户名/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/`
* 订阅(配置)文件目录：`/Users/你的用户名/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/`
### Linux

* 窗口状态文件：`~/.config/io.github.clash-verge-rev.clash-verge-rev/.window-state.json`
* Clash Verge配置文件：`~/.config/io.github.clash-verge-rev.clash-verge-rev/verge.yaml`
* 配置(软件工作)目录：`~/.local/share/io.github.clash-verge-rev.clash-verge-rev/`
* 订阅(配置)文件目录：`~/.local/share/io.github.clash-verge-rev.clash-verge-rev/profiles/`
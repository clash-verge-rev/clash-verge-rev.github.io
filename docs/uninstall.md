# 卸载软件

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
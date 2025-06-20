# 卸载软件

## 📋 卸载说明

### 1. Windows 下卸载删除 service 的方法
- **方法 1**：在软件设置菜单，虚拟网卡旁边点删除图标
- **方法 2**：手动删除：打开软件安装目录，进入 resource 文件夹，使用 cmd 运行 uninstall-service.exe

### 2. macOS 下卸载删除 service 的方法

**步骤 1：停止服务**
```bash
sudo launchctl stop io.github.clash-verge-rev.clash-verge-rev.service
sudo launchctl unload /Library/LaunchDaemons/io.github.clash-verge-rev.clash-verge-rev.service.plist
```

**步骤 2：删除 tun 服务的文件夹**
```bash
sudo rm -rf /Library/PrivilegedHelperTools/io.github.clash-verge-rev.clash-verge-rev.service.bundle
```

> ⚠️ **重要提示**：完成以上步骤后，tun 服务将停止运行，并且在进程中将不再可见。为了确保后续安装其他版本时不会出现问题，请务必删除上述 tun 服务的文件夹。

### 3. Linux 下卸载删除 service 的方法
- **方法 1**：在软件设置菜单，虚拟网卡旁边点删除图标
- **方法 2**：手动删除：命令行执行 `sudo /bin/uninstall-service` （当服务出问题时候，可以删除后再手动安装 `sudo /bin/install-service`）

---

## 📁 相关文件及目录

### 🪟 Windows

| 文件类型 | 路径 | 说明 |
|---------|------|------|
| **WebView 缓存目录** | `C:\Users\你的用户名\AppData\Local\io.github.clash-verge-rev.clash-verge-rev` | ⚠️ 强烈建议每次升级后清空此目录 |
| **窗口状态文件** | `C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\.window-state.json` | 记录窗口大小位置等，窗口出问题请删除此文件 |
| **Clash Verge 配置文件** | `C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\verge.yaml` | 主配置文件 |
| **配置(软件工作)目录** | `C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\` | 软件工作目录 |
| **订阅(配置)文件目录** | `C:\Users\你的用户名\AppData\Roaming\io.github.clash-verge-rev.clash-verge-rev\profiles\` | 存放订阅配置文件 |

### 🍎 macOS

| 文件类型 | 路径 | 说明 |
|---------|------|------|
| **WebView 缓存目录** | `~/Library/Caches/io.github.clash-verge-rev.clash-verge-rev` | ⚠️ 强烈建议每次升级后清空此目录 |
| **窗口状态文件** | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/.window-state.json` | 记录窗口状态信息 |
| **Clash Verge 配置文件** | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/verge.yaml` | 主配置文件 |
| **配置(软件工作)目录** | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/` | 软件工作目录 |
| **订阅(配置)文件目录** | `~/Library/Application Support/io.github.clash-verge-rev.clash-verge-rev/profiles/` | 存放订阅配置文件 |

### 🐧 Linux

| 文件类型 | 路径 | 说明 |
|---------|------|------|
| **WebView 缓存目录** | `~/.cache/io.github.clash-verge-rev.clash-verge-rev` | ⚠️ 强烈建议每次升级后清空此目录 |
| **窗口状态文件** | `~/.config/io.github.clash-verge-rev.clash-verge-rev/.window-state.json` | 记录窗口状态信息 |
| **Clash Verge 配置文件** | `~/.config/io.github.clash-verge-rev.clash-verge-rev/verge.yaml` | 主配置文件 |
| **配置(软件工作)目录** | `~/.local/share/io.github.clash-verge-rev.clash-verge-rev/` | 软件工作目录 |
| **订阅(配置)文件目录** | `~/.local/share/io.github.clash-verge-rev.clash-verge-rev/profiles/` | 存放订阅配置文件 |

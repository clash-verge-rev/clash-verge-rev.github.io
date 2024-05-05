## mac 系统显示“文件已损坏”

> macOS 系统 10.12 版本后对来自非 Mac App Store 中的应用做了限制。

![文件已损坏](../assets/faq/file/mac_file_corrupted.png)

- 问题原因: 开发者没有 **Apple Developer Program** 会员资格。
- 解决方案: 请打开终端并执行下列命令进行授权。

<!-- prettier-ignore -->
!!! warning
    macOS `14.5` 版本移除了 `-r` 参数（提示 `option -r not recognized` ），请根据你的系统版本自行将命令中 `-r` 参数删除后再执行。

```bash
sudo xattr -r -d com.apple.quarantine /Applications/Clash\ Verge.app
```

## 找不到系统文件 os error

问题原因:

1. 内核文件损坏，没找到内核。
2. 内核文件被杀毒软件删除或添加到了隔离列表。

解决方案:

1. 如果文件损坏，删除老配置，卸载老版本，重新安装。
2. 如果被杀毒软件误伤，请将文件手动恢复并添加到白名单。

## 日志过大，占满磁盘

解决方案: 可以将日志等级设置为 `Silent` 或 `Error`，并在 `杂项设置` 中设置自动清理日志间隔。

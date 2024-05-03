## mac 系统显示“文件已损坏”

![文件已损坏](../assets/faq/file/mac_file_corrupted.png)

- 问题原因: 开发者没有 **Apple Developer Program** 会员资格。
- 解决方案: 请打开终端并执行下列命令进行授权。

```bash
sudo xattr -r -d com.apple.quarantine /Applications/Clash\ Verge.app
```

## 找不到系统文件 os error

- 问题原因: 有可能是文件损坏，没找到内核。
- 解决方案: 删除老配置。卸载老版本（确保`应用目录`删除干净），重新安装。

## 日志过大，占满磁盘

解决方案: 可以将日志等级设置为 `Silent` 或 `Error`，并在 `杂项设置` 中设置自动清理日志间隔。

## 修改 GUI 日志显示级别，用于 debug

打开软件 clash verge 菜单“设置”-> 拉倒最下面点开“应用目录”，文本编辑器编辑 verge.yaml 文件，把 app_log_level: 修改为 app_log_level: debug

## proxy-providers 里的机场订阅中的 HY2 节点

配置文件添加（版本号不重要，含有 `clash` 字样即可）。

```yaml
global-ua: clash-verge/v1.6.0
```

## 在命令行中使用代理

在设置页的**复制环境变量类型**选项中选择要使用的命令行类型（`PowerShell`，`Bash`，`CMD`），然后右键屏幕右下方系统托盘里的小图标，选择 `复制环境变量` 即可复制对应的命令行环境变量。然后在命令行中执行对应的命令即可使用代理。

## Linux 无法显示界面/闪退

添加 `WEBKIT_DISABLE_COMPOSITING_MODE=1` 环境变量

## Linux 花屏

添加 `WEBKIT_DISABLE_COMPOSITING_MODE=1` 环境变量

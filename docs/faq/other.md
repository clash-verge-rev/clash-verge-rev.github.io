## proxy-providers 里的机场订阅中的 HY2 节点

配置文件添加（版本号不重要，含有 `clash-verge` 字样即可）。

```yaml
global-ua: clash-verge/v1.6.0
```

## 在命令行中使用代理

在设置页的**复制环境变量类型**选项中选择要使用的命令行类型（`PowerShell`，`Bash`，`CMD`），然后右键屏幕右下方系统托盘里的小图标，选择 `复制环境变量` 即可复制对应的命令行环境变量。然后在命令行中执行对应的命令即可使用代理。

## 无法访问公司内网域名

> 无法访问公司内网的域名，而 IP 可以正常访问。

- 问题原因: 配置中启用了内核的 `DNS` 模块，却未正确配置 DNS 服务器，导致无法解析内网的主机名/域名（由于内网主机名/域名不公开，即便根域名服务器也无法查到记录）。
- 解决办法: 修改配置文件，添加 `nameserver-policy`配置，为内网域名指定 DNS 服务器（一般为内网网关）。

假设你的 IP 为 `10.10.10.123` ，网关为 `10.10.10.1`，要访问的域名为 `www.helloworld.com`。

```yaml
dns:
  nameserver-policy:
    '+.helloworld.com': '10.10.10.1'
```

## GPU 异常占用高

- 问题原因: 暂时未知。
- 解决办法: `设置` -> `界面设置` -> 关闭 `流量图显`。

## 代理界面异常，不显示任何内容

> 代理界面一片空白，不显示任何节点信息，而代理运行正常。

日志中提示: `External controller listen error: listen tcp 127.0.0.1:9097: bind: An attempt was made to access a socket in a way forbidden by its access permissions.`

- 问题原因: 外部控制端口被其他程序占用
- 解决办法: `Clash设置` -> `外部控制` -> 修改 `外部控制监听地址`中的端口并保存，然后退出并重启程序。

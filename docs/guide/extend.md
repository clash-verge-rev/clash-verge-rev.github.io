# 扩展配置与脚本

<!-- prettier-ignore -->
!!! danger "Changelog"
    - `v1.7.x` 版本的 `Merge配置` 改名为 `扩展配置` ，且prepend/append功能移动至订阅右键菜单中的可视化编辑器中实现（例如：prepend-rules移动至订阅右键菜单的 `编辑规则` 中的 `prepend`）。扩展配置仅用于配置项覆写/合并。
    - `v1.7.x` 版本的 `Script配置` 改名为 `扩展脚本` 。
    - `v1.6.x` 版本请参考 [Script配置](./script.md)。

## 扩展分类

- 根据文件类型，扩展可分为 `配置` 和 `脚本` 。`配置` 使用 `yaml` 对配置进行覆写/合并， `脚本` 使用 `javascript` 对配置进行更加灵活的调整。
- 根据作用范围，扩展可分为 `全局扩展` 和 `订阅扩展` 。全局扩展适用于设置大多数订阅均会生效的项，需要特殊处理的订阅使用订阅扩展进行单独调整。
- **这些扩展会同时生效**，按照以下执行链路的顺序执行，对配置文件进行修改。应用设置在扩展之前写入，并在扩展之后把由它接管的字段写回，详见下文「与应用设置的优先级」。

```mermaid
flowchart LR
  S["订阅原文"] --> G["应用设置"] --> A["全局扩展配置"] --> B["全局扩展脚本"] --> C["订阅扩展配置"] --> D["订阅扩展脚本"] --> E["应用设置回写"]

```

## 与应用设置的优先级

设置页里的选项优先级高于订阅原文、扩展配置和扩展脚本。扩展可以覆盖订阅里的任何字段，但下列字段由应用接管，扩展写入的值会在链路末尾被丢弃，应用会弹出提示，并在该扩展卡片的日志中记录：

- 顶层字段：`external-controller`、`external-controller-cors`、`secret`、`mixed-port`、`socks-port`、`port`、`redir-port`、`tproxy-port`、`mode`、`allow-lan`、`log-level`、`ipv6`、`unified-delay`。
- `tun.enable`，以及在 TUN 设置对话框中保存过的字段：`stack`、`device`、`auto-route`、`route-exclude-address`、`auto-redirect`、`auto-detect-interface`、`dns-hijack`、`strict-route`、`mtu`。
- `dns.ipv6`：仅在「DNS 覆写」开启且覆写页面启用 IPv6 时接管。

「DNS 覆写」本身只在扩展之前写入：开启时，覆写页面中填写了值的 `dns` 字段和 `hosts` 会替换订阅里的对应字段；关闭时（包括因订阅自带 DNS 策略而自动关闭）应用不改动 `dns`。两种情况下扩展写入的 `dns` 字段都照常生效，`dns.ipv6` 除外。

## 扩展教程

右键配置卡片，选择编辑扩展/脚本，即可进入编辑器。

### 扩展配置

假设有三个订阅文件，配置片段如下。

```yaml
# 订阅A
dns:
  ipv6: true

# 订阅B
tcp-concurrent: false
dns:
  ipv6: false

# 订阅C
dns:
  enable: false
find-process-mode: strict

```

现如有如下需求：

- 所有订阅均启用 DNS 。
- 所有订阅均启用 `tcp-concurrent` 。
- 订阅 A 进程匹配模式修改为 `always` 。
- 除了订阅 C 外， DNS 均启用 `ipv6` 。

我们可以将如下配置添加到全局扩展配置中。分别对各配置生效。

```yaml
dns:
  enable: true
  ipv6: true
tcp-concurrent: true
```

同时，将如下配置添加到订阅 A 的扩展配置中。

```yaml
find-process-mode: always
```

同时，将如下配置添加到订阅 C 的扩展配置中。

```yaml
dns:
  ipv6: false
```

### 扩展脚本

扩展脚本较 `v1.6.x` 无变动，可参考[Script 配置](./script.md)。

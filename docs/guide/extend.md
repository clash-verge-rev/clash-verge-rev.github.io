<!-- prettier-ignore -->
!!! danger "Changelog"
    - `v1.7.x` 版本的 `Merge配置` 改名为 `扩展配置` ，且prepend/append功能移动至订阅右键菜单中的可视化编辑器中实现（例如：prepend-rules移动至订阅右键菜单的 `编辑规则` 中的 `prepend`）。扩展配置仅用于配置项覆写/合并。
    - `v1.7.x` 版本的 `Script配置` 改名为 `扩展脚本` 。
    - `v1.6.x` 版本请参考 [Merge配置](./merge.md) 和 [Script配置](./script.md)。

## 扩展分类

- 根据文件类型，扩展可分为 `配置` 和 `脚本` 。`配置` 使用 `yaml` 对配置进行覆写/合并， `脚本` 使用 `javascript` 对配置进行更加灵活的调整。
- 根据作用范围，扩展可分为 `全局扩展` 和 `订阅扩展` 。全局扩展适用于设置大多数订阅均会生效的项，需要特殊处理的订阅使用订阅扩展进行单独调整。
- **这些扩展会同时生效**，按照以下执行链路的顺序执行，对配置文件进行修改。

```mermaid
flowchart LR
  A["全局扩展配置"] --> B["全局扩展脚本"] --> C["订阅扩展配置"] --> D["订阅扩展脚本"]

```

## 扩展教程

右键配置卡片，选择编辑扩展/脚本，即可进入编辑器。

### 扩展配置

假设有三个订阅文件，配置片段如下。

```yaml
# 订阅A
dns:
  ipv6: true

# 订阅B
unified-delay: true
dns:
  ipv6: false

# 订阅C
dns:
  enable: false
find-process-mode: strict

```

现如有如下需求：

- 所有订阅均启用 DNS 。
- 所有订阅均禁用 `unified-delay` 。
- 订阅 A 进程匹配模式修改为 `always` 。
- 除了订阅 C 外， DNS 均启用 `ipv6` 。

我们可以将如下配置添加到全局扩展配置中。分别对各配置生效。

```yaml
dns:
  enable: true
  ipv6: true
unified-delay: false
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

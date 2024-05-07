## 新增配置

<!-- prettier-ignore -->
!!! warning
    - 如果创建了多个 Merge 配置，按照启用顺序先后，链式执行。
    - 配置修改后需要重新启用，生效时卡片有颜色标识（右键配置 `禁用` 再 `启用` ，也可以点击右上角的 🔥 按钮）。

<!-- prettier-ignore -->
!!! info
    如果图片太小看不清，请右键图片在新标签页中打开。如果图片文件较大，请耐心等待。

![新增脚本](../assets/guide/merge/merge.gif)

## 配置说明

<!-- prettier-ignore -->
!!! info
    - 配置语法请参考 [配置语法文档](https://wiki.metacubex.one/config/syntax/#_5)。
    - 规则配置请参考 [规则配置文档](https://wiki.metacubex.one/config/rules/)。

<!-- prettier-ignore -->
!!! warning
    - 如果创建了多个 Merge 配置，按照启用顺序先后，链式执行。
    - 配置修改后需要重新启用，生效时卡片有颜色标识（右键配置 `禁用` 再 `启用` ，也可以点击右上角的 🔥 按钮）。

<!-- prettier-ignore -->
!!! failure
    - `v1.6.2` 版本移除了 `prepend-rule-providers`、`prepend-proxy-providers`、`append-rule-providers`、`append-proxy-providers`。
    - 请使用 `rule-providers`、`proxy-providers` 代替（效果等价于 `append-rule-providers`、`append-proxy-providers`）。

```yaml
# 前置规则
prepend-rules: []
# 前置规则集(v1.6.2已移除)
# prepend-rule-providers: {}
# 前置代理
prepend-proxies: []
# 前置代理集(v1.6.2已移除)
# prepend-proxy-providers: {}
# 前置代理组
prepend-proxy-groups: []
# 后置规则
append-rules: []
# 后置规则集(v1.6.2已移除，使用rule-providers代替)
# append-rule-providers: {}
# 后置代理
append-proxies: []
# 后置代理集(v1.6.2已移除，使用proxy-providers代替)
# append-proxy-providers: {}
# 后置代理组
append-proxy-groups: []

# 覆盖原配置(见示例)
```

<!-- prettier-ignore -->
!!! tip
    - 配置除了可以往原配置中添加配置外，还可以覆盖原配置文件中的配置项。
    - 需要**覆盖的配置项**和**原配置文件**中的书写方式一样。

## 配置示例

### 自定义规则

<!-- prettier-ignore -->
!!! warning
    配置规则一般使用的是 `prepend-rules` 而非 `append-rules`（使用 `append-rules` 插入到原配置中的 `MATCH` 规则后会导致插入的规则无效）。

例如:

- 网站 `www.baidu.com`， 走节点 `🇯🇵6日本-东部优化(hy2)` 。
- 网站 `www.google.com`， 走节点 `🇰🇷9韩国-全网优化(hy2)` 。
- 网站 `www.youtube.com`， 走策略组 `♻️自动选择` 。
- 网段 `10.11.12.0/24`，走直连策略组 `DIRECT` 。

```yaml
prepend-rules:
  - DOMAIN-SUFFIX,baidu.com,🇯🇵6日本-东部优化(hy2)
  - DOMAIN-SUFFIX,google.com,🇰🇷9韩国-全网优化(hy2)
  - DOMAIN-SUFFIX,youtube.com,♻️自动选择
  - IP-CIDR,10.11.12.0/24,DIRECT,no-resolve

prepend-proxies: []

prepend-proxy-groups: []

append-rules: []

append-proxies: []

append-proxy-groups: []
```

### 覆盖原配置

<!-- prettier-ignore -->
!!! warning
    - 由 Clash Verge 进行覆写的配置无法被覆写成功。程序需要保证这部分配置受程序控制，以此保证程序功能正常可用（如`mixed-port`、`log-level`、`external-controller`等）。
    - 配置项未配置进去的其他部分配置，会保持原配置不变（**要求 Clash Verge Rev 版本至少为 `v1.6.2`** 。`v1.6.2` 之前的版本，配置项未配置进去的其他部分配置，将会被视为空并覆盖）。

例如: 假设原配置文件中 DNS 启用了 `ipv6` ，现在需要禁用该功能。

```yaml
prepend-rules: []

prepend-proxies: []

prepend-proxy-groups: []

append-rules: []

append-proxies: []

append-proxy-groups: []

# 覆盖DNS配置的ipv6配置
# DNS其余配置将不变(v1.6.2版本及以后)
# DNS其余配置将清空(v1.6.2版本之前)
dns:
  ipv6: false
```

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
    - 配置规则一般使用的是 `prepend-rules` 而非 `append-rules`（插入到原配置中的 `MATCH` 规则后面导致无效）。
    - 配置语法请参考 [配置语法文档](https://wiki.metacubex.one/config/syntax/#_5)。
    - 规则配置请参考 [规则配置文档](https://wiki.metacubex.one/config/rules/)。

```yaml
# 前置规则
prepend-rules: []
# 前置规则集
prepend-rule-providers: {}
# 前置代理
prepend-proxies: []
# 前置代理集
prepend-proxy-providers: {}
# 前置代理组
prepend-proxy-groups: []
# 后置规则
append-rules: []
# 后置规则集
append-rule-providers: {}
# 后置代理
append-proxies: []
# 后置代理集
append-proxy-providers: {}
# 后置代理组
append-proxy-groups: []

```

## 配置示例

- 让 `www.baidu.com` 走`日本节点` 。
- 让 `www.google.com`，走 `韩国节点` 。

```yaml
prepend-rules:
  - DOMAIN-SUFFIX,google.com,🇯🇵6日本-东部优化(hy2)
  - DOMAIN-SUFFIX,baidu.com,🇰🇷9韩国-全网优化(hy2)
```

## 新增配置

<!-- prettier-ignore -->
!!! warning
    - 如果创建了多个 Merge 配置，按照启用顺序先后，**链式执行**。
    - **<font color="red">配置修改后需要重新启用</font>**，生效时卡片有颜色标识（右键配置 `禁用` 再 `启用` ，也可以点击右上角的 🔥 按钮）。

<video controls>
  <source src="../assets/guide/merge/merge.webm">
</video>

## 配置说明

<!-- prettier-ignore -->
!!! info
    - 配置语法请参考 [配置语法文档](https://wiki.metacubex.one/config/syntax/#_5)（**<font color="red">不懂配置文件语法必看！！！</font>**）。
    - 规则配置请参考 [规则配置文档](https://wiki.metacubex.one/config/rules/)（**<font color="green">不懂该用哪个规则必看！！！</font>**）。

<!-- prettier-ignore -->
!!! danger "Changelog"
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

## 配置用法

### 自定义规则

<!-- prettier-ignore -->
!!! warning
    - 规则的匹配顺序是**从上到下依次进行匹配**，匹配成功会提前结束匹配。
    - **`MATCH` 规则一定会匹配成功 **，因此配置文件的规则一般均以** `MATCH` 规则结尾**。
    - 基于上述，配置自定义规则**一般使用的是 `prepend-rules`** 而非 `append-rules`（使用 `append-rules` **会插入规则到原配置中的 `MATCH` 规则后，会导致插入的规则无效**）。
    - 基于上述，**使用 `prepend-rules` 一般不会插入 `MATCH` 规则**（使用 `prepend-rules` **插入 `MATCH` 规则到原配置规则前，会导致原配置的规则无效**）。

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

### 覆盖/合并原配置

<!-- prettier-ignore -->
!!! warning
    - 由 Clash Verge Rev 进行覆写的配置无法被覆写成功。程序需要保证这部分配置受程序控制，以此保证程序功能正常可用（如`mixed-port`、`log-level`、`external-controller`、TUN 模式强制 `dns.enable = true` 等）。
    - 需要**覆盖的配置项**和**原配置文件**中的书写方式一样。

<!-- prettier-ignore -->
!!! danger "Changelog"
    - 被覆写的配置中的未配置的**其他配置项**，会**保持原配置不变**（ `v1.6.2` 版本以上）。
    - 被覆写的配置中的未配置的**其他配置项**，会**视为没有被配置**（ `v1.6.2` 版本之前）。

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

## 配置示例

### Loyalsoldier 规则集

<video controls>
  <source src="../assets/guide/merge/merge_loyalsoldier.webm">
</video>

<!-- prettier-ignore -->
!!! warning
    - 自行将下方配置中的**代理出口(自行修改)**,替换成原配置中**存在的**代理名称或代理组名称（如🚀节点选择、♻️自动选择、:flag_us: 美国节点之类的）。
    - 如果你的 Clash Verge Rev 版本低于 `v1.6.2` ，本配置会**覆盖**原配置中的 `rule-providers` ，请知悉。
    - 首次使用需要下载规则集文件，可能需要加载一段时间，请耐心等待。若等待过久可尝试多次重新激活配置，下载未下载完成的规则集文件。

```yaml
prepend-rules:
  - RULE-SET,applications,DIRECT
  - DOMAIN,clash.razord.top,DIRECT
  - DOMAIN,yacd.haishan.me,DIRECT
  - RULE-SET,private,DIRECT
  - RULE-SET,reject,REJECT
  - RULE-SET,icloud,DIRECT
  - RULE-SET,apple,DIRECT
  - RULE-SET,google,代理出口(自行修改)
  - RULE-SET,proxy,代理出口(自行修改)
  - RULE-SET,direct,DIRECT
  - RULE-SET,lancidr,DIRECT,no-resolve
  - RULE-SET,cncidr,DIRECT,no-resolve
  - RULE-SET,telegramcidr,代理出口(自行修改),no-resolve
  - GEOIP,LAN,DIRECT,no-resolve
  - GEOIP,CN,DIRECT,no-resolve

rule-providers:
  reject:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt"
    path: ./ruleset/loyalsoldier/reject.yaml
    interval: 86400
  icloud:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/icloud.txt"
    path: ./ruleset/loyalsoldier/icloud.yaml
    interval: 86400
  apple:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/apple.txt"
    path: ./ruleset/loyalsoldier/apple.yaml
    interval: 86400
  google:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/google.txt"
    path: ./ruleset/loyalsoldier/google.yaml
    interval: 86400
  proxy:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt"
    path: ./ruleset/loyalsoldier/proxy.yaml
    interval: 86400
  direct:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt"
    path: ./ruleset/loyalsoldier/direct.yaml
    interval: 86400
  private:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/private.txt"
    path: ./ruleset/loyalsoldier/private.yaml
    interval: 86400
  gfw:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt"
    path: ./ruleset/loyalsoldier/gfw.yaml
    interval: 86400
  tld-not-cn:
    type: http
    behavior: domain
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/tld-not-cn.txt"
    path: ./ruleset/loyalsoldier/tld-not-cn.yaml
    interval: 86400
  telegramcidr:
    type: http
    behavior: ipcidr
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/telegramcidr.txt"
    path: ./ruleset/loyalsoldier/telegramcidr.yaml
    interval: 86400
  cncidr:
    type: http
    behavior: ipcidr
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt"
    path: ./ruleset/loyalsoldier/cncidr.yaml
    interval: 86400
  lancidr:
    type: http
    behavior: ipcidr
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt"
    path: ./ruleset/loyalsoldier/lancidr.yaml
    interval: 86400
  applications:
    type: http
    behavior: classical
    url: "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt"
    path: ./ruleset/loyalsoldier/applications.yaml
    interval: 86400
```

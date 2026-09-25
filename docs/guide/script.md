<!-- prettier-ignore -->

# 自定义规则脚本

不知道规则类型? -> [Clash Mihomo路由规则文档](https://wiki.metacubex.one/config/rules)。

不会写JavaScript? -> [菜鸟教程](https://www.runoob.com/js/js-tutorial.html)。

想要更多资料? -> [Script配置](./script.md)

## 通过全局扩展脚本

**原理**：ClashVegerRev通过暴露出可编程的API，即 `config` 对象与 `profileName`
对象，可通过 `main` 函数传入config参数来编辑配置对象。

```javascript

/**
 * 配置中的规则"config.rules"是一个数组，通过新旧数组合并来添加
 * @param prependRule 添加的数组
 */
const prependRule = [
  // 将百度分流到直连
  "DOMAIN-SUFFIX,baidu.com,DIRECT",
  // 将本网站分流到自动选择(前提是你的代理组当中有"自动选择")
  "DOMAIN-SUFFIX,clashverge.dev,自动选择",
];
function main(config) {
  // 把旧规则合并到新规则后面(也可以用其它合并数组的办法)
  let oldrules = config["rules"];
  config["rules"] = prependRule.concat(oldrules);
  return config;
}

```

还可以参考这个issue中讨论的做法-> [issues/1437#issuecomment-2395050752](https://github.com/clash-verge-rev/clash-verge-rev/issues/1437#issuecomment-2395050752)

## 为不同配置文件启用不同的脚本

```javascript

function main(config, profileName) {
    // 设订阅A
  if(profileName === "A") {
    // 对config修改
    // ......
  }
  // 不是“A”则返回未修改的配置
  return config;
}

```


## 脚本说明

- 脚本语言为 `javascript`， 使用 `boa_engine` 作为执行器。
- 程序的入口为 `main` 函数，接受一个 `config` 参数（参数名不限制），并返回修改后的该参数。
- `config` 为 `object` 类型，属性值为 `yaml` 配置文件内容转 `JSON` 后对应的 `object` 对象。

<!-- prettier-ignore -->
!!! danger "Changelog"
    `v1.7.3` 版本新增 `string` 类型参数 `profileName` （参数名不限制），属性值为当前处理的配置文件名。

```javascript
function main(config, profileName) {
  return config;
}
```

### 支持的 API

> 作用域支持以下 API，不支持网络 IO、文件 IO 操作。

| API 类型                 | API 范围                                                                                                                                                                                                                                |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 常量                     | Infinity、NaN、undefined                                                                                                                                                                                                                |
| 基本函数和对象           | Function、Object、Math、JSON、Array、Proxy                                                                                                                                                                                              |
| 数据类型和数组           | ArrayBuffer、SharedArrayBuffer、BigInt、Boolean、Date、DataView、Map、TypedArray、Int8Array、Uint8Array、Uint8ClampedArray、Int16Array、Uint16Array、Int32Array、Uint32Array、BigInt64Array、BigUint64Array、Float32Array、Float64Array |
| 字符串、正则表达式和符号 | String、RegExp、Symbol                                                                                                                                                                                                                  |
| 错误对象                 | Error、RangeError、ReferenceError、TypeError、SyntaxError、EvalError、URIError、AggregateError                                                                                                                                          |
| 反射和异步               | Reflect、Promise                                                                                                                                                                                                                        |
| 编码和解码               | encodeURI、encodeURIComponent、decodeURI、decodeURIComponent                                                                                                                                                                            |
| 弱引用                   | WeakRef、WeakMap、WeakSet                                                                                                                                                                                                               |
| 原子操作                 | Atomics                                                                                                                                                                                                                                 |
| 控制台输出               | console                                                                                                                                                                                                                                 |

## 脚本示例

### 1. 自定义规则(集)

> 如下是一个套用规则集的 Script 脚本示例（可点击复制按钮）。

<!-- prettier-ignore -->
!!! info
    - 你可以在脚本中的 `// 自定义规则` 注释部分的下面，添加自定义的规则。
    - 规则配置请参考 [规则配置文档](https://wiki.metacubex.one/config/rules/)（**<font color="green">不懂该用哪个规则必看！！！</font>**）。

```javascript
// 国内DNS服务器
const domesticNameservers = [
  "https://dns.alidns.com/dns-query", // 阿里云公共DNS
  "https://doh.pub/dns-query", // 腾讯DNSPod
  "https://doh.360.cn/dns-query" // 360安全DNS
];
// 国外DNS服务器
const foreignNameservers = [
  "https://1.1.1.1/dns-query", // Cloudflare(主)
  "https://1.0.0.1/dns-query", // Cloudflare(备)
  "https://208.67.222.222/dns-query", // OpenDNS(主)
  "https://208.67.220.220/dns-query", // OpenDNS(备)
  "https://194.242.2.2/dns-query", // Mullvad(主)
  "https://194.242.2.3/dns-query" // Mullvad(备)
];
// DNS配置
const dnsConfig = {
  "enable": true,
  "listen": "0.0.0.0:1053",
  "ipv6": true,
  "use-system-hosts": false,
  "cache-algorithm": "arc",
  "enhanced-mode": "fake-ip",
  "fake-ip-range": "198.18.0.1/16",
  "fake-ip-filter": [
    // 本地主机/设备
    "+.lan",
    "+.local",
    // Windows网络出现小地球图标
    "+.msftconnecttest.com",
    "+.msftncsi.com",
    // QQ快速登录检测失败
    "localhost.ptlogin2.qq.com",
    "localhost.sec.qq.com",
    // 微信快速登录检测失败
    "localhost.work.weixin.qq.com"
  ],
  "default-nameserver": ["223.5.5.5", "119.29.29.29", "1.1.1.1", "8.8.8.8"],
  "nameserver": [...domesticNameservers, ...foreignNameservers],
  "proxy-server-nameserver": [...domesticNameservers, ...foreignNameservers],
  "nameserver-policy": {
    "geosite:private,cn,geolocation-cn": domesticNameservers,
    "geosite:google,youtube,telegram,gfw,geolocation-!cn": foreignNameservers
  }
};
// 规则集通用配置
const ruleProviderCommon = {
  "type": "http",
  "format": "yaml",
  "interval": 86400
};
// 规则集配置
const ruleProviders = {
  "reject": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt",
    "path": "./ruleset/loyalsoldier/reject.yaml"
  },
  "icloud": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/icloud.txt",
    "path": "./ruleset/loyalsoldier/icloud.yaml"
  },
  "apple": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/apple.txt",
    "path": "./ruleset/loyalsoldier/apple.yaml"
  },
  "google": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/google.txt",
    "path": "./ruleset/loyalsoldier/google.yaml"
  },
  "proxy": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt",
    "path": "./ruleset/loyalsoldier/proxy.yaml"
  },
  "direct": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt",
    "path": "./ruleset/loyalsoldier/direct.yaml"
  },
  "private": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/private.txt",
    "path": "./ruleset/loyalsoldier/private.yaml"
  },
  "gfw": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt",
    "path": "./ruleset/loyalsoldier/gfw.yaml"
  },
  "tld-not-cn": {
    ...ruleProviderCommon,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/tld-not-cn.txt",
    "path": "./ruleset/loyalsoldier/tld-not-cn.yaml"
  },
  "telegramcidr": {
    ...ruleProviderCommon,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/telegramcidr.txt",
    "path": "./ruleset/loyalsoldier/telegramcidr.yaml"
  },
  "cncidr": {
    ...ruleProviderCommon,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt",
    "path": "./ruleset/loyalsoldier/cncidr.yaml"
  },
  "lancidr": {
    ...ruleProviderCommon,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt",
    "path": "./ruleset/loyalsoldier/lancidr.yaml"
  },
  "applications": {
    ...ruleProviderCommon,
    "behavior": "classical",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt",
    "path": "./ruleset/loyalsoldier/applications.yaml"
  },
  "openai": {
    ...ruleProviderCommon,
    "behavior": "classical",
    "url": "https://fastly.jsdelivr.net/gh/blackmatrix7/ios_rule_script@master/rule/Clash/OpenAI/OpenAI.yaml",
    "path": "./ruleset/blackmatrix7/openai.yaml"
  }
};
// 规则
const rules = [
  // 自定义规则
  "DOMAIN-SUFFIX,googleapis.cn,节点选择", // Google服务
  "DOMAIN-SUFFIX,gstatic.com,节点选择", // Google静态资源
  "DOMAIN-SUFFIX,xn--ngstr-lra8j.com,节点选择", // Google Play下载服务
  "DOMAIN-SUFFIX,github.io,节点选择", // Github Pages
  "DOMAIN,v2rayse.com,节点选择", // V2rayse节点工具
  // blackmatrix7 规则集
  "RULE-SET,openai,ChatGPT",
  // Loyalsoldier 规则集
  "RULE-SET,applications,全局直连",
  "RULE-SET,private,全局直连",
  "RULE-SET,reject,广告过滤",
  "RULE-SET,icloud,微软服务",
  "RULE-SET,apple,苹果服务",
  "RULE-SET,google,谷歌服务",
  "RULE-SET,proxy,节点选择",
  "RULE-SET,gfw,节点选择",
  "RULE-SET,tld-not-cn,节点选择",
  "RULE-SET,direct,全局直连",
  "RULE-SET,lancidr,全局直连,no-resolve",
  "RULE-SET,cncidr,全局直连,no-resolve",
  "RULE-SET,telegramcidr,电报消息,no-resolve",
  // 其他规则
  "GEOIP,LAN,全局直连,no-resolve",
  "GEOIP,CN,全局直连,no-resolve",
  "MATCH,漏网之鱼"
];
// 代理组通用配置
const groupBaseOption = {
  "interval": 300,
  "timeout": 3000,
  "url": "https://www.google.com/generate_204",
  "lazy": true,
  "max-failed-times": 3,
  "hidden": false
};

// 程序入口
function main(config) {
  const proxyCount = config?.proxies?.length ?? 0;
  const proxyProviderCount =
    typeof config?.["proxy-providers"] === "object" ? Object.keys(config["proxy-providers"]).length : 0;
  if (proxyCount === 0 && proxyProviderCount === 0) {
    throw new Error("配置文件中未找到任何代理");
  }

  // 覆盖原配置中DNS配置
  config["dns"] = dnsConfig;

  // 覆盖原配置中的代理组
  config["proxy-groups"] = [
    {
      ...groupBaseOption,
      "name": "节点选择",
      "type": "select",
      "proxies": ["延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/adjust.svg"
    },
    {
      ...groupBaseOption,
      "name": "延迟选优",
      "type": "url-test",
      "tolerance": 100,
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/speed.svg"
    },
    {
      ...groupBaseOption,
      "name": "故障转移",
      "type": "fallback",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/ambulance.svg"
    },
    {
      ...groupBaseOption,
      "name": "负载均衡(散列)",
      "type": "load-balance",
      "strategy": "consistent-hashing",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/merry_go.svg"
    },
    {
      ...groupBaseOption,
      "name": "负载均衡(轮询)",
      "type": "load-balance",
      "strategy": "round-robin",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/balance.svg"
    },
    {
      ...groupBaseOption,
      "name": "谷歌服务",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/google.svg"
    },
    {
      ...groupBaseOption,
      "name": "国外媒体",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/youtube.svg"
    },
    {
      ...groupBaseOption,
      "name": "电报消息",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/telegram.svg"
    },
    {
      ...groupBaseOption,
      "url": "https://chatgpt.com",
      "expected-status": "200",
      "name": "ChatGPT",
      "type": "select",
      "include-all": true,
      "filter": "AD|🇦🇩|AE|🇦🇪|AF|🇦🇫|AG|🇦🇬|AL|🇦🇱|AM|🇦🇲|AO|🇦🇴|AR|🇦🇷|AT|🇦🇹|AU|🇦🇺|AZ|🇦🇿|BA|🇧🇦|BB|🇧🇧|BD|🇧🇩|BE|🇧🇪|BF|🇧🇫|BG|🇧🇬|BH|🇧🇭|BI|🇧🇮|BJ|🇧🇯|BN|🇧🇳|BO|🇧🇴|BR|🇧🇷|BS|🇧🇸|BT|🇧🇹|BW|🇧🇼|BZ|🇧🇿|CA|🇨🇦|CD|🇨🇩|CF|🇨🇫|CG|🇨🇬|CH|🇨🇭|CI|🇨🇮|CL|🇨🇱|CM|🇨🇲|CO|🇨🇴|CR|🇨🇷|CV|🇨🇻|CY|🇨🇾|CZ|🇨🇿|DE|🇩🇪|DJ|🇩🇯|DK|🇩🇰|DM|🇩🇲|DO|🇩🇴|DZ|🇩🇿|EC|🇪🇨|EE|🇪🇪|EG|🇪🇬|ER|🇪🇷|ES|🇪🇸|ET|🇪🇹|FI|🇫🇮|FJ|🇫🇯|FM|🇫🇲|FR|🇫🇷|GA|🇬🇦|GB|🇬🇧|GD|🇬🇩|GE|🇬🇪|GH|🇬🇭|GM|🇬🇲|GN|🇬🇳|GQ|🇬🇶|GR|🇬🇷|GT|🇬🇹|GW|🇬🇼|GY|🇬🇾|HN|🇭🇳|HR|🇭🇷|HT|🇭🇹|HU|🇭🇺|ID|🇮🇩|IE|🇮🇪|IL|🇮🇱|IN|🇮🇳|IQ|🇮🇶|IS|🇮🇸|IT|🇮🇹|JM|🇯🇲|JO|🇯🇴|JP|🇯🇵|KE|🇰🇪|KG|🇰🇬|KH|🇰🇭|KI|🇰🇮|KM|🇰🇲|KN|🇰🇳|KR|🇰🇷|KW|🇰🇼|KZ|🇰🇿|LA|🇱🇦|LB|🇱🇧|LC|🇱🇨|LI|🇱🇮|LK|🇱🇰|LR|🇱🇷|LS|🇱🇸|LT|🇱🇹|LU|🇱🇺|LV|🇱🇻|LY|🇱🇾|MA|🇲🇦|MC|🇲🇨|MD|🇲🇩|ME|🇲🇪|MG|🇲🇬|MH|🇲🇭|MK|🇲🇰|ML|🇲🇱|MM|🇲🇲|MN|🇲🇳|MR|🇲🇷|MT|🇲🇹|MU|🇲🇺|MV|🇲🇻|MW|🇲🇼|MX|🇲🇽|MY|🇲🇾|MZ|🇲🇿|NA|🇳🇦|NE|🇳🇪|NG|🇳🇬|NI|🇳🇮|NL|🇳🇱|NO|🇳🇴|NP|🇳🇵|NR|🇳🇷|NZ|🇳🇿|OM|🇴🇲|PA|🇵🇦|PE|🇵🇪|PG|🇵🇬|PH|🇵🇭|PK|🇵🇰|PL|🇵🇱|PS|🇵🇸|PT|🇵🇹|PW|🇵🇼|PY|🇵🇾|QA|🇶🇦|RO|🇷🇴|RS|🇷🇸|RW|🇷🇼|SA|🇸🇦|SB|🇸🇧|SC|🇸🇨|SD|🇸🇩|SE|🇸🇪|SG|🇸🇬|SI|🇸🇮|SK|🇸🇰|SL|🇸🇱|SM|🇸🇲|SN|🇸🇳|SO|🇸🇴|SR|🇸🇷|SS|🇸🇸|ST|🇸🇹|SV|🇸🇻|SZ|🇸🇿|TD|🇹🇩|TG|🇹🇬|TH|🇹🇭|TJ|🇹🇯|TL|🇹🇱|TM|🇹🇲|TN|🇹🇳|TO|🇹🇴|TR|🇹🇷|TT|🇹🇹|TV|🇹🇻|TW|🇹🇼|TZ|🇹🇿|UA|🇺🇦|UG|🇺🇬|US|🇺🇸|UY|🇺🇾|UZ|🇺🇿|VA|🇻🇦|VC|🇻🇨|VN|🇻🇳|VU|🇻🇺|WS|🇼🇸|YE|🇾🇪|ZA|🇿🇦|ZM|🇿🇲|ZW|🇿🇼",
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/chatgpt.svg"
    },
    {
      ...groupBaseOption,
      "name": "微软服务",
      "type": "select",
      "proxies": ["全局直连", "节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/microsoft.svg"
    },
    {
      ...groupBaseOption,
      "name": "苹果服务",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/apple.svg"
    },
    {
      ...groupBaseOption,
      "name": "广告过滤",
      "type": "select",
      "proxies": ["REJECT", "DIRECT"],
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/bug.svg"
    },
    {
      ...groupBaseOption,
      "name": "全局直连",
      "type": "select",
      "proxies": ["DIRECT", "节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/link.svg"
    },
    {
      ...groupBaseOption,
      "name": "全局拦截",
      "type": "select",
      "proxies": ["REJECT", "DIRECT"],
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/block.svg"
    },
    {
      ...groupBaseOption,
      "name": "漏网之鱼",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/fish.svg"
    }
  ];

  // 覆盖原配置中的规则
  config["rule-providers"] = ruleProviders;
  config["rules"] = rules;

  // 返回修改后的配置
  return config;
}
```

### 2. 剔除高倍率节点

> 如下是一个按倍率过滤代理组节点的 Script 脚本示例（可点击复制按钮）。

<!-- prettier-ignore -->
!!! info
    - 通过配置每个代理组的**最大倍率**，将超过最大倍率的节点从代理组中**剔除**。
    - 本脚本的剔除**无法作用于**通过 `include-all`、`include-all-proxies`、`include-all-providers` 引入的节点。

```javascript
// 代理组最大倍率
const multipliers = { "低倍率分组名称(自行修改)": 1.0, "中倍率分组名称(自行增删)": 2.5 };
// 剔除代理节点名称中不含倍率信息的节点
const multipliersOnly = false;
// 正则表达式
const regex = /(\d+(\.\d*)?) *?([xX✕✖⨉]|倍率?)/m;

function main(config) {
  const removed = []; // 过滤后不含任何节点将被移除的group
  config["proxy-groups"] = (config["proxy-groups"] ?? [])
    .map(group => {
      if (multipliers[group?.name]) {
        group["proxies"] = (group["proxies"] ?? []).filter(proxy => {
          const multiplier = regex.exec(proxy)?.[1];
          return multipliersOnly
            ? multiplier !== undefined && Number(multiplier) <= multipliers[group.name]
            : multiplier === undefined || Number(multiplier) <= multipliers[group.name];
        });
        if (group["proxies"]?.length > 0) {
          return group;
        }
        removed.push(group.name);
      } else {
        return group;
      }
    })
    .filter(Boolean);
  if (removed.length > 0) {
    // 移除代理组的引用
    config["proxy-groups"].forEach(group => {group["proxies"] = group["proxies"].filter(proxy => !removed.includes(proxy));});
    // 移除规则的引用
    config["rules"] = (config["rules"] ?? []).filter(rule => rule && removed.every(name => !rule.endsWith(name) && !rule.endsWith(`${name},no-resolve`)));
  }
  return config;
}
```

### 3. 使用扩展脚本实现`prependrules`

参考[Issue1437-comment-2395050752](https://github.com/clash-verge-rev/clash-verge-rev/issues/1437#issuecomment-2395050752)

### 4. 避开被目标站点封锁的节点

> 如下是一个修正「自动测速组总是选中测速正常、实际打不开的节点」的 Script 脚本示例（可点击复制按钮）。

<!-- prettier-ignore -->
!!! info
    - 适用于 `url-test` 类型的自动测速组：组里显示某个节点延迟最低，但实际访问目标站点时打不开。
    - 原因：mihomo 的健康检查发送的是 `HEAD` 请求，而部分 CDN（如 Cloudflare）对 `HEAD` 不执行地区限制，
      于是**被目标站点封锁的节点依然返回 2xx、被判为「健康」**；这类节点又往往物理延迟最低，便一直被选中。
    - 换健康检查地址解决不了这个问题：以 ChatGPT 为例，实测把地址换成
      `https://chatgpt.com/api/auth/session` 或 `https://chatgpt.com/backend-api/models`，
      香港节点依然全部「通过」。应当改为**把这类节点从候选集合里排除**。

```javascript
// 需要修正的分流组
var TARGETS = [
  {
    // 组名，改成订阅里实际的名字
    name: "ChatGPT最低延迟",
    // 从候选里排除的节点名关键字（正则），按自己机场的命名习惯调整
    exclude: "香港|HK|Hong ?Kong",
    // 候选集合干净后，改用标准端点测速，比用目标站点更能反映真实网络质量
    url: "http://www.gstatic.com/generate_204",
    // 健康检查间隔（秒）。默认 120 太长，节点掉线后要等两分钟才切走
    interval: 60
  }
];

// 订阅里常见的信息节点，一并排除
var BASE_EXCLUDE = "剩余|套餐|到期|官网";

function mergeExclude(current, add) {
  var base = typeof current === "string" && current.length > 0 ? current : BASE_EXCLUDE;
  return base.indexOf(add) >= 0 ? base : base + "|" + add;
}

function main(config) {
  var groups = config["proxy-groups"];
  if (!groups || typeof groups.length !== "number") {
    return config;
  }
  for (var i = 0; i < groups.length; i++) {
    var g = groups[i];
    if (!g) {
      continue;
    }
    for (var j = 0; j < TARGETS.length; j++) {
      var t = TARGETS[j];
      if (g.name !== t.name) {
        continue;
      }
      g["type"] = "url-test";
      g["url"] = t.url;
      g["interval"] = t.interval;
      g["timeout"] = 5000;
      g["tolerance"] = 0;
      g["lazy"] = false;
      g["max-failed-times"] = 2;
      g["include-all"] = true;
      g["exclude-type"] = "Direct|Reject";
      g["exclude-filter"] = mergeExclude(g["exclude-filter"], t.exclude);
      // 清掉可能存在的静态名单，让 include-all 生效
      delete g["proxies"];
      delete g["expected-status"];
      delete g["include-all-proxies"];
      delete g["include-all-providers"];
    }
  }
  return config;
}
```

**如何查出哪些节点真的被封锁**

逐个把节点切到 `GLOBAL`（模式切换为「全局」），再用本机混合端口请求目标站点的接口：

```bash
curl -x http://127.0.0.1:7897 -A "Mozilla/5.0" -i https://chatgpt.com/api/auth/session
```

返回 `200` 表示可用；`403` 且响应体含 `blocked-ico` 表示被该站点地区封锁。
把封锁节点的名字关键字填进上面的 `exclude` 即可。

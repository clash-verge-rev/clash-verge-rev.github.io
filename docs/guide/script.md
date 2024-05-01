## 新增脚本

<!-- prettier-ignore -->
!!! warning
    如果创建了多个 Script 配置，按照启用顺序先后，链式执行

![新增脚本](../assets/guide/script/script.gif)

## 脚本说明

- 脚本语言为 `javascript`， 使用 `boa_engine` 作为执行器。
- 程序的入口为 `main` 函数，接受一个 `params` 参数（参数名不限制），并返回修改后的该参数。
- params 为 `object` 类型，属性值为 `yaml` 配置文件内容转 `JSON` 后对应的 `object` 对象。

```javascript
function main(params) {
  return params;
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

> 如下是一个套用 Loyalsoldier 规则集的 Script 脚本示例（可点击复制按钮）。

<!-- prettier-ignore -->
!!! info
    你可以在脚本中的 `// 自定义规则` 下面自行添加规则。
    规则配置请参考 [规则配置文档](https://wiki.metacubex.one/config/rules/)

```javascript
// 国内DNS服务器
const nameservers = [
  "https://dns.alidns.com/dns-query", // 阿里云公共DNS
  "https://doh.pub/dns-query", // 腾讯DNSPod
  "https://doh.360.cn/dns-query" // 360安全DNS
];
// 国外DNS服务器
const fallback_nameservers = [
  "https://1.1.1.1/dns-query", // Cloudflare(主)
  "https://1.0.0.1/dns-query", // Cloudflare(备)
  "https://208.67.222.222/dns-query", // OpenDNS(主)
  "https://208.67.220.220/dns-query", // OpenDNS(备)
  "https://194.242.2.2/dns-query", // Mullvad(主)
  "https://194.242.2.3/dns-query" // Mullvad(备)
];
// DNS配置
const dns = {
  "dns": true,
  "listen": 1053,
  "ipv6": true,
  "use-hosts": true,
  "cache-algorithm": "arc",
  "enhanced-mode": "fake-ip",
  "fake-ip-range": "198.18.0.1/16",
  "fake-ip-filter": ["+.lan", "+.local", "+.msftconnecttest.com", "+.msftncsi.com"],
  "default-nameserver": ["223.5.5.5", "114.114.114.114", "1.1.1.1", "8.8.8.8"],
  "nameserver": nameservers,
  "proxy-server-nameserver": nameservers,
  "nameserver-policy": {
    "geosite:private,cn,geolocation-cn": nameservers,
    "geosite:google,youtube,telegram,gfw,geolocation-!cn": fallback_nameservers
  }
};
// 规则集
const provider_common = {
  "type": "http",
  "format": "yaml",
  "interval": 86400
};
const rule_providers = {
  "reject": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/reject.txt",
    "path": "./rulesets/loyalsoldier/reject.yaml"
  },
  "icloud": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/icloud.txt",
    "path": "./rulesets/loyalsoldier/icloud.yaml"
  },
  "apple": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/apple.txt",
    "path": "./rulesets/loyalsoldier/apple.yaml"
  },
  "google": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/google.txt",
    "path": "./rulesets/loyalsoldier/google.yaml"
  },
  "proxy": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/proxy.txt",
    "path": "./rulesets/loyalsoldier/proxy.yaml"
  },
  "direct": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/direct.txt",
    "path": "./rulesets/loyalsoldier/direct.yaml"
  },
  "private": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/private.txt",
    "path": "./rulesets/loyalsoldier/private.yaml"
  },
  "gfw": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/gfw.txt",
    "path": "./rulesets/loyalsoldier/gfw.yaml"
  },
  "tld-not-cn": {
    ...provider_common,
    "behavior": "domain",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/tld-not-cn.txt",
    "path": "./rulesets/loyalsoldier/tld-not-cn.yaml"
  },
  "telegramcidr": {
    ...provider_common,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/telegramcidr.txt",
    "path": "./rulesets/loyalsoldier/telegramcidr.yaml"
  },
  "cncidr": {
    ...provider_common,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/cncidr.txt",
    "path": "./rulesets/loyalsoldier/cncidr.yaml"
  },
  "lancidr": {
    ...provider_common,
    "behavior": "ipcidr",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/lancidr.txt",
    "path": "./rulesets/loyalsoldier/lancidr.yaml"
  },
  "applications": {
    ...provider_common,
    "behavior": "classical",
    "url": "https://fastly.jsdelivr.net/gh/Loyalsoldier/clash-rules@release/applications.txt",
    "path": "./rulesets/loyalsoldier/applications.yaml"
  }
};
// 规则
const rules = [
  // 自定义规则
  "DOMAIN-SUFFIX,googleapis.cn,节点选择",
  "DOMAIN-SUFFIX,gstatic.com,节点选择",
  "DOMAIN-SUFFIX,xn--ngstr-lra8j.com,节点选择",
  "DOMAIN-SUFFIX,github.io,节点选择",
  "DOMAIN,v2rayse.com,节点选择",
  // Loyalsoldier 规则集
  "RULE-SET,reject,广告过滤",
  "RULE-SET,icloud,微软服务",
  "RULE-SET,apple,苹果服务",
  "RULE-SET,google,谷歌服务",
  "RULE-SET,proxy,节点选择",
  "RULE-SET,gfw,节点选择",
  "RULE-SET,tld-not-cn,节点选择",
  "RULE-SET,applications,全局直连",
  "RULE-SET,private,全局直连",
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
const group_base_option = {
  "interval": 300,
  "timeout": 3000,
  "url": "https://www.google.com/generate_204",
  "lazy": true,
  "max-failed-times": 3,
  "hidden": false
};
// 程序入口
function main(config) {
  const proxy_count = config?.proxies?.length ?? 0;
  const proxy_providers_count =
    typeof config?.["proxy-providers"] === "object" ? Object.keys(config["proxy-providers"]).length : 0;
  if (proxy_count === 0 && proxy_providers_count === 0) {
    throw new Error("配置文件中未找到任何代理");
  }

  // 覆盖原配置中DNS配置
  config["dns"] = dns;

  // 覆盖原配置中的代理组
  config["proxy-groups"] = [
    {
      ...group_base_option,
      "name": "节点选择",
      "type": "select",
      "proxies": ["延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/adjust.svg"
    },
    {
      ...group_base_option,
      "name": "延迟选优",
      "type": "url-test",
      "tolerance": 100,
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/speed.svg"
    },
    {
      ...group_base_option,
      "name": "故障转移",
      "type": "fallback",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/ambulance.svg"
    },
    {
      ...group_base_option,
      "name": "负载均衡(散列)",
      "type": "load-balance",
      "strategy": "consistent-hashing",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/merry_go.svg"
    },
    {
      ...group_base_option,
      "name": "负载均衡(轮询)",
      "type": "load-balance",
      "strategy": "round-robin",
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/balance.svg"
    },
    {
      ...group_base_option,
      "name": "谷歌服务",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/google.svg"
    },
    {
      ...group_base_option,
      "name": "国外媒体",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/youtube.svg"
    },
    {
      ...group_base_option,
      "name": "电报消息",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/telegram.svg"
    },
    {
      ...group_base_option,
      "name": "微软服务",
      "type": "select",
      "proxies": ["全局直连", "节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/microsoft.svg"
    },
    {
      ...group_base_option,
      "name": "苹果服务",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/apple.svg"
    },
    {
      ...group_base_option,
      "name": "广告过滤",
      "type": "select",
      "proxies": ["REJECT", "DIRECT"],
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/bug.svg"
    },
    {
      ...group_base_option,
      "name": "全局直连",
      "type": "select",
      "proxies": ["DIRECT", "节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/link.svg"
    },
    {
      ...group_base_option,
      "name": "全局拦截",
      "type": "select",
      "proxies": ["REJECT", "DIRECT"],
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/block.svg"
    },
    {
      ...group_base_option,
      "name": "漏网之鱼",
      "type": "select",
      "proxies": ["节点选择", "延迟选优", "故障转移", "负载均衡(散列)", "负载均衡(轮询)", "全局直连"],
      "include-all": true,
      "icon": "https://fastly.jsdelivr.net/gh/clash-verge-rev/clash-verge-rev.github.io@main/docs/assets/icons/fish.svg"
    }
  ];

  // 覆盖原配置中的规则
  config["rule-providers"] = rule_providers;
  config["rules"] = rules;

  // 返回修改后的配置
  return config;
}
```

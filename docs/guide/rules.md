<!-- prettier-ignore -->

# 自定义路由规则

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
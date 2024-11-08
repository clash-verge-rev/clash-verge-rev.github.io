## PAC的实现脚本
实现原理通过js是替换某个/所有订阅的rules  

+ 你可以对单个订阅的脚本

![QQ_1731076786921](https://gd-hbimg.huaban.com/14db1f91f125b809c972fd8a67125800c9142fd02e6fe-3iutk3)

+ 或者针对所有订阅的脚本

![QQ_1731076840908](https://gd-hbimg.huaban.com/bf17139cfa77a35ac7413ba4bad276a65faf8fd11fa40-FCm6Cx)

### 示例脚本:

仅代理列出的域名和规则,其他的全部走直连:

```js
function main(config, profileName) {
  replaceRules(config);
  return config;
}

const replaceRules = (config) => {
  const prefix = "DOMAIN-SUFFIX,";
  let selectName = "proxy";
  config["proxy-groups"].forEach(group => {
    if (group.type === "select") {
      selectName = group.name;
    }
  })
  const suffix = "," + selectName;
  const updatedDomin = domin_suffix.map(domain => `${prefix}${domain}${suffix}`);
  //其他的全部走直连
  updatedDomin.push("MATCH,DIRECT")
  config.rules = updatedDomin;
}

const domin_suffix = [
  "autoxjs.com",
  "android.com",
  "appspot.com",
  "chatgpt.com",
  "coderprofile.com",
  "dropbox.com",
  "esa.int",
  "get.dev",
  "ggpht.com",
  "github.com",
  "githubassets.com",
  "go.dev",
  "go-kratos.dev",
  "golang.org",
  "google.com",
  "googleapis.com",
  "googleusercontent.com",
  "googlevideo.com",
  "gravatar.com",
  "greasyfork.org",
  "gstatic.com",
  "ipaddress.my",
  "openai.com",
  "sqlalchemy.org",
  "registry.google",
  "sstatic.net",
  "stack.imgur.com",
  "stackoverflow.com",
  "tokyo-hot.com",
  "torproject.netopenai.com",
  "torproject.org",
  "whatismyipaddress.com",
  "wikipedia.org",
  "youtube.com",
  "ytimg.com",
  "z-lib.org",
  "cookieyes.com",
]


```


# 多订阅合并配置

更多细节请自行查阅 [MetaCubeX/Mihomo官方文档](https://wiki.metacubex.one/config)。

这是一个示例配置，其中的部分配置(可能)不适合每个人，请根据自身环境调整。

> 食用方法：按照格式在上半片中编排自己的订阅

```yaml
proxy-providers:
  1.provider1:
    type: http
    # 节点文件 p1对应provider1
    path: ./proxy_provider/p1.yaml
    # 订阅 链接
    url: https://verge.dginv.click/#/register?code=oaxsAGo6
    # 自动更新时间 21600(秒) / 3600 = 6小时
    interval: 21600
    # 节点名称前缀 p1
    override:
      additional-prefix: "p1 |"
  2.subscribe:
    type: http
    path: ./proxy_provider/p2.yaml
    url: https://verge.dginv.click/#/register?code=oaxsAGo6
    interval: 21600
    override:
      additional-prefix: "P2 |"

# Global Config
mixed-port: 7897
allow-lan: true
mode: rule
global-ua: clash-verge/v2.2.3
geodata-mode: true
geodata-loader: standard
unified-delay: true
log-level: warning
ipv6: true
external-controller: 127.0.0.1:9090
tcp-concurrent: true
enable-process: true
find-process-mode: strict
global-client-fingerprint: chrome
keep-alive-interval: 15
geo-auto-update: true
geo-update-interval: 24
geox-url:
  geoip: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat"
  geosite: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat"
  mmdb: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb"
  asn: "https://cdn.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/GeoLite2-ASN.mmdb"
profile:
  store-selected: true
  store-fake-ip: true
tun:
  enable: false
  device: ClashVergeREV
  stack: mixed
  mtu: 1500
  dns-hijack:
    - "any:53"
    - "tcp://any:53"
  auto-route: true
  auto-detect-interface: true
dns:
  enable: true
  ipv6: true
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  use-hosts: true
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
    - 114.114.114.114
    - system
  fake-ip-filter:
    - "*"
    - "+.lan"
    - "connect.rom.miui.com"
    - "+.miwifi.com"
    - "+.ntp.org"
    - "+.u-tools.cn"
    - "+.mediatek.com"
    - "+.cfprefer1.xyz"
    - "+.wetab.link"
    - "+.tyasaka.xyz"
    # QQ
    - "localhost.ptlogin2.qq.com"
    - "localhost.sec.qq.com"
    # WeChat
    - "localhost.work.weixin.qq.com"
    - "+.weixin.qq.com"
    - "+.wechat.com"
    # Windows
    - "time.windows.com"
    - "+.msftconnecttest.com"
    - "+.msftncsi.com"
    - "+.wpsmail.net"
    - "+.henzanapp.com"
    - "+.pconline.com.cn"
proxy-groups:
  - name: 手动选择
    type: select
    proxies: [DIRECT,自动选择]
    url: "https://www.gstatic.com/generate_204"
    interval: 300
    max-failed-times: 5
    include-all: true
  - name: 自动选择
    type: url-test
    proxies: [DIRECT]
    url: "https://www.gstatic.com/generate_204"
    interval: 300
    max-failed-times: 5
    include-all: true
rules:
  - GEOSITE,CN,DIRECT
  - GEOIP,CN,DIRECT,no-resolve
  - MATCH,手动选择
```


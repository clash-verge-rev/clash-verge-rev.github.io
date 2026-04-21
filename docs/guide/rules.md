# Clash Verge 自定义规则配置

> 自定义规则可以精确控制流量分流，实现特定网站或应用的直连、代理或阻断。

## 配置步骤

### 1. 编辑订阅规则（注意此处编辑的规则**不会**随着订阅更新而失效）

右键点击订阅配置文件，选择 **"编辑规则"**。

![edit-rules](../assets/guide/custom_rules/edit-rules.png)

### 2. 添加规则

![add-rules](../assets/guide/custom_rules/add-rules.png)

### 3. 规则类型说明

规则从上到下依次匹配，匹配到第一条符合条件的规则后停止。

#### 域名规则

**DOMAIN** - 完整域名匹配
```yaml
- DOMAIN,google.com,代理策略
- DOMAIN,baidu.com,DIRECT
```

**DOMAIN-SUFFIX** - 域名后缀匹配
```yaml
- DOMAIN-SUFFIX,google.com,代理策略  # 匹配 *.google.com
- DOMAIN-SUFFIX,cn,DIRECT           # 匹配所有 .cn 域名
```

**DOMAIN-KEYWORD** - 域名关键词匹配
```yaml
- DOMAIN-KEYWORD,google,代理策略     # 匹配包含 google 的域名
- DOMAIN-KEYWORD,ads,REJECT         # 阻断包含 ads 的域名
```

**GEOSITE** - 地理站点匹配
```yaml
- GEOSITE,youtube,代理策略           # YouTube 相关域名
- GEOSITE,cn,DIRECT                 # 中国大陆网站
- GEOSITE,category-ads-all,REJECT   # 广告域名
```

#### IP 规则

**IP-CIDR** - IP 地址段匹配
```yaml
- IP-CIDR,192.168.0.0/16,DIRECT     # 局域网直连
- IP-CIDR,10.0.0.0/8,DIRECT         # 私有网络
```

**GEOIP** - 地理 IP 匹配
```yaml
- GEOIP,CN,DIRECT                   # 中国 IP 直连
- GEOIP,US,代理策略                  # 美国 IP 走代理
```

!!! warning "GEOIP 依赖 DNS 解析，可能被污染影响"
    对于按域名发起、且未先命中域名规则的连接，`GEOIP` 这类 IP 规则需要先解析域名得到目标 IP，再按 IP 归属地匹配。若上游 DNS 被污染，境外域名可能被解析到国内 IP 段，进而被 `GEOIP,CN,DIRECT` 误判为直连。详见下方 **常用配置示例 → 国内网站直连** 小节中关于陷阱的说明。

#### 端口规则

**DST-PORT** - 目标端口匹配
```yaml
- DST-PORT,22,DIRECT                # SSH 端口直连
- DST-PORT,80/443,代理策略           # HTTP/HTTPS 端口
```

#### 应用规则

**PROCESS-NAME** - 进程名匹配
```yaml
- PROCESS-NAME,Telegram.exe,代理策略  # Telegram 走代理
- PROCESS-NAME,WeChat.exe,DIRECT     # 微信直连
```

#### 逻辑规则

**AND** - 多条件同时满足
```yaml
- AND,((DOMAIN-SUFFIX,youtube.com),(GEOIP,!CN)),代理策略
```

**OR** - 多条件满足其一
```yaml
- OR,((DOMAIN-KEYWORD,google),(DOMAIN-KEYWORD,youtube)),代理策略
```

#### 通用规则

**MATCH** - 兜底规则（必须放在最后）
```yaml
- MATCH,代理策略                     # 其他所有流量的默认策略
```

### 4. 代理策略选择

在规则中指定的 **"代理策略"** 需要选择您配置的节点分组：

- `DIRECT` - 直连，不使用代理
- `REJECT` - 阻断连接
- `代理组名称` - 如 `🚀 手动切换`、`🎯 全球直连` 等

### 5. 规则插入位置

规则可以插入到 **"前置规则"** 或 **"后置规则"**：

- **前置规则**：优先级最高，建议将自定义规则放在此处
- **后置规则**：在订阅规则之后生效

**推荐使用前置规则**，因为规则按从上到下的顺序匹配，前置规则能确保您的自定义配置优先生效。

## 常用配置示例

### 广告屏蔽
```yaml
- GEOSITE,category-ads-all,REJECT
- DOMAIN-KEYWORD,ads,REJECT
- DOMAIN-KEYWORD,tracker,REJECT
```

### 国内网站直连
```yaml
- GEOSITE,cn,DIRECT
- GEOIP,CN,DIRECT
- DOMAIN-SUFFIX,cn,DIRECT
```

!!! warning "GEOIP,CN 误判陷阱：显式 HTTP 代理下，境外域名可能被误判为直连"
    上述配置对国内网站友好，但在**显式 HTTP 代理**场景（例如应用直接把 `mixed-port` / `port` 配成 HTTP 代理）下，部分境外域名可能因 DNS 污染被解析到中国大陆 IP，继而命中 `GEOIP,CN,DIRECT`，表现为 TLS 握手超时或 `schannel: failed to receive handshake`、`unexpected EOF during handshake` 等错误。

    **原因**：

    - 对按域名发起的连接，如果前面没有命中域名类规则，`GEOIP` 这类 IP 规则需要先解析域名得到目标 IP，再按 IP 归属地匹配。
    - 在显式 HTTP 代理场景下，客户端发送 `CONNECT host:port`；Mihomo 仍需自行解析该域名，以继续匹配后续 IP 类规则。
    - 如果上游 `nameserver` 返回了受污染的大陆 IP，该请求就可能命中 `GEOIP,CN,DIRECT` 后直连失败。
    - 这不代表应直接删除 `GEOIP,CN,DIRECT`；问题在于需要把明确应走代理的域名规则放在它之前。

    **诊断方式**：

    - 开启 `Debug` 日志，确认目标请求最终命中了哪条规则、使用了哪个策略组。
    - 同时查看 DNS 解析结果；如果目标域名先被解析到异常的大陆 IP，随后又命中 `GeoIP(CN)` + `DIRECT`，即可判定为该问题。
    - 也可在命令行直接复现污染：`nslookup chatgpt.com 223.5.5.5`（PowerShell 下 `Resolve-DnsName chatgpt.com -Server 223.5.5.5`），若返回的 A 记录落在国内 IP 段，即佐证上游 DNS 已被污染。

    **推荐处理**：把明确需要代理的域名前置到 `GEOIP,CN,DIRECT` 之前，例如：

    ```yaml
    - DOMAIN-SUFFIX,chatgpt.com,代理策略
    - DOMAIN-SUFFIX,openai.com,代理策略
    - DOMAIN-SUFFIX,oaistatic.com,代理策略
    - DOMAIN-SUFFIX,oaiusercontent.com,代理策略

    - GEOSITE,cn,DIRECT
    - GEOIP,CN,DIRECT
    - DOMAIN-SUFFIX,cn,DIRECT
    ```

    如需从 DNS 侧缓解，应优先使用 `nameserver-policy`、`fallback` / `fallback-filter`，必要时为直连出口单独配置 `direct-nameserver`。`proxy-server-nameserver` 仅用于代理节点自身域名的解析。

### 流媒体分流
```yaml
- GEOSITE,netflix,🎬 流媒体
- GEOSITE,youtube,📹 YouTube
- GEOSITE,disney,🏰 Disney+
```

### 应用分流
```yaml
- PROCESS-NAME,Telegram.exe,📱 Telegram
- PROCESS-NAME,steam.exe,🎮 游戏加速
- PROCESS-NAME,WeChat.exe,DIRECT
```

## 注意事项

1. **规则顺序很重要** - 规则按从上到下匹配，越靠前优先级越高
2. **测试规则效果** - 配置后建议测试访问对应网站确认规则生效
3. **定期更新规则** - 网站域名可能变化，需要适时调整规则
4. **避免规则冲突** - 确保规则逻辑清晰，避免前后矛盾

## 更多信息

更多规则语法和高级用法请参考：[Clash Meta 规则文档](https://wiki.metacubex.one/config/rules/)
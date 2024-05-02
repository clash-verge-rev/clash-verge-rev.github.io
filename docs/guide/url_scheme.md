## 订阅响应头

### content-disposition

如果响应头中存在 `content-disposition` 字段，则使用 `filename` 对应的值作为配置文件名。

```yaml
content-disposition: attachment; filename="test.yaml"
```

如果含有中文，则需要使用使用 `UTF-8` 编码。

```yaml
content-disposition: attachment;filename*=UTF-8''%E6%B5%8B%E8%AF%95%E8%AE%A2%E9%98%85
```

### profile-update-interval

如果响应头中存在 `profile-update-interval` 字段，则配置文件的 `更新间隔` 将被设置为对应的值（单位: 小时）。

```yaml
profile-update-interval: 24
```

### subscription-userinfo

如果响应头中存在 `subscription-userinfo` 字段，则其对应的流量信息(单位: 字节)、到期信息(时间戳)会显示在订阅卡片上。

```yaml
subscription-userinfo: upload=1234; download=2234; total=1024000; expire=2218532293
```

### profile-web-page-url

如果响应头中存在 `profile-web-page-url` 字段，则右键订阅卡片将会显示 `首页` 按钮。

```yaml
profile-web-page-url: https://example.com
```

<!-- prettier-ignore -->
!!! info
    - 一般要求请求的 `UA` 中含有 `clash` 字样才会返回该响应头。
    - 此功能要求Clash Verge Rev版本至少为 `v1.6.0`。

![profile-web-page-url](../assets/guide/url_scheme/profile_web_page_url.gif)s

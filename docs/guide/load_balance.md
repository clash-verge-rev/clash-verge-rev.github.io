## 负载均衡配置

### 方式一: 你可以对单个订阅进行文件修改.[配置wiki](https://wiki.metacubex.one/config/proxy-groups/load-balance/)

![QQ_1731076283944](https://gd-hbimg.huaban.com/30aabe7358fe04f2866c3c0701f31471688dc2fd5c88b-zzv1HR)

在文件的`proxy-groups`的下添加  

```
proxy-groups:
- name: "load-balance"
  type: load-balance
  proxies:
  - ss1
  - ss2
  - vmess1
  url: 'https://www.gstatic.com/generate_204'
  interval: 300
  #lazy: true
  #strategy: consistent-hashing
```

负载均衡有[多重模式](https://wiki.metacubex.one/config/proxy-groups/load-balance/#strategy)

- `round-robin` 将会把所有的请求分配给策略组内不同的代理节点
- `consistent-hashing` 将相同的 `目标地址` 的请求分配给策略组内的同一个代理节点
- `sticky-sessions`: 将相同的 `来源地址` 和 `目标地址` 的请求分配给策略组内的同一个代理节点，缓存 10 分钟过期

# 方式二 使用js 



方式一有一个缺点,一旦更新订阅,会覆盖掉修改的文件.这时候可以使用针对单个订阅的脚本

![QQ_1731076786921](https://gd-hbimg.huaban.com/14db1f91f125b809c972fd8a67125800c9142fd02e6fe-3iutk3)

或者针对所有订阅的脚本

![QQ_1731076840908](https://gd-hbimg.huaban.com/bf17139cfa77a35ac7413ba4bad276a65faf8fd11fa40-FCm6Cx)

#### 示例脚本

`round-robin` 主要适用于多ip同时访问同一网站的情况:如爬虫或者其他ip限制的情况

一下脚本作用是: 在搜集该订阅下所有ip不同的服务器,并添加一个load-balance 组用于负载均衡

```js
function main(config, profileName) {
  addLoadBalanceGroup(config);
  return config;
}
const addLoadBalanceGroup = (config) => {
  // 仅保留第一个相同 server 的项
  const uniqueProxies = Object.values(config.proxies.reduce((acc, proxy) => {
    if (!acc[proxy.server]) {
      acc[proxy.server] = proxy;
    }
    return acc;
  }, {}));

  // 从 uniqueProxies 中提取所有 name
  const proxyNames = uniqueProxies.map(proxy => proxy.name);
  //strategy¶
  // 负载均衡策略
  // round-robin 将会把所有的请求分配给策略组内不同的代理节点
  // consistent-hashing 将相同的 目标地址 的请求分配给策略组内的同一个代理节点
  // sticky-sessions: 将相同的 来源地址 和 目标地址 的请求分配给策略组内的同一个代理节点，缓存 10 分钟过期
  // 创建 proxy-groups，其中包含从 uniqueProxies 提取的所有 name
  lbgroups = {
    "name": "load-balance",
    "type": "load-balance",
    "proxies": proxyNames,
    "strategy": "round-robin"
  }
  config["proxy-groups"].forEach(group => {
    if (group.type === "select") {
      group.proxies.unshift(lbgroups.name);  // 你可以根据需要修改 lb 的值
    }
  });
  config["proxy-groups"].push(lbgroups)

}
```

此时会出现一个proxy-groups组load-balance ,选择即可实现负载均衡

![QQ_1731077424619](https://gd-hbimg.huaban.com/2be98311a2dd48b8b34952160d49aeed51aa14eac5c3-urJVsg)

### 负载均衡注意事项

同一个连接的时候并不会使用不同的代理,所以要使用不同连接来进行负载均衡,注意要配置要访问的特定域名走代理,可以使用全局或者参考[pac](./pac.md)  



golang的线程池参考(同一网址使用不同ip负载均衡进行访问):`poolhttp.go`

```
package poolhttp

import (
	"log"
	"net/http"
	"net/url"
)

type ClientPool struct {
	mapClients map[*http.Client]*url.URL
	clients    chan *http.Client
	validURLs  []*url.URL
}

func (p *ClientPool) AddURL(c *http.Client, u *url.URL) {
	if _, ok := p.mapClients[c]; !ok {
		p.mapClients[c] = u
	}
}
func (p *ClientPool) GetURL(c *http.Client) *url.URL {
	if _, ok := p.mapClients[c]; ok {
		return p.mapClients[c]
	}
	return nil
}

// 初始化客户端池
func NewClientPool(proxyURLs []string, batchSize int) *ClientPool {
	validURLs := checkProxyURLs(proxyURLs)
	chanSize := batchSize * len(validURLs)
	pool := &ClientPool{
		clients:    make(chan *http.Client, chanSize),
		validURLs:  validURLs,
		mapClients: make(map[*http.Client]*url.URL),
	}
	// 初始化池，填充最大数量的客户端
	for i := 0; i < batchSize; i++ {
		for j := 0; j < len(validURLs); j++ {
			client, _ := newProxyClient(validURLs[j])
			if i == 0 {
				pool.AddURL(client, validURLs[j])
			}
			pool.clients <- client
		}
	}
	return pool
}

// 获取一个代理客户端
func (p *ClientPool) Get() *http.Client {
	return <-p.clients
}

// 将客户端放回池中
func (p *ClientPool) Put(client *http.Client) {
	p.clients <- client
}
func NewProxyClientByUrl(porxyUrl string) *http.Client {
	validURLs := checkProxyURLs([]string{porxyUrl})
	if len(validURLs) < 1 {
		return nil
	}
	client, err := newProxyClient(validURLs[0])
	if err != nil {
		return nil
	}
	return client
}
func newProxyClient(porxyUrl *url.URL) (*http.Client, error) {
	// 创建一个带有代理的 Transport
	transport := &http.Transport{
		Proxy: http.ProxyURL(porxyUrl),
	}
	// 创建一个带有自定义 Transport 的 Client
	client := &http.Client{
		Transport: transport,
	}
	return client, nil
} 
// 创建一个新的代理 HTTP 客户端
func checkProxyURLs(proxyURLs []string) []*url.URL {
	var validURLs []*url.URL
	for _, proxyURL := range proxyURLs {
		parsedURL, err := url.Parse(proxyURL)
		if err != nil {
			log.Printf("无效的 URL: %s, 错误: %v", proxyURL, err)
			continue
		}
		if parsedURL.Scheme == "" || parsedURL.Host == "" {
			log.Printf("无效的 URL: %s, 缺少 scheme 或 host", proxyURL)
			continue
		}
		validURLs = append(validURLs, parsedURL)
	}
	return validURLs
}
```



使用` poolhttp_test.go`



```
package poolhttp

import (
	"fmt"
	"io"
	"log"
	"sync"
	"testing"
	"time"
)

func GetIP(pool *ClientPool) error {
	// 使用客户端池
	client := pool.Get()
	defer pool.Put(client)

	resp, err := client.Get("https://directory.cookieyes.com/api/v1/ip")
	if err != nil {
		log.Printf("%v", err)
	}
	//模拟长连接
	time.Sleep(1 * time.Second)
	defer resp.Body.Close()
	body, err := io.ReadAll(resp.Body)
	if err != nil {
		return fmt.Errorf("client:%v, err:%v", client.Transport, err)
	}
	log.Printf("resp.Body: %s", body)
	return nil
}
func TestPoolHttp(t *testing.T) {
	proxies := []string{}
	for i := 0; i < 6; i++ {
		proxies = append(proxies, fmt.Sprintf("http://127.0.0.1:%d", 7897))
	}

	var wg sync.WaitGroup
	pool := NewClientPool(proxies, 2)
	for i := 0; i < 20; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			err := GetIP(pool)
			if err != nil {
				log.Printf("err:%v", err)
				return
			}
		}()
	}
	wg.Wait()
}
```



上面代码每个http.client复用2次的时候,结果如下:

```
2024/11/08 20:37:21 resp.Body: {"ip":"87.121.61.171","country":"FR","country_name":"France","region_code":"GES","in_eu":true,"continent":"EU"}
2024/11/08 20:37:22 resp.Body: {"ip":"107.189.29.215","country":"LU","country_name":"Luxembourg","region_code":"LU","in_eu":true,"continent":"EU"}
2024/11/08 20:37:22 resp.Body: {"ip":"107.189.29.215","country":"LU","country_name":"Luxembourg","region_code":"LU","in_eu":true,"continent":"EU"}
2024/11/08 20:37:22 resp.Body: {"ip":"103.97.88.209","country":"NL","country_name":"The Netherlands","region_code":"NH","in_eu":true,"continent":"EU"}
2024/11/08 20:37:22 resp.Body: {"ip":"209.200.246.141","country":"CA","country_name":"Canada","region_code":"ON","in_eu":false,"continent":"NA"}
2024/11/08 20:37:22 resp.Body: {"ip":"209.200.246.141","country":"CA","country_name":"Canada","region_code":"ON","in_eu":false,"continent":"NA"}
2024/11/08 20:37:22 resp.Body: {"ip":"61.224.133.143","country":"TW","country_name":"Taiwan","region_code":"TXG","in_eu":false,"continent":"AS"}
2024/11/08 20:37:22 resp.Body: {"ip":"184.174.96.140","country":"US","country_name":"United States","region_code":"DE","in_eu":false,"continent":"NA"}
2024/11/08 20:37:22 resp.Body: {"ip":"184.174.96.140","country":"US","country_name":"United States","region_code":"DE","in_eu":false,"continent":"NA"}
2024/11/08 20:37:22 resp.Body: {"ip":"150.66.12.23","country":"JP","country_name":"Japan","region_code":"14","in_eu":false,"continent":"AS"}
2024/11/08 20:37:23 resp.Body: {"ip":"87.121.61.171","country":"FR","country_name":"France","region_code":"GES","in_eu":true,"continent":"EU"}
2024/11/08 20:37:23 resp.Body: {"ip":"107.189.29.215","country":"LU","country_name":"Luxembourg","region_code":"LU","in_eu":true,"continent":"EU"}
2024/11/08 20:37:23 resp.Body: {"ip":"107.189.29.215","country":"LU","country_name":"Luxembourg","region_code":"LU","in_eu":true,"continent":"EU"}
2024/11/08 20:37:23 resp.Body: {"ip":"209.200.246.141","country":"CA","country_name":"Canada","region_code":"ON","in_eu":false,"continent":"NA"}
2024/11/08 20:37:23 resp.Body: {"ip":"103.97.88.209","country":"NL","country_name":"The Netherlands","region_code":"NH","in_eu":true,"continent":"EU"}
2024/11/08 20:37:23 resp.Body: {"ip":"61.224.133.143","country":"TW","country_name":"Taiwan","region_code":"TXG","in_eu":false,"continent":"AS"}
2024/11/08 20:37:23 resp.Body: {"ip":"209.200.246.141","country":"CA","country_name":"Canada","region_code":"ON","in_eu":false,"continent":"NA"}
2024/11/08 20:37:23 resp.Body: {"ip":"184.174.96.140","country":"US","country_name":"United States","region_code":"DE","in_eu":false,"continent":"NA"}
2024/11/08 20:37:23 resp.Body: {"ip":"103.172.116.236","country":"SG","country_name":"Singapore","region_code":"","in_eu":false,"continent":"AS"}
2024/11/08 20:37:23 resp.Body: {"ip":"103.172.116.236","country":"SG","country_name":"Singapore","region_code":"","in_eu":false,"continent":"AS"}
```


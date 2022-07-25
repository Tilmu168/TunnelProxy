# 搭建隧道代理

### Nginx配置
```
 --建立连接，请在这里配置Redis 的 IP 地址、端口号、密码和用到的 Key
local rhost = "127.0.0.1"
local rport = 6379
local rpass = "abcdefg"
local rkey = "proxy:pool"
local ok, err = redis_instance:connect(rhost, rport)

-- 如果没有密码，移除下面这一行
local res, err = redis_instance:auth(rpass)
```

### yaml配置
```yaml
redis:
  host:  127.0.0.1
  port:  6379
  password:
  key:  'proxy:pool'
proxy:
  '获取代理IP的接口'

```

### 支持解析的代理格式
```json
{
    "msg": "",
    "code": 0,
    "data": {
        "count": 12,
        "proxy_list": [
            "222.162.44.198:19507",
            "125.79.50.171:21095",
            "180.106.246.208:18367",
            "106.116.213.2:16365",
            "113.75.139.125:21737",
            "49.70.22.43:19465",
            "114.104.227.102:18477",
            "111.72.144.127:18708",
            "182.34.103.41:19436",
            "183.166.148.170:21741",
            "115.152.233.141:16680",
            "223.241.116.46:19729"
        ]
    }
}
```

### 修改解析方法 read_ip
```python
def read_ip(self):
    resp = requests.get(self.config['proxy']).json()
    proxy_list = resp.get("data", {}).get("proxy_list", [])
    return proxy_list
```
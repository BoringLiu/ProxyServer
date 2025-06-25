# ProxyServer

将短效代理API构建成随机可检测的隧道代理池
方案基于https://github.com/ThinkerWen/ProxyServer的改进
<br>

## 介绍
此项目用于将API代理构建成隧道代理池，即通过一个IP和端口来自动随机取出代理池中的IP.

API代理即：
* 通过API来获取代理，返回值是代理列表包含1~n个代理
* 例如：`[{"ip":"xxx.xxx.xxx.xxx","port":xx},{"ip":"xxx.xxx.xxx.xxx","port":xx}]`

通过本项目处理后可达成随机挑选代理并直接发起请求
无需在调用的项目中对API代理进行二次处理

<br>

## 安装使用
（项目中的示例使用的是[小象代理](https://www.xiaoxiangdaili.com/)
go get ProxyServer    # 下载依赖项
go build ProxyServer  # 编译可执行程序
./ProxyServer         # 运行
```

<br>

## 使用
在后台启动本项目后，只需配置代理地址为`127.0.0.1:12315`即可（在[配置文件](#配置文件)中可修改），以下用Python做示例：
```python
import requests

proxies = {
    "http": "http://127.0.0.1:12315",
    "https": "http://127.0.0.1:12315"
}
requests.get("http://example.com", proxies=proxies)
```

<br>

## 配置文件
配置文件一般默认即可
```json
{
  "bind_ip": "",                  # 代理服务绑定IP
  "bind_port": 12315,             # 代理服务绑定端口
  "proxy_max_retry": 10,          # 代理IP重试次数（超过后此次请求废弃）
  "proxy_expire_time": 50,        # 代理IP过期时间（超过后此IP移除代理池）
  "proxy_pool_length": 50,        # 代理IP池大小
  "proxy_connect_time_out": 1,    # 代理IP连接超时时间（超过后此IP移除代理池）
  "check_proxy_time_period": 10,  # 代理IP有效性检测间隔
  "refresh_proxy_time_period": 10 # API代理的获取间隔
}
```
# Agent API

## 指标写入 API 规格

OpsMind 支持以http/https （取决于部署的系统是否配置了https证书，具体视情况而定）的协议调用指标写入API：

请求规格为：

```text
POST http(s)://<host>:<port>/metrics/job/<job_name>
[Authorization: Basic <token>]
<metrics body>
```

其中`<metrics body>`为请求的Body部分，Body中的指标数据格式如下：

```text
<metric_name>{<label_name>="<label_value>", …} <metric_value> [<timestampMs>]
<metric_name>{<label_name>="<label_value>", …} <metric_value> [<timestampMs>]
…
```

其中：

1. `<metric_name>`**必须**，为指标项名称，只能由 **字母和下划线** 组成
2. `<label_name>`**必须**，为维度标签名称，只能由 **字母和下划线** 组成
3. `<label_value>`**必须**，为字符串类型的维度值
4. `<metric_value>`**必须**，为浮点数数值
5. `<timestampMs>`可选，为该行指标数据时间戳（毫秒），不指定则**按服务端收取时间**为准

一个合法的指标数据格式（不带时间戳）如下：

```text
process_count{} 800
cpu_usage{cpu="cpu0", mode="idle"} 12345.6
cpu_usage{cpu="cpu1", mode="idle"} 12345.6
mem_usage{mode="free"} 4000
mem_usage{mode="used"} 5000
```

一个合法的指标数据格式（带时间戳）如下：

```text
process_count{} 800 1534391166
cpu_usage{cpu="cpu0", mode="idle"} 12345.6 1534391166
cpu_usage{cpu="cpu1", mode="idle"} 12345.6 1534391166
mem_usage{mode="free"} 4000 1534391166
mem_usage{mode="used"} 5000 1534391166
```

需要注意的是：

1. 相同名称的 metric，维度的种类需保持一致
2. 如果是调用了 Opsmind Agent 的指标上报 API，那么无需在指标项中额外指定服务器维度，因为 Opsmind Agent 会自动给所有收取到的指标数据加上Host这个维度，维度值为 Agent 所在服务器的服务器名称

### 鉴权方式

若请求 Agent 的数据上报 API，则无需添加鉴权头部。

若请求的是服务端的数据上报 API，则需以标准的 HTTP Basic Auth 格式添加鉴权头部：

1. 由 OpsMind 系统管理员提供出特定权限（只允许调用指标写入 API）的用户名“user”和密码“password”
2. 在 API 请求头部中加入： Authorization: Basic base64\($user:$password\) 即可

其中 base64\($user:$password\) 过程请参考：[https://zh.wikipedia.org/wiki/HTTP%E5%9F%BA%E6%9C%AC%E8%AE%A4%E8%AF%81](https://zh.wikipedia.org/wiki/HTTP%E5%9F%BA%E6%9C%AC%E8%AE%A4%E8%AF%81)




---
description: 为了让外部既有系统能够调用 OpsMind API，需要生成响应的鉴权 token。
---

# API 鉴权 Token

## 获取命令行工具

{% hint style="info" %}
当前版本仅支持 Linux 操作系统。
{% endhint %}

下载最新版的 [OpsMind 命令行工具](http://dog.opsmind.com/duck)。

## 初始化、登录命令行工具

```
$ duck admin conn https://xxx.opsmind.com/api https://xxx.opsmind.com/auth
$ duck login <username>
...(print password)
$ duck org list
$ duck org switch <ORG>
```

## 生成公私钥对

```
$ duck key make -a <acl-name> <key-name>
Make sure to copy your new user key now. You won’t be able to see it again!
public: MFwwDQYJK...M10CAwEAAQ==
private: MIIBOwIBAAJBAMzBd...WcmWqDN5h3fWguY3iyg==
```

其中 &lt;acl-name&gt; 可选择以下之一：

1. prom \(指标数据权限\)
2. read-only \(只读权限\)
3. all \(所有权限\)

&lt;key-name&gt; 为此次生成的公私钥对名称（后续需要使用）。

{% hint style="info" %}
出于安全，OpsMind 不会保存生成的私钥，请妥善保存命令打印出来的私钥。
{% endhint %}

## 生成 Basic Auth Token

```text
$ duck key token -d <duration> <key-name> <private>
```

这一步使用上一步生成的密钥签出一个具有有效期的 token，其中：

1. &lt;duration&gt; 为此 token  有效时长，例如 "24h"
2. &lt;key-name&gt; 为公私钥对的名称（上一步指定）
3. &lt;private&gt; 为上一步产生的私钥

命令会输出形如下方的信息：

```text
3w4l42onfhm6g:eyJhbGciOiJSUzI1NiIsImtuflIjoicHJhfdha7fdkdWVyaWVyIiwidHlwIjoiSldUIn0.eyJleHAiOjE4NTc0NTYwMzB9.bCSq_FW5yXWVWfXJbIx6E0ogtcy1j9HLgH20fjdO0sdfvujkhF5PqKx3F_DfXCe-SKVuvQ_kXshd_lBSUwUx7NQ
```

其中 **冒号分割的第一部分（**3w4l42onfhm6g**）**为 Basic Auth 中的_**用户名**_，**冒号分割的第二部分**（eyJhbGciOi..wUx7NQ）为 Basic Auth 的_**密码**_。

一个使用上方 token 的 curl 命令示例：

```text
$ curl -u '3w4l42onfhm6g:eyJhbGciOiJSUzI1NiIsImtuflIjoicHJhfdha7fdkdWVyaWVyIiwidHlwIjoiSldUIn0.eyJleHAiOjE4NTc0NTYwMzB9.bCSq_FW5yXWVWfXJbIx6E0ogtcy1j9HLgH20fjdO0sdfvujkhF5PqKx3F_DfXCe-SKVuvQ_kXshd_lBSUwUx7NQ' https://xxx.opsmind.com/api/v1/hosts/list
```




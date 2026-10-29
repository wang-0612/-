---
{"dg-publish":true,"permalink":"/计算机相关技术知识库/05-Web安全/网站指纹识别与 CDN/","title":"网站指纹识别与 CDN","tags":["Web安全/信息收集"],"dg-note-properties":{"title":"网站指纹识别与 CDN","created":"2026-10-19","tags":["Web安全/信息收集"],"aliases":["指纹识别","whatweb","CDN 识别"]}}
---


# 网站指纹识别与 CDN

> [!warning] 合规声明
> 指纹识别与 CDN 探测仅可在授权目标上进行。

## 什么是 Web 指纹

在 Web 渗透中，**指纹识别**用于判断目标使用的 CMS、Web 组件及其版本。准确获取这些信息可以：

- 快速验证已知漏洞
- 判断是公开 CMS 还是二次开发
- 决定后续的代码审计与测试方向

### 指纹识别的对象

| # | 对象 | 示例 |
|---|------|------|
| 1 | CMS 信息 | 大汉 CMS、织梦、帝国 CMS、phpCMS、ECShop |
| 2 | 前端技术 | HTML5、jQuery、Bootstrap、Pure、Ace |
| 3 | Web 服务器 | Apache、Lighttpd、Nginx、IIS |
| 4 | 应用服务器 | Tomcat、JBoss、WebLogic、WebSphere |
| 5 | 开发语言 | PHP、Java、Ruby、Python、C# |
| 6 | 操作系统 | Linux、Windows Server 2008、Windows 7、Kali、CentOS |
| 7 | CDN 信息 | Cloudflare、360 CDN、365cyd、云加速 |
| 8 | WAF 信息 | Topsec、安全狗、云盾 |
| 9 | IP 与域名 | IP、域名注册信息、服务商 |
| 10 | 端口信息 | 软件或平台开放的常见端口 |

## 识别方式

| 方式 | 说明 | 示例 |
|------|------|------|
| 网站特有文件 | 特定 CMS 独有的静态文件路径 | `/templets/default/style/dedecms.css` → dedecms |
| 网站独有文件的 MD5 | 如 favicon.ico 的哈希（可被修改，不总是准确） | `certutil -hashfile favicon.ico MD5` |
| 文件命名规则 | 特定 CMS 的命名习惯 | — |
| 返回头关键字 | 响应头中的特征字段 | `rememberMe=deleteMe` → Shiro |
| 网页关键字 | HTML 中的特征字符串 | `Powered by Discuz!` |
| URL 特征 | 特定路由格式 | — |
| 网站路径特征 | 固定目录结构 | `/data/sessions/index.html` → dedecms |
| 网站静态资源 | 独有的 JS / CSS 路径 | — |
| 爬虫目录信息 | 爬取整站后归纳特征 | — |

## whatweb

Kali 自带的命令行指纹识别工具：

```bash
whatweb 域名          # 单个域名识别
```

示例输出：

```text
https://shop.zszlm.cn/ [200 OK]
Cookies[h6Dz_2132_lastact,h6Dz_2132_lastvisit,...],
Country[CHINA][CN], Email[admin@admin.com], HTTPServer[nginx],
HttpOnly[h6Dz_2132_saltkey], IP[120.77.254.208], JQuery,
Meta-Author[Discuz! Team and Comsenz UI Team],
MetaGenerator[Discuz! X3.4], PHP[5.6.40], PoweredBy[Discuz!],
Script[text/javascript], Title[论坛 - Powered by Discuz!],
X-Powered-By[PHP/5.6.40], nginx
```

从输出可直接读出：Nginx + PHP 5.6.40 + Discuz! X3.4。

## 在线与插件工具

| 工具 | 地址 / 说明 |
|------|------------|
| 在线指纹识别 | http://whatweb.bugscaner.com/ |
| Wappalyzer | 浏览器插件，实时识别网站技术栈 |

## CDN 识别

### 什么是 CDN

CDN（Content Delivery Network，内容分发网络）构建在现有网络之上，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等模块，让用户**就近获取内容**，降低网络拥塞、提高响应速度和命中率。

基本原理：广泛部署缓存服务器到用户集中的地区，用户访问时通过全局负载技术指向距离最近且工作正常的缓存服务器，由缓存直接响应。

> [!important] 对渗透的影响
> 如果目标使用了 CDN，直接解析到的 IP 是 CDN 节点而非真实服务器。**必须先找到真实 IP**，端口扫描和漏洞测试才有意义。

### 识别方法

```bash
nslookup 域名
```

| 方法 | 工具 / 地址 |
|------|------------|
| 多地 Ping | https://ping.chinaz.com/ 、https://tools.ipip.net/ping.php |
| 在线识别 | https://www.cdnplanet.com/tools/cdnfinder |
| 示例目标 | royalapps.com、taobao.com |

判断思路：

- 多地 ping 同一域名，若返回**多个不同 IP** 或 IP 属于 CDN 厂商网段，通常使用了 CDN
- `nslookup` 得到 CNAME 指向 CDN 厂商域名（如 `*.cdn.cloudflare.net`）也是明显特征
- 在线工具可直接给出 CDN 厂商与节点信息

## 相关笔记

- [[计算机相关技术知识库/05-Web安全/信息收集总览\|信息收集总览]] — 信息收集整体方向
- [[计算机相关技术知识库/05-Web安全/域名信息收集\|域名信息收集]] — 域名与子域名
- [[计算机相关技术知识库/05-Web安全/端口扫描\|端口扫描]] — 确认真实 IP 后再扫端口
- [[计算机相关技术知识库/05-Web安全/目录信息收集\|目录信息收集]] — 按指纹选择目录字典
- [[计算机相关技术知识库/05-Web安全/网络空间搜索引擎语法\|网络空间搜索引擎语法]] — 用 FOFA 等查指纹与资产
- [[计算机相关技术知识库/04-速查表/信息收集速查\|信息收集速查]] — 工具速查
- [[计算机相关技术知识库/04-速查表/搜索语法速查\|搜索语法速查]] — Google / FOFA 语法速查
- [[计算机相关技术知识库/05-Web安全/Web 渗透测试概览\|Web 渗透测试概览]] — 渗透流程总览

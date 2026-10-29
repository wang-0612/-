---
{"dg-publish":true,"permalink":"/好用软件/Rayburst 使用教程/","title":"Rayburst 使用教程","tags":["软件/下载工具"],"dg-note-properties":{"title":"Rayburst 使用教程","created":"2026-10-16","tags":["软件/下载工具"],"aliases":["Rayburst 下载器"]}}
---


# Rayburst 使用教程：开源下载器的安装、界面与常用功能

基于 Rayburst v4.0.1（2026 年 10 月）· 每个步骤都标了截图位置

## 〇、写在前面：Rayburst 是什么

Rayburst 是一款开源下载器（下载管理器），前身是 Motrix Next，目前由作者 AnInsomniacy 维护，采用 MIT 许可证，代码和发布包都托管在 GitHub 上。技术栈是 Tauri 2（Rust 后端）+ Vue 3 / TypeScript 前端，支持 macOS、Windows、Linux 三个平台，下载引擎是作者自己维护的 aria2 分支 aria2-next。

它能做的事可以归成几类：

- 普通文件下载：HTTP / HTTPS / SFTP，多线程分片加速
- BT 相关：BitTorrent、磁力链接（magnet:）、.torrent 文件、ED2K
- 流媒体：HLS / DASH，可自选轨道、录制直播、输出 MP4 / MKV
- 浏览器联动：配合 Rayburst Connect 扩展接管浏览器下载
- 桌面集成：托盘、通知、完成后关机、下载时阻止休眠等

界面自带简体中文，一共 27 种语言，首次启动会按系统语言自动切换。

一个容易踩的坑写在最前面：Rayburst 是 Motrix Next 改名后的新项目，安装之前建议先把旧的 Motrix Next 卸载掉，两者的设置、任务和下载历史并不互通（已经下载好的文件不受影响）。

本文基于 Rayburst v4.0.1 编写，界面细节可能随版本更新变化，以你实际安装的版本为准。文中所有功能描述都来自项目官方 README 与 docs 文档，链接列在文末。

![[rayburst-01.png]]
*图 1 Rayburst 的 GitHub 仓库首页*

## 一、下载与安装：只从官方渠道

### 1.1 官方渠道只有两个

- GitHub Releases：https://github.com/AnInsomniacy/rayburst/releases
- 官方网站：https://rayburst.pages.dev

请不要使用来路不明的“绿色版”“汉化版”安装包。下载器这类工具权限比较高（要读写磁盘、要开监听端口），第三方改过的包风险很大。

![[rayburst-02.png]]
*图 2 Releases 页面的安装包列表*

### 1.2 选对安装包

| 平台 | 下载这个文件 |
| --- | --- |
| Windows（绝大多数电脑） | Rayburst_x.x.x_x64-setup.exe |
| Windows（骁龙等 ARM 设备） | Rayburst_x.x.x_arm64-setup.exe |
| macOS（Apple 芯片 M 系列） | Rayburst_x.x.x_aarch64.dmg |
| macOS（Intel 芯片） | Rayburst_x.x.x_x64.dmg |
| Linux（Debian / Ubuntu） | Rayburst_x.x.x_amd64.deb |
| Linux（Fedora / RHEL） | Rayburst-x.x.x-1.x86_64.rpm |
| Linux（其他发行版） | Rayburst_x.x.x_amd64.AppImage |

拿不准就选 x64。Windows 上可以在「设置 → 系统 → 关于」里看「系统类型」，写着“基于 x64”就选 x64 版。

![[rayburst-03.png]]
*图 3 确认自己的系统架构*

### 1.3 Windows 安装

双击下载好的 setup.exe，跟着向导点下一步就行，大约十秒装完，不需要重启电脑。

![[rayburst-04.png]]
*图 4 Windows 安装向导*

![[rayburst-05.png]]
*图 5 安装完成后从开始菜单启动*

### 1.4 Linux 安装（三种方式）

Debian / Ubuntu：

```bash
sudo dpkg -i Rayburst_x.x.x_amd64.deb
```

Fedora / RHEL：

```bash
sudo rpm -i Rayburst-x.x.x-1.x86_64.rpm
```

其他发行版直接用 AppImage：

```bash
chmod +x Rayburst_x.x.x_amd64.AppImage
./Rayburst_x.x.x_amd64.AppImage
```

另外，FlatPark（社区维护、开发者认可的 Flatpak 源）也提供安装包：

```bash
flatpak remote-add --if-not-exists flatpark https://dl.flatpark.org/flatpark.flatpakrepo
flatpak install flatpark dev.aninsomniacy.rayburst
```

注意两点：Flatpak 版不能用应用内的更新功能，需要用 flatpak update dev.aninsomniacy.rayburst 更新；它也不注册浏览器的 Native Messaging 主机，要用浏览器扩展的话得先手动启动 Rayburst。

### 1.5 安装包报警怎么办

Rayburst 的 macOS 版本用的是 ad-hoc 签名、没有做公证；Windows 的正式签名是可选项（走 SignPath 流程）。所以浏览器或系统可能会提示“来源不明”。这种情况下可以核对发布签名——每个发布包都配了 .sig 文件，先把签名从 base64 转成 minisign 格式，再验证：

```bash
python3 -c 'import base64,sys; sys.stdout.write(base64.b64decode(sys.stdin.read()).decode())' < Rayburst_x.x.x_<文件名>.sig > Rayburst_x.x.x_<文件名>.minisig
minisign -V -m Rayburst_x.x.x_<文件名> -x Rayburst_x.x.x_<文件名>.minisig -p rayburst.pub
```

看到 Signature and comment signature verified 就说明文件没被动过。补充一句：.sig 是 Tauri 更新器的签名，不是 GPG 签名，要用 minisign 验证；公钥在 src-tauri/tauri.conf.json 的 plugins.updater.pubkey 里，base64 解码后存成 rayburst.pub 即可。官方所有发布包都是 GitHub Actions 自动构建的，也可以直接按 README 从源码编译。

### 1.6 为什么没有便携版

官方 FAQ 解释得很清楚：Rayburst 的下载引擎 aria2-next 是运行时启动的 sidecar 子进程，引擎二进制必须和主程序放在一起，没法塞进单个 exe；magnet://、ed2k://、thunder:// 这些深链接和 .torrent 文件关联需要写注册表，只有安装器能做；自动更新也需要一个固定的安装路径。所以官方只提供安装版。

## 二、界面导览

打开之后，主界面分成四块：

- 左侧导航栏：Tasks（任务）下面是 All、In Progress、Failed、Completed 四个筛选，每条后面带数量徽标；左下角是 About 和 Settings。
- 顶部工具栏：新建任务（＋）、排序、刷新、全部开始、全部暂停、全部停止、清空列表，集中在内容区右上角。
- 任务列表：每个任务显示文件名、进度和实时速度；内容区左上角显示当前筛选名（比如 All）；没有任务时会显示一张浅色的标志水印。
- 底部分页控件：默认每页 20 条。

![[rayburst-06.png]]
*图 6 Rayburst 主界面分区（自己加编号标注）*

![[rayburst-07.png]]
*图 7 下载中的任务列表*

## 三、添加下载任务

### 3.1 直接粘贴链接

点顶部工具栏的 ＋，把 HTTP / HTTPS 链接粘贴进去，选好保存位置，确认即可。

![[rayburst-08.png]]
*图 8 新建任务窗口*

![[rayburst-09.png]]
*图 9 选择保存目录与文件名*

### 3.2 种子与磁力

安装版会注册 magnet:、ed2k:、thunder: 协议和 .torrent 文件关联，所以：

- 双击 .torrent 文件 → 自动用 Rayburst 打开
- 在浏览器里点磁力链接 → 直接交给 Rayburst

BT 任务支持选择性下载（只勾需要的文件），引擎侧支持 DHT、Peer Exchange、加密控制等。

![[rayburst-10.png]]
*图 10 BT 任务的选择性下载*

### 3.3 浏览器扩展 Rayburst Connect

扩展已经上架 Chrome 应用商店、Microsoft Edge 加载项和 Firefox 附加组件，也可以从它的 GitHub Releases 下载 zip 手动加载。使用前提是：桌面端 Rayburst 4 或更新版本，并且在扩展设置里填好桌面端 Extension API 的端口和密钥。

装好之后可以做到这些事：

- 浏览器下载自动转给 Rayburst 多线程下载，支持按站点、文件类型、文件体积等条件过滤
- 右键点击链接 / 图片 / 音视频 → 「Download with Rayburst」
- 按住 Alt 单击链接可以这次放过，仍然交给浏览器自己下载
- 扩展弹窗里能实时看到上传/下载速度和任务列表，支持暂停、继续、打开所在文件夹
- 磁力链接与 .torrent 链接也会被接住并转给桌面端
- 默认开启 Cookie 转发，需要登录才能下载的文件也能正常下载
- 可选隐藏 Chrome 自带的下载栏（Chromium 115 以上，Firefox 不支持）

![[rayburst-11.png]]
*图 11 Rayburst Connect 的商店安装页*

![[rayburst-12.png]]
*图 12 在扩展里配置 API 端口与密钥*

![[rayburst-13.png]]
*图 13 扩展弹窗的实时仪表盘*

![[rayburst-14.png]]
*图 14 右键菜单入口*

## 四、常用设置

点左下角的 Settings 进入。下面几组是日常最常用的（不同版本的界面分组名称可能略有出入，以你安装的版本为准）。

### 4.1 并发与连接

可以分别设置同时进行的任务数、单个任务的连接数、BT 对等连接数。下载慢的时候主要调这里，不过不要盲目拉满——连太多反而容易触发对方服务器限速。

### 4.2 速度限制与时段排程

支持全局和单任务的上传 / 下载限速，还可以按「星期 + 时间段」排程。比如白天限速不影响别人上网，夜里自动放开跑。

![[rayburst-15.png]]
*图 15 速度限制与时段排程设置*

### 4.3 下载组织

收藏目录与最近目录、可选按文件类型自动分类、完成历史（存在本地 SQLite 数据库里）、过期记录清理。

### 4.4 网络与代理

可以给「下载」「应用更新」「Tracker 更新」分别设置作用域代理，也支持自动探测系统代理。开着代理软件下载失败时，先来这里确认一下。

### 4.5 桌面集成与轻量模式

托盘图标（可选显示实时速度）、macOS 的 Dock 进度与徽标、magnet / ed2k / thunder 协议处理都在这里。另外有个「轻量模式」：最小化到托盘时会销毁 WebView 前端，只保留 Rust 侧的下载引擎、任务监控、通知和历史，常驻后台时内存占用更低。

![[rayburst-16.png]]
*图 16 托盘菜单*

### 4.6 通知与电源

任务开始 / 完成 / 失败都会有系统原生通知；可以设置下载期间阻止电脑休眠；还能设置「全部下载完成后自动关机」，挂机下载很实用。

### 4.7 外观与语言

浅色 / 深色 / 跟随系统三选一，10 套配色方案，27 种语言，首次启动按系统语言自动选择。

![[rayburst-17.png]]
*图 17 主题与配色设置*

### 4.8 更新与诊断

更新通道分 Stable（稳定）、Beta（测试）、Latest 三种，并且把「下载」和「安装」分成两个阶段，不会在你正下载文件的时候突然打断。诊断方面可以查看结构化日志、一键导出诊断 ZIP（提 Issue 时很有用）、以及检查数据库完整性。

![[rayburst-18.png]]
*图 18 更新通道设置*

## 五、媒体下载（HLS / DASH）

Rayburst 支持 HLS 和 DASH 流媒体：可以选择原生轨道、加载字幕、录制直播、断点续传，最终输出 MP4 或 MKV 文件。

![[rayburst-19.png]]
*图 19 HLS / DASH 的轨道与字幕选择*

合规提醒：这个功能请只用于你有权保存的内容，例如自建的流媒体服务、你自己的直播回放、或者明确授权你下载的素材。拿它去抓取别人网站的付费内容，既违反平台规则也可能构成侵权，后果自负。

## 六、常见问题

1. macOS 提示「应用已损坏，无法打开」

这不是安装包坏了。官方 FAQ 给的做法是打开「终端」执行下面这行，每次升级后要重新执行一次：

```bash
xattr -dr com.apple.quarantine /Applications/Rayburst.app
```

这条命令只是移除 Gatekeeper 给下载文件打的隔离标记。

2. 浏览器或杀毒软件报警

因为安装包没有（或可选）正式签名。按 1.5 的方法用 .sig + minisign 校验一下，确认文件来自官方发布即可。

3. Linux 里窗口空白、画不出来

官方 docs/LINUX.md 有专门说明（主要和 GPU 渲染回退有关），遇到问题先看那份文档。

4. 从 Motrix Next 升级过来

先卸载旧版再装 Rayburst；设置、任务和下载历史不会导入（用应用内更新可能会出现两个应用并存、或者残留旧快捷方式的情况）。已经下载好的文件不受影响，保留即可。

5. 下载没速度

按顺序检查：任务是不是被暂停了、并发和连接数是不是设得很低、有没有开着代理、限速时段是不是正在生效。

6. 更新通道怎么选

日常使用选 Stable；想尝鲜再用 Beta 或 Latest，但要有遇到新问题的心理准备。

7. 想给作者反馈问题

项目欢迎提 PR 和 Issue。提 Issue 之前可以用设置里的「导出诊断 ZIP」把日志一起发上去，定位问题会快很多。

## 七、合规与版权说明

- 下载渠道：请只从 GitHub Releases 或官方网站获取安装包。第三方修改过的安装包不在本文讨论范围内，也不建议使用。
- 软件许可：Rayburst 采用 MIT 许可证，Copyright (c) 2025-present AnInsomniacy。引用软件本身请遵守其许可证条款；转载请注明本文出处。
- 产品名称：按项目品牌规范，产品名 Rayburst 不做翻译，官方中文口号是「重新定义开源下载器」。
- 使用边界：下载器只是工具。请只用它下载你有权获取的内容——自己的文件、公开授权的资源、合规分发的镜像与种子等，不要用它下载或传播未获授权的内容。
- 本文的功能描述与数据来自项目官方 README、docs/BRAND.md 与 Rayburst Connect 文档，文中所有截图均为作者本人实机截取。

## 八、参考链接

- 项目仓库：https://github.com/AnInsomniacy/rayburst
- 官方网站：https://rayburst.pages.dev
- 下载引擎 aria2-next：https://github.com/AnInsomniacy/aria2-next
- 浏览器扩展 Rayburst Connect：https://github.com/AnInsomniacy/rayburst-connect
- 项目文档目录：https://github.com/AnInsomniacy/rayburst/tree/main/docs
- MIT 许可证原文：https://github.com/AnInsomniacy/rayburst/blob/main/LICENSE

<div align="center">

<img src="assets/icon.png" alt="mail2api" width="96" height="96">

# mail2api 桌面版

### 把一堆邮箱变成一个统一的收信 API 和一个像邮件客户端一样的控制台

**导入邮箱凭据，应用密码自动开通，IMAP 自动轮询，最新邮件、验证码、订阅续费一处查看——不用再配环境变量、不用自己守护进程。**

[![Version](https://img.shields.io/github/v/release/dofastted/mail2api-releases?color=blue&label=version)](https://github.com/dofastted/mail2api-releases/releases)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgrey.svg)](https://github.com/dofastted/mail2api-releases/releases)
[![Built with Tauri](https://img.shields.io/badge/built%20with-Tauri%202-orange.svg)](https://tauri.app/)
[![Downloads](https://img.shields.io/github/downloads/dofastted/mail2api-releases/total)](https://github.com/dofastted/mail2api-releases/releases/latest)

**[下载](#下载与安装) · [快速开始](#快速开始) · [功能](#功能) · [常见问题](#常见问题) · [反馈问题](https://github.com/dofastted/mail2api-releases/issues)**

</div>

## ❤️赞助

> 如果 mail2api 帮你省下了时间，欢迎请作者喝杯咖啡，你的支持会让它持续更新。

<details open>
<summary>点击收起</summary>

<div align="center">

<img src="assets/sponsor-alipay.jpg" alt="支付宝赞助二维码：快乐编程" width="260">

**支付宝扫码赞助**（支持信用卡 / 花呗）

</div>

</details>

## 为什么用 mail2api？

手里有几十上百个 Gmail / Outlook / QQ / 企业邮箱，要收验证码、看账单、追续费，每个都去网页登录一遍既慢又容易被风控；自己写脚本又要处理应用密码、2FA、代理、IMAP 断线重连。

**mail2api** 把这些收进一个桌面应用：粘贴一份凭据清单，剩下的交给它。

- **一次导入，自动开通** — `账号 | 密码 | 2FA | 辅助邮箱 | 代理` 一行一个，Gmail / Outlook 用每个账号自己的浏览器环境自动创建应用密码，只有遇到验证码等真正需要人的环节才请你出手
- **每个邮箱独立出口** — 每个账号绑定自己的代理（SOCKS5 / HTTP），浏览器和 IMAP 都走同一条线路，邮箱之间从不共用 IP
- **像邮件客户端一样看信** — 安全沙箱渲染 HTML 邮件、屏蔽追踪器、一键复制验证码、附件下载、单封邮件分享链接
- **统一 REST API** — `GET /api/v1/mailboxes/{id}/latest` 拿到最新邮件，脚本和自动化直接接入
- **转发与提醒** — 新邮件可镜像到一个统一 IMAP 邮箱、推送到 Telegram；自动识别订阅账单并提醒续费
- **数据只在本机** — 所有凭据用本机生成的密钥加密存储，密钥保存在系统钥匙串
- **跨平台原生应用** — Windows、macOS、Linux，基于 Tauri 2，自动更新

## 下载与安装

### 系统要求

- **Windows**：Windows 10 及以上（x64）
- **macOS**：macOS 11 (Big Sur) 及以上，Apple 芯片与 Intel 均支持
- **Linux**：x86_64，glibc 2.35+ 与 WebKitGTK 4.1，例如 Ubuntu 22.04+、Debian 12+、较新的 Fedora
- **Google Chrome**：自动开通应用密码时需要（装在系统默认位置即可，会自动找到）

### Windows 用户

从 [Releases](https://github.com/dofastted/mail2api-releases/releases/latest) 下载 `mail2api_{版本}_x64-setup.exe`，双击安装（安装到当前用户，无需管理员权限）。

### macOS 用户

从 [Releases](https://github.com/dofastted/mail2api-releases/releases/latest) 下载对应芯片的安装包：

- Apple 芯片（M1 及以上）：`mail2api_{版本}_aarch64.dmg`
- Intel：`mail2api_{版本}_x64.dmg`

打开 dmg，把 mail2api 拖进「应用程序」。

> **提示**：在 Apple 签名公证完成之前，首次打开若提示「无法验证开发者」，请在「应用程序」中右键 mail2api →「打开」，或执行：
>
> ```bash
> xattr -dr com.apple.quarantine /Applications/mail2api.app
> ```

### Linux 用户

从 [Releases](https://github.com/dofastted/mail2api-releases/releases/latest) 下载 `mail2api_{版本}_amd64.AppImage`：

```bash
chmod +x mail2api_*_amd64.AppImage
./mail2api_*_amd64.AppImage
```

AppImage 会在应用内自动更新到新版本。

## 快速开始

1. **首次启动**：mail2api 会为这台电脑生成加密密钥、API Token 和管理员 2FA 密钥，存入系统钥匙串
2. **保存密钥备份**：按提示把 `mail2api-secrets.json` 保存到离线、安全的位置——**丢了它，已保存的邮箱凭据将无法解密**
3. **绑定管理员 2FA**：用验证器 App（Google Authenticator、1Password 等）扫描页面上的二维码
4. **进入控制台**：输入验证器上的 6 位验证码登录（一次登录保持 30 天）
5. **导入邮箱**：粘贴凭据清单，每行一个账号，例如

   ```text
   user@gmail.com | 邮箱密码 | 2FA密钥 | 辅助邮箱 | socks5h://用户名:代理密码@1.2.3.4:1080
   ```

6. 等邮箱状态变成「轮询中」，新邮件就会出现在中间的邮件列表里

> **提示**：关闭窗口只会隐藏到托盘，mail2api 继续在后台收信；要彻底退出，请在托盘菜单里点「退出 mail2api」。

## 功能

| 功能 | 说明 |
| --- | --- |
| 邮箱类型 | Gmail、Outlook、QQ 邮箱、任意 IMAP 邮箱 |
| 应用密码开通 | Gmail / Outlook 自动；QQ 邮箱需短信，走人工协助 |
| 人工协助 | 遇到验证码、手机确认时保留浏览器窗口，你处理完自动继续 |
| 代理 | 每邮箱独立代理、代理池、批量检测、出口 IP 与地区识别 |
| 浏览器环境 | 每邮箱独立浏览器资料、语言 / 时区 / 地理位置 / UA、扩展中心 |
| 收信 | IMAP 轮询、垃圾箱、附件、验证码识别、单封分享 |
| 转发 | 统一 IMAP 邮箱镜像、Telegram 推送与机器人命令 |
| 订阅 | 账单邮件识别、续费日期推算、Telegram 续费提醒 |
| 安全 | 凭据加密存储、管理员 2FA、审计日志、数据库定时快照 |
| 桌面体验 | 托盘常驻、单实例、外链用系统浏览器打开、附件存到「下载」、自动更新 |

## 常见问题

<details>
<summary><b>数据存在哪里？</b></summary>

| | Windows | macOS | Linux |
| --- | --- | --- | --- |
| 数据 | `%LOCALAPPDATA%\com.mail2api.desktop` | `~/Library/Application Support/com.mail2api.desktop` | `~/.local/share/com.mail2api.desktop` |
| 日志 | 数据目录下 `logs` | `~/Library/Logs/com.mail2api.desktop` | 数据目录下 `logs` |
| 密钥 | 凭据管理器 `com.mail2api.desktop` | 钥匙串 `com.mail2api.desktop` | Secret Service，没有时为 `data/secrets.json`（仅本用户可读） |

</details>

<details>
<summary><b>换电脑或重装系统后怎么恢复？</b></summary>

把整个数据目录拷到新电脑的同一位置，启动后应用会提示「导入密钥备份」，选择首次运行时保存的 `mail2api-secrets.json` 即可。应用会先校验这份密钥能否打开现有数据，选错文件不会覆盖任何东西。

</details>

<details>
<summary><b>更新会不会丢数据？</b></summary>

不会。每次更新前会先在数据目录的 `backups/` 下生成一份校验过的数据库快照 `pre-update-<旧版本>.db`（保留最近 3 份），再停止服务、安装新版本。更新包都经过签名校验，被篡改的安装包会被拒绝。

</details>

<details>
<summary><b>能在脚本里调用吗？</b></summary>

可以。托盘菜单「复制 API Token」，然后：

```bash
curl -H "Authorization: Bearer <token>" "http://127.0.0.1:<端口>/api/v1/mailboxes/<account_id>/latest?limit=3"
```

端口可以在托盘「在浏览器中打开控制台」打开的地址里看到（从 18080 起）。

</details>

<details>
<summary><b>遇到问题怎么反馈？</b></summary>

请在 [Issues](https://github.com/dofastted/mail2api-releases/issues) 描述问题，附上系统版本和日志目录里的 `mail2api.log`、`desktop.log`（发送前请删掉其中的邮箱地址等个人信息）。

</details>

---

<div align="center">

本仓库只发布 mail2api 桌面版安装包与自动更新数据。

</div>

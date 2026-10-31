# Cloudflare Tunnel Manager

一个面向 Debian、Ubuntu 等 Linux 系统的 Cloudflare Tunnel 交互式管理脚本。

通过简单的菜单操作，即可完成 `cloudflared` 安装、Cloudflare 浏览器授权、Tunnel 创建、DNS 路由配置以及 systemd 服务管理。

无需手动编写复杂配置文件，让 Linux 服务器快速、安全地接入 Cloudflare Tunnel。

## ✨ 功能特性

- **官方软件源**：仅使用 Cloudflare 官方 APT 仓库安装 `cloudflared`。
- **签名验证**：通过 Cloudflare 官方 GPG 密钥验证软件包仓库。
- **自动环境检测**：自动识别 Linux 发行版和 CPU 架构。
- **浏览器授权**：通过 Cloudflare 官方网页登录授权。
- **自动创建 Tunnel**：交互式创建 Cloudflare Tunnel。
- **自动配置 DNS**：自动为指定域名创建 DNS 路由。
- **自定义回源地址**：支持输入本地服务地址及端口。
- **systemd 管理**：自动创建服务并设置开机启动。
- **服务管理**：支持启动、停止、重启和查看运行状态。
- **实时日志**：通过 journalctl 查看 Tunnel 运行日志。
- **在线更新**：通过官方 APT 仓库更新 cloudflared。
- **配置修改**：支持修改本地服务地址和绑定域名。
- **交互式菜单**：彩色终端界面，操作简单直观。

## 🖥️ 系统兼容性

| Linux 系统 | 支持情况 |
|---|---|
| Debian 11 / 12 / 13 | 支持范围内 |
| Ubuntu 20.04 / 22.04 / 24.04 | 支持范围内 |
| Linux Mint | 可尝试 |
| Kali Linux | 可尝试 |
| Pop!_OS | 可尝试 |
| Fedora / CentOS / Rocky Linux | 不支持 |
| Arch Linux / Alpine Linux | 不支持 |

**运行要求：**
- Linux 系统
- APT 软件包管理器
- systemd
- Root 权限
- 能够访问 Cloudflare 官方软件源

说明：Debian 和 Ubuntu 是主要目标系统。其他 Debian 衍生发行版虽然可能兼容，但未逐一完成实际测试。

## 📦 安装方法

### 1. 下载脚本

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/SunMoonWithYou/cf_tunnel/main/install.sh)
```

## 🚀 使用说明

运行脚本后，会显示交互式管理菜单。

### 创建 Tunnel

选择：

```text
1. 安装 cloudflared 并创建 Tunnel
```

脚本将自动完成以下操作：

1. 检测系统环境。
2. 添加 Cloudflare 官方 APT 软件源。
3. 导入官方 GPG 签名密钥。
4. 安装 cloudflared。
5. 启动 Cloudflare 浏览器授权流程。
6. 输入 Tunnel 名称。
7. 输入需要绑定的完整域名。
8. 输入本地服务地址。
9. 自动创建 Tunnel。
10. 自动配置 DNS 路由。
11. 生成 Tunnel 配置文件。
12. 创建 systemd 服务并设置开机启动。

### 浏览器授权

执行授权操作后，终端会显示 Cloudflare 官方授权链接。

将链接复制到本地电脑浏览器：

1. 登录 Cloudflare 账户。
2. 选择需要使用的域名。
3. 确认授权。
4. 返回服务器终端继续操作。

注意：执行授权的服务器 SSH 会话应保持开启，授权证书保存在服务器本地。

## 🎛️ 管理菜单

| 菜单 | 功能 |
|---|---|
| 1 | 安装 cloudflared 并创建 Tunnel |
| 2 | 修改本地服务地址 / 端口 |
| 3 | 修改绑定域名 |
| 4 | 启动服务 |
| 5 | 停止服务 |
| 6 | 重启服务 |
| 7 | 查看运行状态 |
| 8 | 查看实时日志 |
| 9 | 更新 cloudflared |
| 10 | 卸载本机服务 / 配置 |
| 0 | 退出程序 |

## 📁 配置文件

默认配置目录：

```text
/etc/cloudflared/
```

主要文件：

| 文件 | 说明 |
|---|---|
| `config.yml` | Tunnel 配置文件 |
| `UUID.json` | Tunnel 身份凭据 |
| `manager.conf` | 管理器状态配置 |

systemd 服务：

```text
/etc/systemd/system/cloudflared-custom.service
```

## 🔧 常用命令

查看服务状态：

```bash
systemctl status cloudflared-custom
```

启动服务：

```bash
systemctl start cloudflared-custom
```

停止服务：

```bash
systemctl stop cloudflared-custom
```

重启服务：

```bash
systemctl restart cloudflared-custom
```

查看实时日志：

```bash
journalctl -u cloudflared-custom -f
```

查看 cloudflared 版本：

```bash
cloudflared --version
```

## 🔐 安全说明

- 仅从 Cloudflare 官方 APT 仓库安装 cloudflared。
- 使用官方 GPG 密钥进行 APT 仓库签名验证。
- 不需要向第三方提供 Cloudflare 登录密码。
- Tunnel 授权证书和凭据保存在服务器本地。
- 建议限制 `/etc/cloudflared/` 目录及凭据文件的访问权限。
- 请勿公开分享 `cert.pem`、Tunnel JSON 凭据或其他授权文件。

## ⚠️ 注意事项

1. 使用前请确保域名已经添加到 Cloudflare。
2. Cloudflare Tunnel 需要服务器能够正常访问 Cloudflare 网络。
3. 本地服务必须已经运行，否则 Tunnel 虽然连接成功，访问网站仍可能失败。
4. 修改域名后，旧 DNS 记录不会自动删除，需要根据实际情况自行清理。
5. 卸载脚本只处理服务器本地服务和配置，不会自动删除 Cloudflare 账户中的 Tunnel 和 DNS 记录。
6. 本项目目前尚未完成所有发行版的实际兼容性测试。

## 🛠️ 技术栈

- Bash
- Cloudflare cloudflared
- Cloudflare Tunnel
- Cloudflare 官方 APT Repository
- GPG
- systemd
- journalctl

## 📄 许可证

本项目采用 MIT License 开源协议。

## ⭐ 支持项目

如果这个项目对你有帮助，欢迎给仓库点一个 Star ⭐

欢迎提交 Issue 和 Pull Request，一起完善项目。

---

**免责声明：** 本项目是第三方开源管理脚本，与 Cloudflare 官方没有隶属或合作关系。Cloudflare 和 Cloudflare Tunnel 是 Cloudflare, Inc. 的相关商标及产品。使用本项目产生的风险由使用者自行承担。

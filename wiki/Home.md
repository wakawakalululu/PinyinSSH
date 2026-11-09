# 春夏秋手机安全助手

在 Android 设备上运行完整的 SSH 服务，并通过标准 SSH 客户端管理设备、传输文件与远程运维。

## 项目简介

春夏秋手机安全助手 是一个**独立发布的二次开发版本（modified fork）**，在应用内运行完整的 `sshd` 服务器（基于 Dropbear），并提供 `rsync`、`sftp`/`scp` 文件传输能力。启动服务后，同一可信局域网内的电脑即可用标准 SSH 客户端登录设备、传输文件、执行备份。

在基础 SSH 之上，还内置了：

- **提权模式**：应用权限（普通）/ Root（su）/ ADB（无线调试）三种运行身份
- **内置终端**：在应用内以不同权限执行命令并实时查看输出
- **内网穿透**：托管官方 `frpc`，把本机 sshd 端口暴露到公网
- **Tailscale 组网**：托管官方 `tailscaled`，加入你自己的 tailnet
- **哪吒监控上报**：托管官方 `nezha-agent`，向面板汇报设备状态
- **远程 ADB 桥接**：独立 SSH 入口、公钥授权与命令前缀策略（默认关闭）
- **远程桌面**：屏幕共享与可选触控控制
- **WebDAV 只读访问**：通过 Android SAF 在系统文件选择器中浏览 HTTPS WebDAV 内容

> 完整功能、安装与配置说明见[完整中文说明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md)。

## 使用文档

- [安装与快速上手](Installation)
- [常见问题](FAQ)
- [构建与发布](Development)

## 项目入口

- [完整中文说明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md)
- [English README](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.en.md)
- [GitHub Pages](https://wakawakalululu.github.io/PinyinSSH/)
- [下载 Releases](https://github.com/wakawakalululu/PinyinSSH/releases)

## 安全提示

远程桌面等服务没有认证和 TLS，只能在可信的私有网络中使用，禁止直接暴露到公网。Root、ADB 权限和端口映射会显著扩大操作影响范围；请只授权可信密钥并确认网络边界。使用前请务必阅读[安全说明与免责声明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#安全说明与免责声明)。

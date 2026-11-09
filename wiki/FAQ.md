# 常见问题

## Releases 页面没有新 APK / 没有签名版本？

默认分支每次提交都会构建 APK；仅在构建通过且配置完整的签名密钥后，才会生成版本标签并发布 GitHub Release。维护者需要在仓库 Actions Secrets 中配置 `KEYSTORE_BASE64`、`KEYSTORE_PASSWORD`、`KEY_ALIAS` 和 `KEY_PASSWORD`。未配置或验签失败会阻止发布（`release` 任务的 `Require a signed release APK` 步骤会报错退出）。

## 为什么 Wiki 没有自动更新？

Wiki 同步由 Actions 完成，内容源是仓库 `wiki/` 目录。仓库需要配置 `WIKI_TOKEN` Secret，且该令牌必须能够写入仓库 Wiki；未配置时 Pages 仍会部署，但 Wiki 同步步骤会给出警告并跳过。详见[构建与发布](Development#配置-wikitoken)一节。

## SSH 无法连接

- 确认应用中的 SSH 服务仍在运行，并核对界面显示的 IP 与端口（默认 `2222`）；
- 确认电脑与手机处于同一网络，没有客户端隔离或防火墙阻断；
- 检查登录用户名、认证方式（密码 / 公钥 / 一次性密码）是否正确，以及 SSH 主机密钥变更提示；
- Android 进入省电或休眠时可能限制后台运行，可启用应用的前台服务通知或调整电池策略。

## 是否应启用 Root 或 ADB 模式？

一般不需要。保持普通应用权限即可；只有理解额外权限带来的风险并确有场景时，才启用 Root 或 ADB 模式。Root 模式下 SSH 会话即 root，任何能登录者等同获得整机权限；ADB 模式拥有 `shell` 用户能力（`pm`/`am`/`getprop` 等）。

## 为什么不能覆盖安装 / 升级旧版本？

旧版本与本版本使用不同的包名与签名，是两个独立应用；如需切换，请先卸载旧版本（其配置会一并清除）。同一签名下的版本升级则可正常覆盖安装。

## 服务启动失败或端口被占用？

- 在主界面 / 通知中核对监听地址是否与预期一致；
- 换一个端口（允许范围 `1024–32768`）；
- 检查是否有其他应用占用同一端口；小于 1024 的特权端口普通应用无法绑定。

## frpc 为什么建议在「服务器地址」里直接填 IP？

官方 Linux 版 `frpc` 是纯 Go 程序，在部分 Android 环境下读取不到系统 DNS 配置，会出现域名无法解析。直接填 frps 的 IP 即可绕过。详见[完整说明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#frpc-内网穿透配置教程)。

## 远程桌面安全吗？

不安全：该 HTTP（MJPEG）服务没有认证和 TLS，任何能访问端口的人都能查看屏幕，启用无障碍后还可能操控设备。**仅在可信私有网络使用，禁止映射到公网或用公开 frpc 转发。**

更多问题与排查见[完整 FAQ](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#常见问题-faq)。

# 常见问题

## Releases 页面没有新 APK

默认分支每次提交都会构建 APK；仅在构建通过且配置完整的签名密钥后，才会生成版本标签并发布 GitHub Release。维护者需要在仓库 Secrets 中配置 `KEYSTORE_BASE64`、`KEYSTORE_PASSWORD`、`KEY_ALIAS` 和 `KEY_PASSWORD`。

## 为什么 Wiki 没有自动更新？

Wiki 同步由 Actions 完成。仓库需要配置 `WIKI_TOKEN` Secret，令牌必须能够写入仓库 Wiki；未配置时 Pages 仍会部署，Wiki 同步步骤会给出警告并跳过。

## SSH 无法连接

- 确认应用中的 SSH 服务仍在运行，并检查设备 IP 地址与端口。
- 确认电脑与手机之间的网络没有客户端隔离或防火墙阻断。
- 检查登录用户名、认证方式及 SSH 主机密钥变更提示。
- Android 进入省电或休眠状态时，系统可能限制后台运行；可按需启用应用提供的前台服务通知。

## 是否应启用 Root 或 ADB 模式？

一般不需要。保持普通应用权限；只有理解额外权限带来的风险并确有使用场景时，才启用 Root 或 ADB 模式。

更多问题与排查说明见[完整 FAQ](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#常见问题-faq)。

# 安装与快速上手

## 系统要求

- Android 8.0 及以上（`minSdk 26`）
- 同一可信局域网内的电脑（用于 SSH 连接）
- 完整使用文件访问需手动授予「所有文件访问权限」（`MANAGE_EXTERNAL_STORAGE`）

## 安装

从 [Releases](https://github.com/wakawakalululu/PinyinSSH/releases) 下载最新签名 APK：

- 发布文件：`PhoneSecurityAssistant-<版本>.apk`（release）
- 调试文件：`PhoneSecurityAssistant-<版本>-debug.apk`（debug，包名 `com.tencentx.pinyin.debug`）

应用包名为 `com.tencentx.pinyin`。**切换到不同签名的版本前，请先备份数据**；与旧版本（不同包名/签名）互不兼容，不能直接覆盖安装升级，需先卸载旧版。

首次启动会请求必要权限；要完整使用 `rsync`/`sftp` 访问共享存储，必须手动授予「所有文件访问权限」，否则只能访问应用自身目录。

## 启动 SSH 服务

1. 在应用中设置登录用户名、密码或导入 SSH 公钥（`authorized_keys`）。
2. 选择权限模式：普通应用权限（默认、最小权限）；Root 或 ADB 模式仅在确有需要时启用。
3. 启动 SSH 服务，查看界面显示的设备地址与端口（默认端口 `2222`）。
4. 在同一可信网络的电脑上连接，例如：

   ```sh
   ssh -p 2222 your-user@phone-ip
   ```

5. 文件同步使用 SSH 传输（`rsync -e ssh` / `scp` / `sftp`），具体示例见[完整说明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#文件同步使用)。

## 验证主机密钥

首次连接时 SSH 客户端会提示主机密钥指纹，请核对后再确认。不要将密码、公钥授权或服务端口暴露给不可信网络。

## 权限模式

| 模式 | 运行身份 | 适用场景 |
| --- | --- | --- |
| 应用权限（普通） | 应用自身 UID | 默认；无需 Root，最安全 |
| Root 权限（su） | root | 需要系统级操作；请确认已 Root 并授权 |
| ADB 权限（无线调试） | adb `shell` 用户 | 无需 Root，需先配对无线调试 |

## 后台运行与保活

- 服务以后台前台服务运行，并显示常驻通知；
- 可在「设置 → 自启」开启「开机自启」「打开应用时自启」；
- 部分厂商系统需把应用电池策略设为「不受限制」，否则服务可能被回收；
- 可从系统快捷设置面板添加磁贴，一键启停服务。

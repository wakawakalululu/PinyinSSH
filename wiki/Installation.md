# 安装与快速上手

## 安装

从 [Releases](https://github.com/wakawakalululu/PinyinSSH/releases) 下载最新签名 APK。应用包名为 `com.tencentx.pinyin`；切换到不同签名的旧版本前，请先备份数据。首次使用文件传输功能时，按 Android 提示授予所需的文件访问权限。

## 启动 SSH 服务

1. 在应用中设置登录用户名、密码或导入 SSH 公钥。
2. 选择权限模式。普通应用权限是默认且最小权限的模式；Root 或 ADB 模式仅在确有需要时启用。
3. 启动 SSH 服务，并查看界面显示的设备地址和端口（默认端口为 `2222`）。
4. 在同一可信网络的电脑上连接，例如：

   ```sh
   ssh -p 2222 your-user@phone-ip
   ```

5. 文件同步使用 SSH 传输；具体示例参见[项目说明](https://github.com/wakawakalululu/PinyinSSH/blob/master/README.md#文件同步使用)。

首次连接时请核对 SSH 主机密钥指纹。不要将密码、公钥授权或服务端口暴露给不可信网络。

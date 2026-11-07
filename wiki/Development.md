# 构建与发布

## 本地构建

项目需要 JDK、Android SDK、NDK 与 CMake。按 [BUILDING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING) 配置环境后，可运行：

```sh
./gradlew testDebugUnitTest lintDebug assembleDebug assembleRelease
```

完整 CI 依赖、签名配置及原生组件说明见 [BUILDING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING)、[BUILDING-SIGNING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING-SIGNING)。

## 自动构建与发布

- 默认分支每次提交都会执行 Android 构建、单元测试和 lint。
- 发布前必须在仓库 Actions Secrets 配置 `KEYSTORE_BASE64`、`KEYSTORE_PASSWORD`、`KEY_ALIAS` 和 `KEY_PASSWORD`。
- 构建成功后会创建 `v<versionName>-<提交短哈希>` 标签并发布签名 APK 与 `SHA256SUMS`。
- Wiki 内容以仓库 `wiki/` 目录为源；需另行配置可写入 Wiki 的 `WIKI_TOKEN` Secret。
- GitHub Pages 从 `docs/` 目录部署。

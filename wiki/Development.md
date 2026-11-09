# 构建与发布

## 本地构建

项目需要 JDK、Android SDK、NDK 与 CMake。按 [BUILDING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING) 配置环境后，可运行：

```sh
./gradlew testDebugUnitTest lintDebug assembleDebug assembleRelease
```

完整 CI 依赖、签名配置及原生组件说明见 [BUILDING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING)、[BUILDING-SIGNING](https://github.com/wakawakalululu/PinyinSSH/blob/master/BUILDING-SIGNING)。

## 自动化发布流水线

仓库包含两个工作流（位于 `.github/workflows/`）：

- `build-apk.yml`：构建 debug/release APK，并在签名就绪时发布 GitHub Release；
- `publish-docs.yml`：把 `docs/` 部署到 GitHub Pages，并把 `wiki/` 同步到仓库 Wiki。

触发规则：

- `build-apk.yml`：
  - 推送 `main`/`master`：**构建并上传产物**；签名就绪时会额外创建 `v<version>-<12位提交哈希>` 标签并发布（含签名 APK 与 `SHA256SUMS`）；
  - 推送 `v*` 标签：**构建并发布** Release（标签需匹配 `v<applicationVersionName>` 或 `v<version>-<12位哈希>`）；
  - `pull_request`：构建但产物未签名；
  - `workflow_dispatch`：手动构建（可选 `abis` 输入）。
  - 仅文档类改动（`docs/**`、`wiki/**`、`*.md` 等，本工作流文件除外）**不会**触发本工作流（已配置 `paths-ignore`）。
- `publish-docs.yml`：仅当改动涉及 `docs/**`、`wiki/**` 或工作流自身时触发。

> 提示：文档改动不应触发 APK 重新构建与发布；如误触发，请检查 `build-apk.yml` 的 `paths-ignore` 是否覆盖相关路径。

## 配置签名 Secrets

在 `wakawakalululu/PinyinSSH` → Settings → Secrets and variables → Actions 中新增以下仓库 Secret（值为单行文本）：

| Secret | 说明 |
| --- | --- |
| `KEYSTORE_BASE64` | 签名库 base64：`base64 -w0 myKeystore.jks` |
| `KEYSTORE_PASSWORD` | keystore 口令 |
| `KEY_ALIAS` | 密钥别名 |
| `KEY_PASSWORD` | 密钥口令 |

四个值需同时配置；缺任一个（或 `KEYSTORE_BASE64` 为空）都会导致发布被阻止（`release` 任务的 `Require a signed release APK` 步骤会报错退出）。未配置时分支构建仍可成功，但 release APK 为未签名。

## 配置 WIKI_TOKEN

Wiki 同步使用独立的 Personal Access Token（Wiki 是独立于代码仓库的 `*.wiki.git`，内置 `GITHUB_TOKEN` 不覆盖其写权限）：

1. 在 GitHub → Settings → Developer settings → Personal access tokens → **classic** 生成一个 PAT；
2. 权限选择 **`repo`**（fine-grained PAT 目前不覆盖 Wiki 写权限）；
3. 在仓库 Actions Secrets 中新增 Secret，名称必须为 **`WIKI_TOKEN`**，值为该 PAT；
4. 若误命名为 `GH_TOKEN`，工作流读的是 `secrets.WIKI_TOKEN`，会判定为「未配置」而跳过同步。

### 轮换与失效排查

- **轮换**：在 GitHub 撤销旧 PAT 并生成新 PAT，更新 `WIKI_TOKEN` Secret 即可；不影响已发布的 Wiki 内容。
- **未同步 / 跳过**：检查最近一次 `Publish documentation / Sync GitHub Wiki` 任务日志；若显示 `WIKI_TOKEN is not configured; skipping Wiki sync` 警告，说明 Secret 缺失或命名不正确。
- **权限不足**：日志出现 `401`/`403` 时，确认 PAT 为 classic 且具备 `repo` 权限、未过期、未被吊销。
- **PAT 泄露**：立即在 GitHub 撤销该 PAT；由于它仅用于 Wiki 写操作，影响范围限于 Wiki 内容，但仍建议审查 Wiki 历史。

## 校验发布产物

- Release 资产包含 `PhoneSecurityAssistant-<版本>.apk`（release，已签名）、`PhoneSecurityAssistant-<版本>-debug.apk` 与 `SHA256SUMS`；
- `SHA256SUMS` 由 CI 用 `sha256sum` 生成，并在发布前 `sha256sum --check` 校验通过；
- 签名 APK 经 `apksigner verify` 验证（`Verified using v2 scheme`）。下载后可本地复核：

```sh
sha256sum -c SHA256SUMS
apksigner verify --print-certs PhoneSecurityAssistant-<版本>.apk
```

# CHANGELOG：dsh-purge 1.1.93 本地适配版

## 发布标识

- 包名：`dsh-purge`
- 版本：`1.1.93`
- 发布类型：本地适配社区构建 / community build
- 上游基线：`1.1.92`
- 主宿主目标：DeepSeek Harness `0.2.2-rc.3`
- 兼容声明：`0.2.2-rc.3`、`0.2.1-rc.5`、`0.2.1-rc.2`
- 生成日期：2026-11-15

本版本没有冒充上游正式发布。上游代码、许可证、NOTICE 和商业许可说明均保留。

## 改动

### 1. 宿主目标

`package.json` 更新为：

```json
"dshTarget": "0.2.2-rc.3",
"dshTargets": [
  "0.2.2-rc.3",
  "0.2.1-rc.5",
  "0.2.1-rc.2"
]
```

### 2. `lib/core.js`

- 适配 DeepSeek Harness `0.2.2-rc.3` 的提示词装配流程。
- `system-prompt/assemble` 返回结构变化后保留 `tools`、`sections` 和必要运行时上下文。
- 保留 `dsh-purge` section 与注入文本。
- 补充新版宿主下的错误兜底和兼容路径。
- 保留并验证核心补丁：

```text
#13 #14 #17 #18 #35 #40 #62 #67 #74
```

适配文件指纹：

```text
SHA-256 A818257DA28A45F07CB0529B595A6361EF17C30D435E16C4A15D6E2D1A3A7CEC
size    391942 bytes
```

### 3. `lib/desktop.js`

- Windows 桌面密封 asar 换包改用独立 Node 助手。
- 助手等待旧 Harness 进程退出后重试挪包，降低宿主在换包时静默退出的概率。
- 失败时保留 pending/恢复信息，并避免内嵌 PowerShell 分支在部分 `0.2.2-rc.3` 安装上提前结束。
- 保留 asar、unpacked 与 `app/` 的入口回退逻辑。

适配文件指纹：

```text
SHA-256 737F82161284F16A56CD9F3B6AD11AFD852E8DC1C282E90D8E4A8B39AD2A4280
size    82776 bytes
```

### 4. 发布文档

新增：

- `安装与注意事项.md`
- `CHANGELOG-1.1.93.md`
- `校验清单.md`
- `SHA256SUMS.txt`
- 发布压缩包的 `.zip.sha256` 文件

三份上游 README 的版本、宿主徽章和适配范围已更新为本次本地构建的信息。

## 验证结果

### 宿主与兼容性

```text
host_version:      0.2.2-rc.3
host_target:       0.2.2-rc.3 / 0.2.1-rc.5 / 0.2.1-rc.2
hostCompat.known:  true
hostCompat.ok:     true
hostCleanMarkers:  present
ready:             true
host_supported:    true
patches_pending:   0
patches_lost:      false
```

### 补丁状态

状态页按补丁组统计：

```text
applied: 65
pending: 0
skipped: 1
drifted: 1
na:      9
```

原始行级审计：

```text
applied: 75
na:      9
skipped: 1
drifted: 1
```

已知差异：

- `#54 DELIVERABLE_DO_NOT`：`drifted`，可选补丁，不影响核心适配。
- `#80 MINIMAL_OFFICIAL_TOOLSET`：`skipped`，可选补丁。
- 其余未命中的行属于当前宿主不提供的可选能力，状态为 `na`。

### 源码一致性

安装目录与工作源码目录的以下文件哈希一致：

```text
lib/core.js
lib/desktop.js
```

发布包直接使用同一份 `core.js` 和 `desktop.js`。

### 语法检查

通过本机 Node `v24.21.0`：

```text
node --check lib/core.js
node --check lib/desktop.js
```

注意：`package.json` 声明的最低运行时仍为 Node.js 18。

## 已知限制

1. 只在当前本机 `0.2.2-rc.3` 桌面环境中完成了核心适配验证；`0.2.1-rc.5` 与 `0.2.1-rc.2` 是声明兼容，不是本次逐项实机回归。
2. `#54` 仍为可选漂移，不能把“65 项已应用”误解成全部 67 项无差异。
3. 本包不包含运行时 `node_modules`、asar 备份、插件备份或 `.bak-*` 临时文件。
4. 安装或升级前必须完全退出 Harness，包括托盘和 Electron 子进程。
5. 现有 `app.asar.bak` 必须保留；本次未改动、未重新打包、未写入该备份。

## 升级与回滚

完整步骤见 `安装与注意事项.md`。最短路径：

1. 完全退出 Harness。
2. 备份旧插件目录。
3. 覆盖发布包，或通过本地插件管理器重装。
4. 启动后检查 `/purge status`。
5. 若加载失败，恢复旧插件目录，不要删除 `app.asar.bak`。

## 许可

本项目上游使用 Apache-2.0。再分发必须保留：

- `LICENSE`
- `NOTICE`
- `LICENSE-COMMERCIAL.md`
- 上游仓库署名：`https://github.com/HunYiSage/dsh-purge`
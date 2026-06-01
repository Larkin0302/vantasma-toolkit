# Changelog

All notable changes to wxchat are documented in this file.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [SemVer](https://semver.org/spec/v2.0.0.html).

## [2.0.0] - 2026-05-12

### Added — 一键自动化解密 & CLI 工业级化
- **`sudo wxchat setup` / `sudo wxchat decrypt`**：自带 SQLCipher v4 解密层
  - `wxchat_native/find_keys_macos.c`（自写）扫 WeChat 主进程内存 `x'<key><salt>'` ASCII 模式
  - `wxchat_core/crypto.py`（自写）PBKDF2-HMAC-SHA512 + AES-256-CBC + page-by-page HMAC
  - `wxchat_core/decrypt_pipeline.py` 编排：扫 key → match salt → 解 db → 落数据目录
  - 实测端到端 < 30 秒解 17 个 db
- **`wxchat --version`**：标准版本输出
- **`wxchat --json`**：全 62 个子命令支持 JSON 输出
  - 11 个原生 JSON：`ls / info / doctor / contacts / history / group-info / group-members / stats-overview / voice-stats / biz-accounts`
  - 其余 51 个通用 wrap：`{command, output_text, exit_code, format, note}`
- **`wxchat completion {bash,zsh,fish}`**：shell completion 一行装
- **`wxchat watch`**：实时监听新消息（tail -f 风格，poll `message_resource.db`）
- **退出码遵循 BSD sysexits.h**：64=usage / 66=noinput / 70=software / 77=noperm / ...
- **全局 flag**：`--no-color` / `--quiet` / `--verbose` / `NO_COLOR=1` 支持
- **子命令命名统一 kebab-case**：62 个子命令全部 `<noun>-<verb>` 风格

### Changed — Breaking
- 子命令拆分：`fav search` → `fav-search`，`voice transcribe` → `voice-transcribe`，
  `stats overview` → `stats-overview`，`sns ls` → `sns-ls` 等等
- 旧的 `wxchat <parent> <action>` 形式不再支持

### Removed
- 移除 `docs/UPSTREAM_INVENTORY.md`（v1.0 时期的迁移历史，已不需要）

---

## [1.1.0] - 2026-05-12

### Added — 22 个新子命令 + 实时监听
- 撤回消息 / 未读明细 / 联系人标签 / 群详情 / FTS 快速搜
- 表情包 / 企微会话 / 朋友圈互动 / 小程序 / 视频号直播 / 外部 IM
- 公众号详情 / 服务通知订阅 / 微信内搜索历史 / 转发历史 ...

---

## [1.0.0] - 2026-05-12

### Added — 自给自足查询库
- `wxchat_core/` 5 模块（cache / contacts / messages / voice / content）
- 17 个子命令：ls / search / history / export / contacts / sns / files / solitaire / voice / fav / friends / finder / money / stats / biz / avatar / info
- 去掉所有 `mcp_server` 依赖

[2.0.0]: https://github.com/xiangruiai/wxchat/releases/tag/v2.0.0
[1.1.0]: https://github.com/xiangruiai/wxchat/releases/tag/v1.1.0
[1.0.0]: https://github.com/xiangruiai/wxchat/releases/tag/v1.0.0

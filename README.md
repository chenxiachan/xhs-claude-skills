<div align="center">

# 📕 小红书转 Obsidian

[![Claude Code](https://img.shields.io/badge/Claude_Code-插件-7C3AED)](https://docs.anthropic.com/en/docs/claude-code)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

中文 &nbsp;|&nbsp; **[English](README_EN.md)**

</div>

---

一个 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 插件，把小红书帖子一键提取为简洁的 [Obsidian](https://obsidian.md) 笔记。支持图文和视频帖子——视频会自动下载并用本地 whisper 做语音转录。不需要 MCP 服务、无头浏览器或任何后端，只用 cookies + HTTP + 本地模型。

---

## 🚀 安装

### 前置要求

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)（本插件的运行环境）
- [Obsidian](https://obsidian.md)（只需 vault 文件夹存在，不需要 CLI）
- 视频转录（可选）：`brew install ffmpeg` + `pip install mlx-whisper`

### 安装插件

```bash
claude /plugin install chenxiachan/xhs-claude-skills
```

### 首次使用

```
/xhs https://www.xiaohongshu.com/explore/...
```

首次运行会自动引导你完成 **30 秒 Cookie 设置**：

1. 🌐 打开 Chrome → xiaohongshu.com（确保已登录）
2. 🔧 打开 DevTools Console（F12）
3. 📋 粘贴 skill 给出的代码 → cookies 自动复制到剪贴板
4. 💾 保存到 `~/cookies.json`
5. ✅ 搞定 — 之后每次运行自动使用

> 🔄 Cookies 过期时 skill 会自动检测并重新引导，无需手动检查。

---

## ✨ 功能

| 命令 | 说明 |
|:-----|:-----|
| `/xhs <链接>` | 📄 提取单个帖子 — 文字、图片、视频转录 |
| `/xhs-batch <链接列表>` | 📦 批量提取多个帖子 |
| `/xhs-analyze [关键词]` | 🔍 分析已保存的帖子 — 总结、对比、发现模式 |

### 📂 输出

笔记按日期排序，直接放在 Obsidian vault 的 `xhs/` 文件夹下：

```
xhs/
├── 2026-03-22 YY方法论解析.md
├── 2026-03-29 ZZ技术突破.md
├── img/
└── video/
```

每条笔记是**决策工具**——扫一眼决定深挖还是跳过：

```markdown
# 一句话洞察                          ← 判断，不是描述

核心论点，2-3 句话。

**与我的关联：** 为什么跟我有关。
**值得深挖吗：** 是/否 + 理由。

> [!tip]- 详情                         ← 默认折叠
> 结构化内容...

> [!info]- 笔记属性                     ← 默认折叠
> 来源 · 日期 · 互动 · 标签
```

"与我的关联"会读取 Claude Code 的 [memory](https://docs.anthropic.com/en/docs/claude-code) 自动适配你的背景，无需手动配置。

---

## 🏗 工作原理

```
 小红书链接
     │
     ▼
 ┌─────────────────────────┐
 │  Cookie 认证              │  ← 复用 Chrome 登录态
 └────────────┬────────────┘
              ▼
 ┌─────────────────────────┐
 │  解析 __INITIAL_STATE__  │  ← 一次 HTTP 请求拿到全部数据
 └────┬──────┬──────┬──────┘
      ▼      ▼      ▼
    文字    图片    视频
                     │
                curl → ffmpeg → mlx-whisper
                     │
                     ▼
              Obsidian 笔记
```

---

## ⚙️ 配置

| 配置项 | 默认值 | 说明 |
|:------|:------|:-----|
| Cookies | `~/cookies.json` | 小红书认证 |
| 输出目录 | `~/Documents/Obsidian Vault/xhs` | Obsidian vault 路径 |

路径不同时编辑 `skills/xhs/SKILL.md` 中的常量定义。

## 📁 插件结构

```
rednote-to-obsidian/
├── .claude-plugin/plugin.json
└── skills/
    ├── xhs/SKILL.md
    ├── xhs-batch/SKILL.md
    └── xhs-analyze/SKILL.md
```

<div align="center">

---

MIT License

</div>

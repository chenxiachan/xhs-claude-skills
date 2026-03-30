<div align="center">

# 📕 RedNote to Obsidian

[![Claude Code](https://img.shields.io/badge/Claude_Code-plugin-7C3AED)](https://docs.anthropic.com/en/docs/claude-code)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

**[中文](README.md)** &nbsp;|&nbsp; English

</div>

---

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) plugin that extracts [RedNote (小红书)](https://www.xiaohongshu.com) posts into concise [Obsidian](https://obsidian.md) notes. Supports text, images, and video — video posts are automatically downloaded and transcribed locally with whisper. No MCP server, no headless browser, no backend. Just cookies + HTTP + local models.

---

## 🚀 Install

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (runtime environment)
- [Obsidian](https://obsidian.md) (just needs the vault folder — no CLI required)
- Video transcription (optional): `brew install ffmpeg` + `pip install mlx-whisper`

### Install the plugin

```bash
claude /plugin install chenxiachan/xhs-claude-skills
```

### First use

```
/xhs https://www.xiaohongshu.com/explore/...
```

On first run, the skill auto-guides you through a **30-second cookie setup**:

1. 🌐 Open Chrome → xiaohongshu.com (make sure you're logged in)
2. 🔧 Open DevTools Console (F12)
3. 📋 Paste the snippet the skill gives you → cookies auto-copied to clipboard
4. 💾 Save to `~/cookies.json`
5. ✅ Done — all future runs use this automatically

> 🔄 When cookies expire, the skill detects it and re-prompts. No manual checking needed.

---

## ✨ Features

| Command | Description |
|:--------|:------------|
| `/xhs <url>` | 📄 Extract a single post — text, images, video transcription |
| `/xhs-batch <urls>` | 📦 Batch extract multiple posts |
| `/xhs-analyze [keyword]` | 🔍 Analyze saved posts — summarize, compare, find patterns |

### 📂 Output

Notes are date-sorted and saved directly in your Obsidian vault under `xhs/`:

```
xhs/
├── 2026-03-22 YY-methodology.md
├── 2026-03-29 ZZ-breakthrough.md
├── img/
└── video/
```

Each note is a **decision tool** — scan in 5 seconds, decide to dig deeper or skip:

```markdown
# One-line insight                     ← judgment, not description

Core argument, 2-3 sentences.

**Relevance:** Why this matters to you.
**Worth digging?** Yes/No + reason.

> [!tip]- Details                       ← collapsed by default
> Structured content...

> [!info]- Metadata                     ← collapsed by default
> Source · date · stats · tags
```

The "Relevance" line reads from Claude Code's [memory system](https://docs.anthropic.com/en/docs/claude-code) to auto-adapt to your background. No manual config needed.

---

## 🏗 How it works

```
 RedNote URL
     │
     ▼
 ┌─────────────────────────┐
 │  Cookie auth              │  ← Reuses Chrome login session
 └────────────┬────────────┘
              ▼
 ┌─────────────────────────┐
 │  Parse __INITIAL_STATE__ │  ← One HTTP request, all data
 └────┬──────┬──────┬──────┘
      ▼      ▼      ▼
    Text   Images  Video
                     │
                curl → ffmpeg → mlx-whisper
                     │
                     ▼
              Obsidian note
```

---

## ⚙️ Configuration

| Setting | Default | Description |
|:--------|:--------|:------------|
| Cookies | `~/cookies.json` | RedNote auth |
| Output dir | `~/Documents/Obsidian Vault/xhs` | Obsidian vault path |

Edit constants in `skills/xhs/SKILL.md` if your paths differ.

## 📁 Plugin structure

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

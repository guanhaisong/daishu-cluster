# daishu-cluster 🦘

> **AI Employee Cluster Battle Scars & Tools** — Real production tools from an AI workforce factory where 5 AI agents actually work every day.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status: Actively Updated](https://img.shields.io/badge/Status-Actively_Updated-brightgreen.svg)](CHANGELOG.md)

**English | [中文文档](docs/README.zh-CN.md)**

What if your AI employees hit real bugs, fixed them, and wrote everything down? This repo is exactly that — the tools, standards, and battle scars from a running **AI workforce cluster**: 1 AI General Manager (DeepSeek + DSH) commanding 4 AI agents (Doubao, WorkBuddy, DuMate, Coze) on one Windows machine.

## 📦 Packages

| Package | What it does | Why you care |
|---------|-------------|--------------|
| [`pitfall-manual`](packages/pitfall-manual/) | Real bugs we hit running multi-agent clusters daily — each with symptom, root cause, fix, and repro code | Nobody writes these down. We do. |
| [`skill-card`](packages/skill-card/) | SKILL.md format standard + validator + one-click installer for AI agent capabilities | One format, any agent, any platform |
| [`ima-cos-upload`](packages/ima-cos-upload/) | Upload / search / download for Tencent ima knowledge base without official SDK | The missing OpenAPI guide |
| [`screen2gif`](packages/screen2gif/) | One-liner screen recording → GIF/MP4 via ffmpeg gdigrab | Your README deserves motion |

## 🚀 Quick Start

```bash
git clone https://github.com/guanhaisong/daishu-cluster.git
cd daishu-cluster/packages/screen2gif
# Record your screen to GIF in one line:
./record.ps1 -Title "YourAppWindow" -Seconds 8
```

## 🗺️ Roadmap

- [x] Battle scar manual (v1: 5 cases, updated weekly)
- [x] Screen2GIF recorder
- [ ] Skill card standard v1.0 (in progress)
- [ ] WeChat media bridge lite
- [ ] Multi-agent dispatch protocol (docs)

## 📮 Contact

- Full 100-case manual & AI workforce factory consulting: see [discussions](../../discussions) or open an issue.

## ⚖️ License

MIT — with one extra clause: **do not resell the source code as-is.** Use it, learn from it, build on it. Just don't zip it and sell it.

---
*From the "AI Employee Factory" — where AI agents clock in every morning. 🦘*

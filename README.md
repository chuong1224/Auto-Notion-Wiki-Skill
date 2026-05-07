# 🧠 AI Wiki Curator — Claude Code Skill

<p align="center">
  <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/releases">
    <img src="https://img.shields.io/github/v/release/chuong1224/Auto-Notion-Wiki-Skill?color=brightgreen&label=version" alt="Version">
  </a>
  <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT">
  </a>
  <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/commits/main">
    <img src="https://img.shields.io/github/last-commit/chuong1224/Auto-Notion-Wiki-Skill" alt="Last commit">
  </a>
  <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill">
    <img src="https://img.shields.io/github/stars/chuong1224/Auto-Notion-Wiki-Skill?style=social" alt="GitHub Stars">
  </a>
</p>

> A Claude Code skill that automatically saves, classifies, and organizes AI knowledge into your Notion wiki — articles, tools, prompts, sources, **open source repos**, and notes, all in one place.

> Skill cho Claude Code tự động lưu, phân loại và tổ chức kiến thức AI vào Notion wiki của bạn — bài viết, công cụ, prompt, nguồn theo dõi, **repo open source** và ghi chú, tất cả trong một nơi.

**Version:** 2.0.0 · **License:** MIT

---

## Version History

| Version | Date       | Key Changes |
|---------|------------|-------------|
| **2.0.0** | 2026-05-07 | Added **💻 Open Source Repos** database (6 databases total). Fixed classification so `github.com/{user}/{repo}` now correctly goes to Open Source Repos instead of Tools. Added Gotchas **G13–G15**. Updated all classification rules, schemas, and workflow details. |
| 1.3.0   | 2026-04-24 | Initial public release with 5 databases and core 6-step workflow. |

---

## English

### What is AI Wiki Curator?

**AI Wiki Curator** is a [Claude Code](https://claude.ai/code) skill that turns a single save command into a fully structured Notion wiki entry. Paste a link, drop a prompt, write a quick note, or share a GitHub repo — Claude automatically classifies it, fills in the metadata, saves it to the right Notion database, and logs the change.

### Trigger Phrases

The skill activates whenever you say something like:

| You say | Claude does |
|---|---|
| `luu link nay vao wiki` | Save URL → classify → write to Notion |
| `them tool nay vao notion` | Create a tool entry in 🛠 Công Cụ AI |
| `ghi lai prompt nay` | Save prompt to 🧩 Prompt & Template |
| `theo doi kenh youtube nay` | Add channel to 📡 Nguồn Theo Dõi |
| `save bai viet nay` | Save article to 📚 Kiến Thức |
| `luu repo github nay` / `save this open source repo` | Save repo to 💻 Open Source Repos |
| `luu note nay` | Save to 🗒 Ghi Chú Cá Nhân |
| `add this to my AI wiki` | Same as above, English version |

> The skill does **not** activate for pure knowledge questions (`what is X?`) or content-creation requests with no intent to save.

### Wiki Structure (6 Databases + Change Logs)

On first run, the skill creates a full structure under a master Notion page **🧠 AI Wiki**:

| Component | Type | Contains |
|---|---|---|
| 📚 Kiến Thức & Nghiên Cứu | Database | Papers, articles, blogs, single videos |
| 🛠 Công Cụ AI | Database | Tools, apps, APIs, SaaS |
| **💻 Open Source Repos** | **Database** | **GitHub / GitLab / Codeberg repos, free OSS projects** |
| 🧩 Prompt & Template | Database | Prompts, system prompts, templates |
| 📡 Nguồn Theo Dõi | Database | Channels, profiles, newsletters, communities |
| 🗒 Ghi Chú Cá Nhân | Database | Raw notes, insights, ideas |
| 📋 Change Logs | Sub-page | Full update history with versioning |

### 6-Step Workflow

1. **Parse** — Extract URL and text. Fetch title + description from the URL if available.
2. **Classify** — Apply classification rules (see below). Ask user if ambiguous.
3. **Dedup check** — Search Notion before creating. Offer update vs. new if duplicate found.
4. **Write to Notion** — Create entry with full schema. Create database if missing.

> ⚠️ **CONTENT INTEGRITY RULE (v2.0.0)**
> When saving Prompts, Notes, Skills, Templates (any plain text content from user):
> - **NEVER** edit, paraphrase, add/remove meaning, translate, or rewrite
> - **ONLY** allowed to format for readability: code blocks, line breaks, whitespace, emoji headers
> - For URLs: fetching title and short description is fine, doesn't violate this rule

5. **Update metadata** —
   - **5a.** Update "Last updated" timestamp on the master page 🧠 AI Wiki (append if missing).
   - **5b.** Prepend new entry to 📋 Change Logs with auto-incremented version.
6. **Confirm** — Print a summary with Notion link.

``` 
✅ Saved: langchain
📁 Into: 💻 Open Source Repos
🏷 Tags: local-run, production-ready
📅 Change Log: Version v2.0 - 07/05/2026
🔗 Notion: https://notion.so/…
```

### Auto-Classification (Updated in v2.0.0)

**By URL domain:**

| URL Pattern | Database | Tag |
|---|---|---|
| `arxiv.org`, `paperswithcode.com`, `openreview.net` | 📚 Kiến Thức | paper |
| `youtube.com/@channel`, `youtube.com/channel/` | 📡 Nguồn Theo Dõi | YouTube |
| `youtube.com/watch`, `youtu.be/{id}` | 📚 Kiến Thức | video |
| `twitter.com/{user}` / `x.com/{user}` (profile) | 📡 Nguồn Theo Dõi | Twitter-X |
| `twitter.com/{user}/status/` (specific tweet) | 📚 Kiến Thức | article |
| `github.com/{user}` (profile) | 📡 Nguồn Theo Dõi | GitHub |
| **`github.com/{user}/{repo}`** | **`💻 Open Source Repos`** | **`repo`** |
| `huggingface.co/papers/` | 📚 Kiến Thức | paper |
| `huggingface.co/spaces/` | 🛠 Công Cụ AI | demo |
| `huggingface.co/models/`, `/datasets/` | 🛠 Công Cụ AI | model |
| `cursor.sh`, `v0.dev`, `perplexity.ai`... | 🛠 Công Cụ AI | tool |
| `medium.com`, `dev.to`... | 📚 Kiến Thức | article |
| `discord.gg/`, `reddit.com/r/` | 📡 Nguồn Theo Dõi | community |

> **Note:** Repos with a commercial hosted version (e.g. Supabase, Milvus) → still go to 💻 Open Source Repos if you paste the GitHub link.

**By text content (no URL):**

| Signal | Database | Tag |
|---|---|---|
| Starts with "Act as", "You are a", "Bạn là" + `###` | 🧩 Prompt | system-prompt |
| Template with `[placeholder]` | 🧩 Prompt | template |
| Short quick note | 🗒 Ghi Chú | raw-note |
| Long structured text | 🗒 Ghi Chú | note |

### Notion Database Schema (v2.0.0)

**💻 Open Source Repos** (new in v2.0.0)

| Property | Type | Notes |
|---|---|---|
| Tên Repo | title | Repo name (e.g. langchain, ollama) |
| URL | url | Original repo URL |
| Ngôn ngữ | select | Python / JavaScript / TypeScript / Rust / Go / C++ / Other |
| Danh mục | select | LLM / Agent / RAG / Fine-tuning / Framework / Inference / Utils / Tools / Dataset |
| Tags | multi_select | local-run, no-GPU, GPU-required, beginner-friendly, production-ready, MIT, Apache-2.0 |
| Mô tả | rich_text | What the repo does + why you saved it (1–3 sentences) |
| License | select | MIT / Apache-2.0 / GPL / BSD / Other / Unknown |
| Stars ước tính | select | dưới-1k / 1k-10k / 10k-50k / 50k+ |
| Ngày lưu | date | Auto |
| Đã clone / thử | checkbox | Default: false |

*(Other databases remain similar to v1.3.0 with minor improvements — full details in `ai-wiki-curator.md`)*

### Requirements

| Requirement | Details |
|---|---|
| Claude Code | Latest version |
| `NOTION_API_KEY` | Internal integration token from Notion Integrations |
| `NOTION_PAGE_ID` | ID of your **🧠 AI Wiki** parent page |

### Installation

1. Copy the skill file:
   ```bash
   cp ai-wiki-curator.md ~/.claude/skills/     # global
   # or
   cp ai-wiki-curator.md .claude/skills/       # project-local
   ```

2. Set environment variables and grant Notion integration access.

3. On first use, Claude will create the 6 databases + master page.

### Gotchas (Updated v2.0.0)

- **G13** — Repo có hosted version thương mại (Supabase, Milvus...): Paste GitHub link → 💻 Open Source Repos. Paste landing page → 🛠 Công Cụ AI.
- **G14** — Thiếu License / Stars: Dùng giá trị "Unknown" / "dưới-1k" làm default, không block việc lưu.
- **G15** — Hugging Face: `huggingface.co/models/` → 🛠 Công Cụ AI (model). GitHub code repo của cùng project → 💻 Open Source Repos (riêng biệt).

*(Full list of 15 Gotchas available in the detailed skill file `ai-wiki-curator.md`)*

### License

MIT — see [LICENSE](LICENSE).

---

## Tiếng Việt

### AI Wiki Curator là gì?

**AI Wiki Curator** là skill cho Claude Code biến lệnh lưu thành entry đầy đủ trong Notion wiki. Dán link, thả prompt, ghi chú nhanh hoặc chia sẻ repo GitHub — Claude tự động phân loại, điền metadata, lưu đúng database và ghi lịch sử.

### Version History

Xem bảng phiên bản ở đầu trang (phiên bản mới nhất: **2.0.0**).

### Cài đặt & Yêu cầu

Giống phần English ở trên.

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

<div align="center">
  Made with Claude Code · <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/issues">Report an issue</a> · Updated to v2.0.0
</div>

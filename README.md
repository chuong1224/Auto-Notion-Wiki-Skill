# 🧠 AI Wiki Curator — Claude Code Skill

> A Claude Code skill that automatically saves, classifies, and organizes AI knowledge into your Notion wiki — articles, tools, prompts, sources, and notes, all in one place.

> Skill cho Claude Code tự động lưu, phân loại và tổ chức kiến thức AI vào Notion wiki của bạn — bài viết, công cụ, prompt, nguồn theo dõi và ghi chú, tất cả trong một nơi.

**Version:** 1.3.0 · **License:** MIT

---

## English

### What is AI Wiki Curator?

**AI Wiki Curator** is a [Claude Code](https://claude.ai/code) skill that turns a single save command into a fully structured Notion wiki entry. Paste a link, drop a prompt, or write a quick note — Claude automatically classifies it, fills in the metadata, saves it to the right Notion database, and logs the change.

### Trigger Phrases

The skill activates whenever you say something like:

| You say | Claude does |
|---|---|
| `luu link nay vao wiki` | Save URL → classify → write to Notion |
| `them tool nay vao notion` | Create a tool entry in 🛠 Công Cụ AI |
| `ghi lai prompt nay` | Save prompt to 🧩 Prompt & Template |
| `theo doi kenh youtube nay` | Add channel to 📡 Nguồn Theo Dõi |
| `save bai viet nay` | Save article to 📚 Kiến Thức |
| `luu note nay` | Save to 🗒 Ghi Chú Cá Nhân |
| `add this to my AI wiki` | Same as above, English version |
| `save to notion wiki` | Same as above, English version |

> The skill does **not** activate for pure knowledge questions (`what is X?`) or content-creation requests with no intent to save.

### Wiki Structure

On first run, the skill creates a full structure under a master Notion page **🧠 AI Wiki**:

| Component | Type | Contains |
|---|---|---|
| 📚 Kiến Thức & Nghiên Cứu | Database | Papers, articles, blogs, single videos |
| 🛠 Công Cụ AI | Database | Tools, apps, APIs, GitHub repos |
| 🧩 Prompt & Template | Database | Prompts, system prompts, templates |
| 📡 Nguồn Theo Dõi | Database | Channels, profiles, newsletters, communities |
| 🗒 Ghi Chú Cá Nhân | Database | Raw notes, insights, ideas |
| 📋 Change Logs | Sub-page | Full update history with versioning |

### 6-Step Workflow

1. **Parse** — Extract URL and text. Fetch title + description from the URL if available.
2. **Classify** — Apply classification rules (see below). Ask user if ambiguous.
3. **Dedup check** — Search Notion before creating. Offer update vs. new if duplicate found.
4. **Write to Notion** — Create entry with full schema. Create database if missing.

> ⚠️ CONTENT INTEGRITY RULE (v1.3.0)
> When saving Prompts, Notes, Skills, Templates (any plain text content from user):
> - NEVER edit, paraphrase, add/remove meaning, translate, or rewrite
> - ONLY allowed to format for readability: code blocks, line breaks, whitespace, emoji headers
> - For URLs: fetching title and short description is fine, doesn't violate this rule

5. **Update metadata** —
   - **5a.** Update "Last updated" timestamp on the master page 🧠 AI Wiki (append if missing).
   - **5b.** Prepend new entry to 📋 Change Logs with auto-incremented version. Read latest version from first entry → increment minor (`v1.0 → v1.1 → v1.2`…). If unreadable, fallback to date-based versioning. Entry format: `Version vX.X - DD/MM/YYYY - [Action] [Category]: [Title]`.
6. **Confirm** — Print a summary with Notion link.

```
✅ Saved: Cursor AI
📁 Into: 🛠 Công Cụ AI
🏷 Tags: coding, free-tier
📅 Change Log: Version v1.3 - 24/04/2026
🔗 Notion: https://notion.so/…
```

### Auto-Classification

**By URL domain:**

| URL Pattern | Database | Tag |
|---|---|---|
| `arxiv.org`, `paperswithcode.com` | 📚 Kiến Thức | paper |
| `youtube.com/@channel`, `youtube.com/channel/` | 📡 Nguồn Theo Dõi | YouTube |
| `youtube.com/watch`, `youtu.be/{id}` | 📚 Kiến Thức | video |
| `twitter.com/{user}` / `x.com/{user}` (profile) | 📡 Nguồn Theo Dõi | Twitter-X |
| `twitter.com/{user}/status/` (specific tweet) | 📚 Kiến Thức | article |
| `github.com/{user}` (profile) | 📡 Nguồn Theo Dõi | GitHub |
| `github.com/{user}/{repo}` | 🛠 Công Cụ AI | open-source |
| `huggingface.co/papers/` | 📚 Kiến Thức | paper |
| `huggingface.co/spaces/` | 🛠 Công Cụ AI | demo |
| `medium.com`, `dev.to`, `towardsdatascience.com` | 📚 Kiến Thức | article |
| `discord.gg/`, `reddit.com/r/` | 📡 Nguồn Theo Dõi | community |
| `substack.com` (author homepage) | 📡 Nguồn Theo Dõi | Substack |
| `substack.com/p/` (article) | 📚 Kiến Thức | newsletter |
| `linkedin.com/in/`, `linkedin.com/company/` | 📡 Nguồn Theo Dõi | LinkedIn |
| `tiktok.com/@{user}` (profile) | 📡 Nguồn Theo Dõi | TikTok |
| `tiktok.com/@{user}/video/` | 📚 Kiến Thức | video |
| `huggingface.co/models/`, `/datasets/` | 🛠 Công Cụ AI | model |
| `huggingface.co/{user}` (profile) | 📡 Nguồn Theo Dõi | HuggingFace |
| `cursor.sh`, `v0.dev`, `perplexity.ai`, `runway.ml`, `replit.com` | 🛠 Công Cụ AI | tool |
| `openai.com/blog/`, `anthropic.com/news/` | 📚 Kiến Thức | article |

**By text content (no URL):**

| Signal | Database | Tag |
|---|---|---|
| Starts with "Act as", "You are a", "Bạn là" | 🧩 Prompt | system-prompt |
| Template with `[placeholder]` | 🧩 Prompt | template |
| Short quick note | 🗒 Ghi Chú | raw-note |
| Long structured text | 🗒 Ghi Chú | note |

### Notion Database Schema

**📚 Kiến Thức & Nghiên Cứu (Knowledge & Research)**

| Property | Type | Notes |
|---|---|---|
| Tiêu đề | title | Article / paper / video name |
| URL | url | Original link |
| Loại | select | paper / article / video / blog / newsletter |
| Tags | multi_select | LLM, Agents, RAG, Fine-tuning, Vision… |
| Tác giả / Nguồn | rich_text | Author name or source site |
| Tóm tắt | rich_text | 1–3 sentence summary |
| Ngày lưu | date | Auto: today |
| Đã đọc | checkbox | Default: false |

**🛠 Công Cụ AI (AI Tools)**

| Property | Type | Notes |
|---|---|---|
| Tên Tool | title | Tool name |
| URL | url | |
| Danh mục | select | Coding / Writing / Image / Video / Search / Productivity / API |
| Tags | multi_select | free-tier, open-source, local, API… |
| Mô tả | rich_text | What the tool does (1–3 sentences) |
| Giá | select | Free / Freemium / Paid / Open-source |
| Ngày lưu | date | Auto |
| Đã dùng thử | checkbox | Default: false |

**🧩 Prompt & Template**

| Property | Type | Notes |
|---|---|---|
| Tên Prompt | title | Descriptive name |
| Nội dung | rich_text | ⚠️ Saved VERBATIM — do not edit |
| Loại | select | System Prompt / User Prompt / Template / Chain |
| Dùng cho | multi_select | Writing / Coding / Research / Analysis |
| Tags | multi_select | |
| Nguồn | url | Original link (if any) |
| Ngày lưu | date | Auto |
| Đã test | checkbox | Default: false |

**📡 Nguồn Theo Dõi (Sources)**

| Property | Type | Notes |
|---|---|---|
| Tên | title | Person / channel / community name |
| URL | url | Profile / channel link |
| Nền tảng | select | YouTube / Twitter-X / LinkedIn / TikTok / Substack / Discord / GitHub |
| Chủ đề | multi_select | LLM / Agents / AI News / Research / Coding / Product |
| Mô tả | rich_text | Why worth following (1–2 sentences) |
| Ngày lưu | date | Auto |
| Đang theo dõi | checkbox | Default: false |

**🗒 Ghi Chú Cá Nhân (Personal Notes)**

| Property | Type | Notes |
|---|---|---|
| Tiêu đề | title | One-line summary (auto-generated if absent) |
| Nội dung | rich_text | ⚠️ Saved VERBATIM — do not edit |
| Tags | multi_select | |
| Trạng thái | select | Quick Note / Needs Action / Processed |
| Ngày lưu | date | Auto |

### Requirements

| Requirement | Details |
|---|---|
| Claude Code | Latest version |
| `NOTION_API_KEY` | Internal integration token (`secret_…`) from [Notion Integrations](https://www.notion.so/my-integrations) |
| `NOTION_PAGE_ID` | ID of your **🧠 AI Wiki** parent page in Notion |

### Installation

1. Copy the skill file to your Claude Code skills directory:

   ```bash
   # Global — available in all projects
   cp ai-wiki-curator.md ~/.claude/skills/

   # Or project-local
   cp ai-wiki-curator.md .claude/skills/
   ```

2. Set environment variables:

   ```bash
   export NOTION_API_KEY="secret_xxxxxxxxxxxxxxxxxxxx"
   export NOTION_PAGE_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

3. Grant your Notion integration access to the target page:
   - Open the page in Notion → **···** → **Add connections** → select your integration.

4. On first use, Claude will ask for confirmation before creating the 5 databases.

### Gotchas

- **G1 — Ambiguous input:** Don't guess. Ask 1 question with 2 specific options.
- **G2 — Duplicates:** Always search Notion first. Never create duplicates without asking.
- **G3 — Missing database:** First run → create all 5 databases + master page. Write to database, not plain page.
- **G4 — URL fetch failure:** Don't stop. Create entry with original URL, leave title for user to fill.
- **G5 — Multiple items:** Process sequentially, one at a time. Summarize all at the end.
- **G6 — Profile vs Post:** `youtube.com/@channel` → Sources. `youtube.com/watch` → Knowledge.
- **G7 — Missing tags:** Infer 2–3 tags from title/content. Never leave empty.
- **G8 — First-time setup:** Ask confirmation before creating all 5 databases at once.
- **G9 — Missing Change Logs:** Auto-create sub-page "📋 Change Logs" inside 🧠 AI Wiki. First entry starts at v1.0. Don't ask user.
- **G10 — Missing "Last updated":** Append text block at bottom of master page. Never leave master page without it.
- **G11 — Unreadable version:** If Change Logs exist but version can't be parsed → fallback: `Version [YYYYMMDD] - DD/MM/YYYY - [Content]`. Never skip Change Logs step.
- **G12 — Tampering with Prompt / Note / Skill content (v1.3.0):** NEVER paraphrase, translate, shorten, or rewrite user-provided content. Only pure formatting allowed: code blocks, line breaks, alignment. If you feel like "improving" wording → STOP. Save verbatim. Violating this rule is the most serious error of the skill.

### License

MIT — see [LICENSE](LICENSE).

---

## Tiếng Việt

### AI Wiki Curator là gì?

**AI Wiki Curator** là một skill cho [Claude Code](https://claude.ai/code) biến một lệnh lưu đơn giản thành một entry đầy đủ trong Notion wiki. Dán link, thả prompt, hoặc gõ ghi chú nhanh — Claude tự động phân loại, điền metadata, lưu vào đúng database Notion, và ghi lại lịch sử thay đổi.

### Câu lệnh kích hoạt

Skill tự động bật khi bạn nói:

| Bạn nói | Claude làm |
|---|---|
| `luu link nay vao wiki` | Lưu URL → phân loại → ghi Notion |
| `them tool nay vao notion` | Tạo entry trong 🛠 Công Cụ AI |
| `ghi lai prompt nay` | Lưu vào 🧩 Prompt & Template |
| `theo doi kenh youtube nay` | Thêm vào 📡 Nguồn Theo Dõi |
| `save bai viet nay` | Lưu vào 📚 Kiến Thức |
| `luu note nay` | Lưu vào 🗒 Ghi Chú Cá Nhân |
| `add this to my AI wiki` | Tương tự, dạng tiếng Anh |
| `save to notion wiki` | Tương tự, dạng tiếng Anh |

> Skill **không** kích hoạt khi bạn chỉ hỏi kiến thức (`what is X?`) hoặc yêu cầu tạo nội dung mới mà không có ý định lưu.

### Cấu trúc Wiki

Lần đầu chạy, skill tạo đủ cấu trúc dưới master page **🧠 AI Wiki**:

| Thành phần | Loại | Nội dung |
|---|---|---|
| 📚 Kiến Thức & Nghiên Cứu | Database | Papers, bài viết, blog, video đơn lẻ |
| 🛠 Công Cụ AI | Database | Tools, apps, APIs, GitHub repos |
| 🧩 Prompt & Template | Database | Prompts, system prompts, templates |
| 📡 Nguồn Theo Dõi | Database | Kênh, profile, newsletter, community |
| 🗒 Ghi Chú Cá Nhân | Database | Raw notes, insights, ý tưởng |
| 📋 Change Logs | Sub-page | Lịch sử cập nhật đầy đủ kèm versioning |

### Workflow 6 Bước

1. **Parse** — Tách URL và text. Fetch title + mô tả từ URL nếu có thể.
2. **Phân loại** — Áp dụng quy tắc classification. Hỏi user nếu không chắc.
3. **Kiểm tra trùng** — Search Notion trước khi tạo. Hỏi cập nhật hay tạo mới nếu đã tồn tại.
4. **Ghi Notion** — Tạo entry đầy đủ theo schema. Tạo database nếu chưa có.

> ⚠️ CONTENT INTEGRITY RULE (v1.3.0)
> Khi lưu Prompt, Note, Skill, Template (bất kỳ nội dung dạng text thuần do user cung cấp):
> - TUYỆT ĐỐI KHÔNG chỉnh sửa từ ngữ, paraphrase, thêm ý, bớt ý, dịch, hoặc viết lại
> - CHỈ ĐƯỢC PHÉP format để dễ đọc: code block, xuống dòng, căn chỉnh khoảng trắng, emoji header
> - Với URL/link: fetch title và mô tả ngắn từ web là bình thường, không vi phạm rule này

5. **Cập nhật metadata** —
   - **5a.** Cập nhật "Cập nhật lần cuối" trên master page 🧠 AI Wiki (append nếu chưa có).
   - **5b.** Prepend entry mới vào 📋 Change Logs với version tự tăng. Đọc version mới nhất từ entry đầu tiên → tăng minor (`v1.0 → v1.1 → v1.2`…). Nếu không đọc được, fallback sang version theo ngày. Format entry: `Version vX.X - DD/MM/YYYY - [Hành động] [Mục]: [Tiêu đề]`.
6. **Xác nhận** — In tóm tắt kèm link Notion.

```
✅ Đã lưu: Cursor AI
📁 Vào mục: 🛠 Công Cụ AI
🏷 Tags: coding, free-tier
📅 Change Log: Version v1.3 - 24/04/2026
🔗 Notion: https://notion.so/…
```

### Phân loại tự động

**Theo URL domain:**

| URL Pattern | Database | Tag |
|---|---|---|
| `arxiv.org`, `paperswithcode.com` | 📚 Kiến Thức | paper |
| `youtube.com/@channel`, `youtube.com/channel/` | 📡 Nguồn Theo Dõi | YouTube |
| `youtube.com/watch`, `youtu.be/{id}` | 📚 Kiến Thức | video |
| `twitter.com/{user}` / `x.com/{user}` (profile) | 📡 Nguồn Theo Dõi | Twitter-X |
| `twitter.com/{user}/status/` (tweet cụ thể) | 📚 Kiến Thức | article |
| `github.com/{user}` (profile) | 📡 Nguồn Theo Dõi | GitHub |
| `github.com/{user}/{repo}` | 🛠 Công Cụ AI | open-source |
| `huggingface.co/papers/` | 📚 Kiến Thức | paper |
| `huggingface.co/spaces/` | 🛠 Công Cụ AI | demo |
| `medium.com`, `dev.to`, `towardsdatascience.com` | 📚 Kiến Thức | article |
| `discord.gg/`, `reddit.com/r/` | 📡 Nguồn Theo Dõi | community |
| `substack.com` (homepage tác giả) | 📡 Nguồn Theo Dõi | Substack |
| `substack.com/p/` (bài viết) | 📚 Kiến Thức | newsletter |
| `linkedin.com/in/`, `linkedin.com/company/` | 📡 Nguồn Theo Dõi | LinkedIn |
| `tiktok.com/@{user}` (profile) | 📡 Nguồn Theo Dõi | TikTok |
| `tiktok.com/@{user}/video/` | 📚 Kiến Thức | video |
| `huggingface.co/models/`, `/datasets/` | 🛠 Công Cụ AI | model |
| `huggingface.co/{user}` (profile) | 📡 Nguồn Theo Dõi | HuggingFace |
| `cursor.sh`, `v0.dev`, `perplexity.ai`, `runway.ml`, `replit.com` | 🛠 Công Cụ AI | tool |
| `openai.com/blog/`, `anthropic.com/news/` | 📚 Kiến Thức | article |

**Theo nội dung text (không có URL):**

| Dấu hiệu | Database | Tag |
|---|---|---|
| Bắt đầu "Act as", "You are a", "Bạn là" | 🧩 Prompt | system-prompt |
| Template có `[placeholder]` | 🧩 Prompt | template |
| Text ngắn, ghi nhanh | 🗒 Ghi Chú | raw-note |
| Text dài, có cấu trúc | 🗒 Ghi Chú | note |

### Schema Database Notion

**📚 Kiến Thức & Nghiên Cứu**

| Property | Type | Ghi chú |
|---|---|---|
| Tiêu đề | title | Tên bài / paper / video |
| URL | url | Link gốc |
| Loại | select | paper / article / video / blog / newsletter |
| Tags | multi_select | LLM, Agents, RAG, Fine-tuning, Vision… |
| Tác giả / Nguồn | rich_text | Tên tác giả hoặc site |
| Tóm tắt | rich_text | 1–3 câu nội dung chính |
| Ngày lưu | date | Auto: hôm nay |
| Đã đọc | checkbox | Default: false |

**🛠 Công Cụ AI**

| Property | Type | Ghi chú |
|---|---|---|
| Tên Tool | title | |
| URL | url | |
| Danh mục | select | Coding / Writing / Image / Video / Search / Productivity / API |
| Tags | multi_select | free-tier, open-source, local, API… |
| Mô tả | rich_text | Tool làm được gì (1–3 câu) |
| Giá | select | Free / Freemium / Trả phí / Open-source |
| Ngày lưu | date | Auto |
| Đã dùng thử | checkbox | Default: false |

**🧩 Prompt & Template**

| Property | Type | Ghi chú |
|---|---|---|
| Tên Prompt | title | Đặt tên mô tả mục đích |
| Nội dung | rich_text | ⚠️ Lưu NGUYÊN VĂN — không chỉnh sửa |
| Loại | select | System Prompt / User Prompt / Template / Chain |
| Dùng cho | multi_select | Viết lách / Coding / Research / Phân tích |
| Tags | multi_select | |
| Nguồn | url | Link gốc (nếu có) |
| Ngày lưu | date | Auto |
| Đã test | checkbox | Default: false |

**📡 Nguồn Theo Dõi**

| Property | Type | Ghi chú |
|---|---|---|
| Tên | title | Tên người / kênh / community |
| URL | url | Link profile / channel |
| Nền tảng | select | YouTube / Twitter-X / LinkedIn / TikTok / Substack / Discord / GitHub |
| Chủ đề | multi_select | LLM / Agents / AI News / Research / Coding / Product |
| Mô tả | rich_text | Tại sao đáng theo dõi (1–2 câu) |
| Ngày lưu | date | Auto |
| Đang theo dõi | checkbox | Default: false |

**🗒 Ghi Chú Cá Nhân**

| Property | Type | Ghi chú |
|---|---|---|
| Tiêu đề | title | Tóm tắt 1 dòng (tự sinh nếu user không cung cấp) |
| Nội dung | rich_text | ⚠️ Lưu NGUYÊN VĂN — không chỉnh sửa |
| Tags | multi_select | |
| Trạng thái | select | Ghi nhanh / Cần xử lý / Đã xử lý |
| Ngày lưu | date | Auto |

### Yêu cầu

| Yêu cầu | Chi tiết |
|---|---|
| Claude Code | Phiên bản mới nhất |
| `NOTION_API_KEY` | Integration token nội bộ (`secret_…`) từ [Notion Integrations](https://www.notion.so/my-integrations) |
| `NOTION_PAGE_ID` | ID trang **🧠 AI Wiki** cha trong Notion |

### Cài đặt

1. Sao chép file skill vào thư mục skills của Claude Code:

   ```bash
   # Toàn cục — dùng được trong mọi project
   cp ai-wiki-curator.md ~/.claude/skills/

   # Hoặc riêng cho project
   cp ai-wiki-curator.md .claude/skills/
   ```

2. Thiết lập biến môi trường:

   ```bash
   export NOTION_API_KEY="secret_xxxxxxxxxxxxxxxxxxxx"
   export NOTION_PAGE_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

3. Cấp quyền cho Notion integration vào trang đích:
   - Mở trang trong Notion → **···** → **Add connections** → chọn integration của bạn.

4. Lần đầu dùng, Claude sẽ hỏi xác nhận trước khi tạo 5 database.

### Lưu ý

- **G1 — Input mơ hồ:** Không tự đoán. Hỏi 1 câu, 2 lựa chọn cụ thể.
- **G2 — Trùng lặp:** Luôn search Notion trước. Không tạo duplicate mà không hỏi.
- **G3 — Database chưa có:** Lần đầu → tạo đủ 5 database + master page. Ghi vào database, không phải page thường.
- **G4 — URL không fetch được:** Không dừng. Tạo entry với URL gốc, title để user điền sau.
- **G5 — Nhiều items cùng lúc:** Xử lý tuần tự từng item. Báo tóm tắt tất cả ở cuối.
- **G6 — Profile vs Post:** `youtube.com/@channel` → Nguồn Theo Dõi. `youtube.com/watch` → Kiến Thức.
- **G7 — Thiếu tags:** Tự suy luận 2–3 tags từ title/content. Không để trống.
- **G8 — Lần đầu khởi tạo:** Hỏi xác nhận trước khi tạo hàng loạt 5 database.
- **G9 — Change Logs chưa tồn tại:** Tự tạo sub-page "📋 Change Logs" bên trong 🧠 AI Wiki. Entry đầu tiên bắt đầu từ v1.0. Không hỏi user.
- **G10 — "Cập nhật lần cuối" chưa có:** Append text block mới ở cuối master page. Không để master page thiếu dòng này.
- **G11 — Không đọc được version:** Nếu Change Logs tồn tại nhưng không parse được version → fallback: `Version [YYYYMMDD] - DD/MM/YYYY - [Nội dung]`. Không bỏ qua bước ghi Change Logs.
- **G12 — Tự ý chỉnh sửa nội dung Prompt / Note / Skill (v1.3.0):** TUYỆT ĐỐI KHÔNG paraphrase, dịch, rút gọn, hay viết lại nội dung do user cung cấp. Chỉ được phép format thuần túy: code block, xuống dòng, căn chỉnh. Nếu cảm thấy muốn "cải thiện" câu chữ → DỪNG. Lưu nguyên văn. Vi phạm rule này là lỗi nghiêm trọng nhất của skill.

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

<div align="center">
  Made with Claude Code · <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/issues">Report an issue</a>
</div>

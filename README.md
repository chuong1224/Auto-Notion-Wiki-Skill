# 🧠 AI Wiki Curator — Claude Code Skill

> A Claude Code skill that automatically saves, classifies, and organizes AI knowledge into your Notion wiki — articles, tools, prompts, sources, and notes, all in one place.

> Skill cho Claude Code tự động lưu, phân loại và tổ chức kiến thức AI vào Notion wiki của bạn — bài viết, công cụ, prompt, nguồn theo dõi và ghi chú, tất cả trong một nơi.

**Version:** 1.2.0 · **License:** MIT

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
5. **Update metadata** — Update "Last updated" timestamp on master page + prepend new entry to 📋 Change Logs with auto-incremented version (`v1.0 → v1.1 → v1.2`).
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

**By text content (no URL):**

| Signal | Database | Tag |
|---|---|---|
| Starts with "Act as", "You are a", "Bạn là" | 🧩 Prompt | system-prompt |
| Template with `[placeholder]` | 🧩 Prompt | template |
| Short quick note | 🗒 Ghi Chú | raw-note |
| Long structured text | 🗒 Ghi Chú | note |

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
5. **Cập nhật metadata** — Cập nhật "Cập nhật lần cuối" trên master page + prepend entry mới vào 📋 Change Logs với version tự tăng (`v1.0 → v1.1 → v1.2`).
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

**Theo nội dung text (không có URL):**

| Dấu hiệu | Database | Tag |
|---|---|---|
| Bắt đầu "Act as", "You are a", "Bạn là" | 🧩 Prompt | system-prompt |
| Template có `[placeholder]` | 🧩 Prompt | template |
| Text ngắn, ghi nhanh | 🗒 Ghi Chú | raw-note |
| Text dài, có cấu trúc | 🗒 Ghi Chú | note |

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

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

<div align="center">
  Made with Claude Code · <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/issues">Report an issue</a>
</div>

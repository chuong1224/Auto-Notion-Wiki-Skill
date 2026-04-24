# AI Wiki Curator — Claude Code Skill

> Automatically turn your codebase into a living Notion wiki, powered by Claude AI.

> Tự động biến codebase của bạn thành một Notion wiki sống động, được hỗ trợ bởi Claude AI.

---

## English

### What is this?

**AI Wiki Curator** is a [Claude Code](https://claude.ai/code) skill that reads your repository and automatically generates or updates structured documentation pages in [Notion](https://notion.so). One command is all it takes to keep your team's wiki in sync with the actual code.

### Features

- Scans your entire repository — source files, configs, `package.json`, `pyproject.toml`, and more
- Generates a full wiki structure: **Home**, **Architecture**, **API Reference**, **Configuration**, **How-To Guides**, and **Changelog**
- Writes directly to Notion using rich blocks (headings, callouts, code blocks with syntax highlighting, tables, toggles)
- Never deletes existing pages — only creates or updates
- Supports multiple output languages (`en`, `vi`, `ja`, `zh`, …)
- Works with any language or framework

### Requirements

| Requirement | Details |
|---|---|
| Claude Code | Latest version — [install guide](https://docs.anthropic.com/en/docs/claude-code) |
| `NOTION_API_KEY` | Internal integration token (`secret_…`) from [Notion Integrations](https://www.notion.so/my-integrations) |
| `NOTION_PAGE_ID` | ID of the Notion parent page where the wiki will live |

### Installation

1. Copy `ai-wiki-curator.md` into your Claude Code skills directory:

   ```bash
   # Global skills (available in all projects)
   cp ai-wiki-curator.md ~/.claude/skills/

   # Or project-local skills
   cp ai-wiki-curator.md .claude/skills/
   ```

2. Set the required environment variables (add them to your shell profile or `.env`):

   ```bash
   export NOTION_API_KEY="secret_xxxxxxxxxxxxxxxxxxxx"
   export NOTION_PAGE_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

3. Make sure your Notion integration has access to the target page:
   - Open the page in Notion
   - Click **···** → **Add connections** → select your integration

### Usage

Navigate to your project root and run:

```
/ai-wiki-curator
```

Claude will scan the repository, build the wiki structure, and push everything to Notion. A summary with Notion URLs is printed when done.

#### Optional arguments

```
/ai-wiki-curator focus=api,config depth=deep language=vi
```

| Argument | Values | Default | Description |
|---|---|---|---|
| `focus` | `all` `api` `config` `guides` `changelog` | `all` | Limit which sections to generate |
| `depth` | `shallow` `normal` `deep` | `normal` | How deeply Claude reads the codebase |
| `language` | `en` `vi` `ja` `zh` … | `en` | Language for generated wiki content |

### Example output

```
✓ Home             https://notion.so/…/Home-xxxx
✓ Architecture     https://notion.so/…/Architecture-xxxx
✓ API Reference    https://notion.so/…/API-Reference-xxxx
✓ Configuration    https://notion.so/…/Configuration-xxxx
✓ How-To Guides    https://notion.so/…/How-To-Guides-xxxx
✓ Changelog        https://notion.so/…/Changelog-xxxx

6 pages created/updated in 42s.
```

### License

MIT — see [LICENSE](LICENSE).

---

## Tiếng Việt

### Đây là gì?

**AI Wiki Curator** là một skill dành cho [Claude Code](https://claude.ai/code) — công cụ đọc toàn bộ repository của bạn và tự động tạo hoặc cập nhật các trang tài liệu có cấu trúc trên [Notion](https://notion.so). Chỉ một lệnh duy nhất là wiki của nhóm bạn đã đồng bộ với code thực tế.

### Tính năng

- Quét toàn bộ repository — source files, config, `package.json`, `pyproject.toml`, và nhiều hơn nữa
- Tạo cấu trúc wiki đầy đủ: **Trang chủ**, **Kiến trúc**, **API Reference**, **Cấu hình**, **Hướng dẫn sử dụng**, và **Changelog**
- Ghi trực tiếp vào Notion với các rich block (tiêu đề, callout, code block có syntax highlighting, bảng, toggle)
- Không bao giờ xóa trang cũ — chỉ tạo mới hoặc cập nhật
- Hỗ trợ nhiều ngôn ngữ đầu ra (`en`, `vi`, `ja`, `zh`, …)
- Tương thích với mọi ngôn ngữ lập trình và framework

### Yêu cầu

| Yêu cầu | Chi tiết |
|---|---|
| Claude Code | Phiên bản mới nhất — [hướng dẫn cài đặt](https://docs.anthropic.com/en/docs/claude-code) |
| `NOTION_API_KEY` | Integration token nội bộ (`secret_…`) từ [Notion Integrations](https://www.notion.so/my-integrations) |
| `NOTION_PAGE_ID` | ID của trang Notion cha nơi wiki sẽ được tạo |

### Cài đặt

1. Sao chép `ai-wiki-curator.md` vào thư mục skills của Claude Code:

   ```bash
   # Skills toàn cục (dùng được trong mọi project)
   cp ai-wiki-curator.md ~/.claude/skills/

   # Hoặc skills riêng cho project
   cp ai-wiki-curator.md .claude/skills/
   ```

2. Thiết lập biến môi trường (thêm vào shell profile hoặc file `.env`):

   ```bash
   export NOTION_API_KEY="secret_xxxxxxxxxxxxxxxxxxxx"
   export NOTION_PAGE_ID="xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
   ```

3. Đảm bảo Notion integration của bạn có quyền truy cập trang đích:
   - Mở trang trong Notion
   - Nhấn **···** → **Add connections** → chọn integration của bạn

### Cách dùng

Di chuyển đến thư mục gốc của project và chạy:

```
/ai-wiki-curator
```

Claude sẽ quét repository, xây dựng cấu trúc wiki và đẩy tất cả lên Notion. Một bản tóm tắt kèm link Notion sẽ được in ra khi hoàn thành.

#### Tham số tùy chọn

```
/ai-wiki-curator focus=api,config depth=deep language=vi
```

| Tham số | Giá trị | Mặc định | Mô tả |
|---|---|---|---|
| `focus` | `all` `api` `config` `guides` `changelog` | `all` | Giới hạn các phần wiki cần tạo |
| `depth` | `shallow` `normal` `deep` | `normal` | Độ sâu Claude đọc codebase |
| `language` | `en` `vi` `ja` `zh` … | `en` | Ngôn ngữ cho nội dung wiki được tạo ra |

### Ví dụ kết quả

```
✓ Trang chủ        https://notion.so/…/Home-xxxx
✓ Kiến trúc        https://notion.so/…/Architecture-xxxx
✓ API Reference    https://notion.so/…/API-Reference-xxxx
✓ Cấu hình         https://notion.so/…/Configuration-xxxx
✓ Hướng dẫn        https://notion.so/…/How-To-Guides-xxxx
✓ Changelog        https://notion.so/…/Changelog-xxxx

6 trang đã được tạo/cập nhật trong 42 giây.
```

### Giấy phép

MIT — xem [LICENSE](LICENSE).

---

<div align="center">
  Made with Claude Code · <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/issues">Report an issue</a>
</div>

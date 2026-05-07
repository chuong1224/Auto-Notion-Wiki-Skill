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
</p>

**Version:** 2.0.0 · **License:** MIT

---

## Version History

| Version | Date       | Changes |
|---------|------------|---------|
| **2.0.0** | 2026-05-07 | Added 💻 Open Source Repos database. Fixed GitHub repo classification. Added Gotchas G13–G15. |
| 1.3.0   | 2026-04-24 | Initial public release. |

---

## English

### What is AI Wiki Curator?

**AI Wiki Curator** is a skill for Claude Code that automatically saves, classifies, and organizes AI-related content into your Notion wiki. It supports articles, tools, prompts, sources to follow, open source repositories, and personal notes.

The skill only activates when you explicitly want to save something. It does not respond to general knowledge questions.

### Trigger Commands

Use any of the following commands:

| Command Example | Result |
|---|---|
| `save this link to wiki` | Save URL to the correct database |
| `add this tool` | Create entry in AI Tools |
| `save this prompt` | Save to Prompt & Template |
| `save this GitHub repo` | Save to Open Source Repos |
| `add this to my AI wiki` | General save command |

Vietnamese commands are also supported (e.g. `luu link nay vao wiki`, `luu repo github nay`).

### Wiki Structure

The skill creates 6 databases under a master page called **🧠 AI Wiki**:

- 📚 Knowledge & Research
- 🛠 AI Tools
- 💻 Open Source Repos
- 🧩 Prompt & Template
- 📡 Sources to Follow
- 📝 Personal Notes
- 📋 Change Logs (sub-page)

### Workflow

1. Parse input
2. Classify content
3. Check for duplicates in Notion
4. Write entry with full metadata
5. Update master page and Change Logs
6. Show confirmation with Notion link

**Important Rule:** User-provided text is always saved **verbatim**. Only formatting is allowed.

### Classification Highlights (v2.0.0)

GitHub repositories (`github.com/{user}/{repo}`) are now saved to the **Open Source Repos** database.

Full classification table and database schemas are available in the file `ai-wiki-curator.md`.

### Installation

Copy `ai-wiki-curator.md` into your Claude Code skills folder and set your Notion API key + page ID.

### License

MIT

---

## Tiếng Việt

### AI Wiki Curator là gì?

**AI Wiki Curator** là skill dành cho Claude Code giúp tự động lưu, phân loại và tổ chức nội dung liên quan đến AI vào Notion wiki của bạn. Skill hỗ trợ bài viết, công cụ, prompt, nguồn theo dõi, repo mã nguồn mở và ghi chú cá nhân.

Skill chỉ kích hoạt khi bạn có ý định lưu thông tin. Nó không trả lời câu hỏi kiến thức chung.

### Câu lệnh kích hoạt

Sử dụng các câu lệnh sau:

| Ví dụ câu lệnh | Kết quả |
|---|---|
| `luu link nay vao wiki` | Lưu URL vào đúng database |
| `them tool nay vao notion` | Tạo entry trong Công Cụ AI |
| `ghi lai prompt nay` | Lưu vào Prompt & Template |
| `luu repo github nay` | Lưu vào Open Source Repos |
| `add this to my AI wiki` | Lệnh lưu tổng quát |

### Cấu trúc Wiki

Skill tự động tạo 6 database dưới trang chính **🧠 AI Wiki**:

- 📙 Kiến Thức & Nghiên Cứu
- 🛠 Công Cụ AI
- 💻 Open Source Repos
- 🧩 Prompt & Template
- 📡 Nguồn Theo Dõi
- 📝 Ghi Chú Cá Nhân
- 📋 Change Logs (trang con)

### Quy trình làm việc

1. Phân tích input
2. Phân loại nội dung
3. Kiểm tra trùng lặp trong Notion
4. Ghi entry đầy đủ metadata
5. Cập nhật trang chính và Change Logs
6. Hiển thị xác nhận kèm link Notion

**Quy tắc quan trọng:** Nội dung do người dùng cung cấp được lưu **nguyên văn**. Chỉ cho phép định dạng.

### Điểm nổi bật phân loại (v2.0.0)

Repo GitHub (`github.com/{user}/{repo}`) bây giờ được lưu vào database **Open Source Repos**.

Bảng phân loại đầy đủ và schema database có trong file `ai-wiki-curator.md`.

### Cài đặt

Sao chép file `ai-wiki-curator.md` vào thư mục skills của Claude Code và thiết lập Notion API key + page ID.

### Giấy phép

MIT

---

<div align="center">
  Made with Claude Code · <a href="https://github.com/chuong1224/Auto-Notion-Wiki-Skill/issues">Báo lỗi / Report issue</a>
</div>

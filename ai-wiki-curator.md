---
name: ai-wiki-curator
version: 2.0.0
description: Kich hoat khi user muon luu noi dung vao AI Wiki tren Notion, them kien thuc AI, phan loai link bai viet, ghi chu, tool, prompt, nguon theo doi, repo open source. BAT CU KHI NAO user noi: luu link nay, them tool nay, ghi lai prompt nay, theo doi kenh nay, save cai nay vao wiki, luu repo nay, save github repo, luu open source nay, add vao notion, update wiki AI. Vi du: "luu link nay vao wiki", "them tool nay vao notion", "ghi lai prompt nay", "theo doi kenh youtube nay", "save bai viet nay", "luu repo github nay", "save this open source repo", "add this repo to wiki", "luu note nay", "add this to my AI wiki". Khong kich hoat khi user chi hoi kien thuc (what is X?) hoac yeu cau tao noi dung moi khong co y dinh luu.
---

# AI Wiki Curator

Nhận input từ user → phân loại tự động → ghi vào đúng database trong Notion AI Wiki
→ cập nhật "Cập nhật lần cuối" trên master page → ghi Change Logs.

## Cấu Trúc Wiki (6 Database + 1 Sub-page)

Khi wiki chưa tồn tại, tạo đủ cấu trúc dưới master page **🧠 AI Wiki**:

| Thành phần | Loại | Nội dung |
|---|---|---|
| 📚 Kiến Thức & Nghiên Cứu | database | Papers, bài viết, blog, video đơn lẻ |
| 🛠 Công Cụ AI | database | Tools thương mại, apps, APIs, SaaS |
| 💻 Open Source Repos | database | GitHub/GitLab repos, free OSS projects |
| 🧩 Prompt & Template | database | Prompts, system prompts, templates |
| 📡 Nguồn Theo Dõi | database | Kênh, profile, newsletter, community |
| 🗒 Ghi Chú Cá Nhân | database | Raw notes, insights, ý tưởng |
| 📋 Change Logs | sub-page | Lịch sử cập nhật wiki |

---

## Workflow (6 Bước)

**B1 — Parse**: Tách URL và text. Nếu có URL → web_fetch lấy title + mô tả ngắn.
Nếu fetch thất bại → vẫn tiếp tục với URL gốc.

**B2 — Phân loại**: Áp dụng bảng Classification bên dưới.
Nếu không chắc → hỏi user 1 câu, đưa 2 lựa chọn cụ thể.

**B3 — Kiểm tra trùng**: Dùng notion-search trước khi tạo.
Nếu đã tồn tại → hỏi "Cập nhật hay tạo mới?"

**B4 — Ghi Notion**: Tạo entry với đầy đủ properties theo schema bên dưới.
Nếu database chưa có → tạo mới.

> ⚠️ **CONTENT INTEGRITY RULE (v1.3.0 — BẮT BUỘC)**
>
> Khi lưu **Prompt, Note, Skill, Template** (bất kỳ nội dung dạng text thuần do user cung cấp):
> - **TUYỆT ĐỐI KHÔNG** chỉnh sửa từ ngữ, paraphrase, thêm ý, bớt ý, dịch, hoặc viết lại theo cách khác
> - **CHỈ ĐƯỢC PHÉP** format để dễ đọc: code block, xuống dòng, căn chỉnh khoảng trắng, emoji section header
> - **Quy tắc kiểm tra**: nếu đọc lại nội dung lưu mà không thấy đúng 100% từng từ của user → sai
> - Với URL/link: fetch title và mô tả ngắn từ web là bình thường, không vi phạm rule này

**B5 — Cập nhật Wiki Metadata**:

*5a — Cập nhật "Cập nhật lần cuối" trên master page 🧠 AI Wiki:*
- Tìm text block chứa "Cập nhật lần cuối" ở cuối master page
- Nếu tìm thấy → cập nhật ngày hôm nay: `_Cập nhật lần cuối: DD/MM/YYYY_`
- Nếu chưa có → append text block mới ở cuối trang với nội dung trên

*5b — Ghi vào sub-page 📋 Change Logs:*
- Tìm sub-page "📋 Change Logs" bên trong 🧠 AI Wiki
- Nếu chưa có → tạo sub-page mới, tiêu đề "📋 Change Logs", version bắt đầu v1.0
- Nếu đã có → đọc entry đầu tiên để lấy version mới nhất → tăng minor version (v1.0 → v1.1)
- Thêm entry MỚI Ở ĐẦU TRANG (prepend), không append cuối:
  `Version vX.X - DD/MM/YYYY - [Nội dung cập nhật]`
- Nội dung cập nhật = auto-generate: "[Action] [Category]: [Title]"
  Ví dụ: "Them tool: Cursor AI", "Luu repo: LangChain", "Luu paper: MoE Survey"

**B6 — Xác nhận**:
```
✅ Đã lưu: [Tên]
📁 Vào mục: [Database]
🏷 Tags: [tag1, tag2]
📅 Change Log: Version vX.X - DD/MM/YYYY
🔗 Notion: [link]
```

---

## Classification Rules

### Theo URL Domain

| URL Pattern | Database | Tag |
|---|---|---|
| arxiv.org, paperswithcode.com, openreview.net | 📚 Kiến Thức | paper |
| youtube.com/@name, youtube.com/channel/ | 📡 Nguồn Theo Dõi | YouTube |
| youtube.com/watch, youtu.be/{videoID} | 📚 Kiến Thức | video |
| twitter.com/{user} hoặc x.com/{user} (profile) | 📡 Nguồn Theo Dõi | Twitter-X |
| twitter.com/{user}/status/ (tweet cụ thể) | 📚 Kiến Thức | article |
| linkedin.com/in/, linkedin.com/company/ | 📡 Nguồn Theo Dõi | LinkedIn |
| tiktok.com/@{user} (profile) | 📡 Nguồn Theo Dõi | TikTok |
| tiktok.com/@{user}/video/ | 📚 Kiến Thức | video |
| substack.com (homepage tác giả) | 📡 Nguồn Theo Dõi | Substack |
| substack.com/p/ (bài viết cụ thể) | 📚 Kiến Thức | newsletter |
| github.com/{user} (profile, 1 segment) | 📡 Nguồn Theo Dõi | GitHub |
| **github.com/{user}/{repo}** | **💻 Open Source Repos** | **repo** |
| **gitlab.com/{user}/{repo}** | **💻 Open Source Repos** | **repo** |
| **codeberg.org/{user}/{repo}** | **💻 Open Source Repos** | **repo** |
| huggingface.co/papers/ | 📚 Kiến Thức | paper |
| huggingface.co/spaces/ | 🛠 Công Cụ AI | demo |
| huggingface.co/models/, /datasets/ | 🛠 Công Cụ AI | model |
| huggingface.co/{user} (profile) | 📡 Nguồn Theo Dõi | HuggingFace |
| cursor.sh, v0.dev, perplexity.ai, runway.ml, replit.com... | 🛠 Công Cụ AI | tool |
| medium.com, dev.to, towardsdatascience.com | 📚 Kiến Thức | article |
| openai.com/blog/, anthropic.com/news/ | 📚 Kiến Thức | article |
| discord.gg/, reddit.com/r/ | 📡 Nguồn Theo Dõi | community |

> **Phân biệt 🛠 Công Cụ AI vs 💻 Open Source Repos:**
> - 🛠 = Sản phẩm thương mại / SaaS / có hosted version → user dùng trực tiếp
> - 💻 = Repo code tự deploy / fork / đóng góp → user cần clone/build

### Theo Nội Dung Text (Không Có URL)

| Dấu hiệu | Database | Tag |
|---|---|---|
| Bắt đầu "Act as", "You are a", "Bạn là", có "###" | 🧩 Prompt | system-prompt |
| Dạng template có placeholder [X] | 🧩 Prompt | template |
| Text ngắn, ghi chú nhanh | 🗒 Ghi Chú | raw-note |
| Text dài, có cấu trúc, nhiều khái niệm | 🗒 Ghi Chú | note |

---

## Notion Schema (6 Database)

### 📚 Kiến Thức & Nghiên Cứu
| Property | Type | Ghi chú |
|---|---|---|
| Tiêu đề | title | Tên bài / paper / video |
| URL | url | Link gốc |
| Loại | select | paper / article / video / blog / newsletter |
| Tags | multi_select | LLM, Agents, RAG, Fine-tuning, Vision... |
| Tác giả / Nguồn | rich_text | Tên tác giả hoặc site |
| Tóm tắt | rich_text | 1–3 câu nội dung chính |
| Ngày lưu | date | Auto: hôm nay |
| Đã đọc | checkbox | Default: false |

### 🛠 Công Cụ AI
| Property | Type | Ghi chú |
|---|---|---|
| Tên Tool | title | |
| URL | url | |
| Danh mục | select | Coding / Writing / Image / Video / Search / Productivity / API |
| Tags | multi_select | free-tier, local, API... |
| Mô tả | rich_text | Tool làm được gì (1–3 câu) |
| Giá | select | Free / Freemium / Trả phí |
| Ngày lưu | date | Auto |
| Đã dùng thử | checkbox | Default: false |

### 💻 Open Source Repos
| Property | Type | Ghi chú |
|---|---|---|
| Tên Repo | title | Tên repo (ví dụ: langchain, ollama) |
| URL | url | Link repo gốc |
| Ngôn ngữ | select | Python / JavaScript / TypeScript / Rust / Go / C++ / Other |
| Danh mục | select | LLM / Agent / RAG / Fine-tuning / Framework / Inference / Utils / Tools / Dataset |
| Tags | multi_select | local-run, no-GPU, GPU-required, beginner-friendly, production-ready, MIT, Apache-2.0 |
| Mô tả | rich_text | Repo làm được gì + lý do lưu (1–3 câu) |
| License | select | MIT / Apache-2.0 / GPL / BSD / Other / Unknown |
| Stars ước tính | select | dưới-1k / 1k-10k / 10k-50k / 50k+ |
| Ngày lưu | date | Auto |
| Đã clone / thử | checkbox | Default: false |

### 🧩 Prompt & Template
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

### 📡 Nguồn Theo Dõi
| Property | Type | Ghi chú |
|---|---|---|
| Tên | title | Tên người / kênh / community |
| URL | url | Link profile / channel |
| Nền tảng | select | YouTube / Twitter-X / LinkedIn / TikTok / Substack / Discord / GitHub / HuggingFace |
| Chủ đề | multi_select | LLM / Agents / AI News / Research / Coding / Product |
| Mô tả | rich_text | Tại sao đáng theo dõi (1–2 câu) |
| Ngày lưu | date | Auto |
| Đang theo dõi | checkbox | Default: false |

### 🗒 Ghi Chú Cá Nhân
| Property | Type | Ghi chú |
|---|---|---|
| Tiêu đề | title | Tóm tắt 1 dòng (tự sinh nếu user không cung cấp) |
| Nội dung | rich_text | ⚠️ Lưu NGUYÊN VĂN — không chỉnh sửa |
| Tags | multi_select | |
| Trạng thái | select | Ghi nhanh / Cần xử lý / Đã xử lý |
| Ngày lưu | date | Auto |

---

## Gotchas

**G1 — Input mơ hồ**: Không tự đoán. Hỏi 1 câu, 2 lựa chọn cụ thể.
**G2 — Trùng lặp**: Luôn search Notion trước. Không tạo duplicate mà không hỏi.
**G3 — Database chưa có**: Lần đầu → tạo đủ 6 database + master page. Ghi vào database, không phải page thường.
**G4 — URL không fetch được**: Không dừng. Tạo entry với URL gốc, title để user điền sau.
**G5 — Nhiều items cùng lúc**: Xử lý tuần tự từng item. Báo tóm tắt tất cả ở cuối.
**G6 — Profile vs Post**: youtube.com/@channel → Nguồn Theo Dõi. youtube.com/watch → Kiến Thức.
**G7 — Thiếu tags**: Tự suy luận 2–3 tags từ title/content. Không để trống.
**G8 — Lần đầu khởi tạo**: Hỏi xác nhận trước khi tạo hàng loạt 6 database.
**G9 — Change Logs chưa tồn tại**: Tạo sub-page "📋 Change Logs" ngay bên trong 🧠 AI Wiki. Entry đầu tiên bắt đầu từ v1.0. Không hỏi user, tự tạo.
**G10 — "Cập nhật lần cuối" chưa có trên master page**: Append text block mới ở cuối trang. Không để master page thiếu dòng này.
**G11 — Không đọc được version từ Change Logs**: Nếu page tồn tại nhưng không parse được version → dùng ngày làm fallback: `Version [YYYYMMDD] - DD/MM/YYYY - [Nội dung]`. Không bỏ qua bước ghi Change Logs.
**G12 — Tự ý chỉnh sửa nội dung Prompt / Note / Skill**: TUYỆT ĐỐI KHÔNG paraphrase, dịch, rút gọn, hay viết lại nội dung do user cung cấp. Chỉ được phép format thuần túy. Vi phạm rule này là lỗi nghiêm trọng nhất.
**G13 — Repo có hosted version thương mại**: Một số repo (ví dụ: Supabase, Milvus) vừa có bản OSS vừa có SaaS. Nếu user paste link GitHub repo → 💻 Open Source. Nếu paste link landing page (supabase.com) → 🛠 Công Cụ AI. Không tự chuyển loại.
**G14 — Thiếu thông tin repo**: License và Stars thường không có ngay. Để giá trị "Unknown" / "dưới-1k" làm default nếu không fetch được. Không block việc lưu vì thiếu 2 field này.
**G15 — HuggingFace model repo vs code repo**: huggingface.co/models/ → 🛠 Công Cụ AI (model). Nếu user paste link GitHub của cùng project đó → 💻 Open Source Repos (repo code). Hai entry riêng biệt, không merge.

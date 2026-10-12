# 5 plugin cho Claude Code — trạng thái và cách cài

Nguồn: video https://www.youtube.com/watch?v=7IiDEWOUc6c

| # | Plugin | Nguồn | Trạng thái trong repo này |
|---|--------|-------|---------------------------|
| 1 | Claude Code Setup | Anthropic (chính thức) | ✅ Đã bật trong `.claude/settings.json` |
| 2 | Claude Security | Anthropic (chính thức, beta) | ✅ Đã bật trong `.claude/settings.json` |
| 3 | Claude Mem | Bên thứ ba (thedotmack) | ✅ Đã bật trong `.claude/settings.json` |
| 4 | Headroom | Bên thứ ba (headroomlabs-ai) | ✅ Đã bật (cần cài thêm CLI Python) |
| 5 | OmniRoute | Bên thứ ba (proxy, không phải plugin) | ⚠️ Chỉ nên cài trên máy cá nhân |

Khi mở repo này trong Claude Code, chương trình sẽ hỏi bạn có tin tưởng
(trust) các plugin được khai báo không — chọn **Yes** để cài.

---

## 1. Claude Code Setup (chính thức)

Quét mã nguồn và đề xuất hooks, skills, MCP server, sub-agent phù hợp.
Chỉ đọc, không sửa file.

```text
/plugin install claude-code-setup@claude-plugins-official
```

Cách dùng: hỏi Claude *"Hãy đề xuất các automation cho dự án này"*.

## 2. Claude Security (chính thức, beta)

Nhiều agent quét lỗ hổng bảo mật, kiểm tra lại từng phát hiện, rồi tạo
bản vá (patch) để bạn xem trước khi áp dụng. Mỗi lần quét tính vào hạn
mức sử dụng của gói.

```text
/plugin install claude-security@claude-plugins-official
/claude-security
```

## 3. Claude Mem (bên thứ ba)

Lưu ngữ cảnh dự án giữa các phiên và tự nạp lại khi mở phiên mới.
Cần Node.js 18+. Dữ liệu lưu ở `~/.claude-mem` trên máy, nên **không giữ
được trong phiên cloud** (container bị xoá sau mỗi phiên).

```text
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem@thedotmack
```

Khởi động lại Claude Code sau khi cài.

## 4. Headroom (bên thứ ba)

Nén log, kết quả công cụ và đoạn mã lặp trước khi gửi cho mô hình để
tiết kiệm token (nhà phát triển công bố giảm 60–95%, chưa được kiểm
chứng độc lập). Plugin chỉ là hook khởi động; phần chính là CLI Python.

```bash
pip install "headroom-ai[all]"
headroom wrap claude          # gỡ bằng: headroom unwrap claude
```

```text
/plugin marketplace add headroomlabs-ai/headroom
/plugin install headroom@headroom-marketplace
```

## 5. OmniRoute (bên thứ ba — cẩn thận)

OmniRoute **không phải plugin**. Đây là một gateway chạy trên máy bạn,
nhận yêu cầu của Claude Code và chuyển sang nhiều nhà cung cấp AI khác
(nhiều nhà cung cấp miễn phí). Con số "1,6 tỷ token miễn phí" chỉ đến từ
blog/video, chưa được xác minh.

Lưu ý trước khi dùng:

- Mã nguồn và prompt của bạn sẽ được gửi tới **nhà cung cấp khác, không
  phải Anthropic**. Không dùng cho mã nguồn hoặc bản vẽ mật của công ty
  nếu chưa được phép.
- Chất lượng câu trả lời khi chuyển sang mô hình miễn phí thường thấp hơn.
- Không đặt `ANTHROPIC_BASE_URL` trong `.claude/settings.json` của repo:
  mọi phiên (kể cả phiên cloud) sẽ lỗi khi gateway không chạy.

Cài trên máy cá nhân:

```bash
npm install -g omniroute
omniroute                     # mở dashboard ở http://localhost:20128
```

Sau khi cấu hình nhà cung cấp trong dashboard, chỉ trỏ Claude Code tới
gateway cho từng lần chạy (kiểm tra lại địa chỉ chính xác trong
dashboard của OmniRoute):

```bash
ANTHROPIC_BASE_URL=http://localhost:20128 claude
```

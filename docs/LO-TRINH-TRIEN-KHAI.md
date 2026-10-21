# Lộ trình xây dựng hệ thống tự động: Marketing – Quản trị – Báo cáo

Mục tiêu: Claude đọc dữ liệu kinh doanh, đề xuất kế hoạch marketing và quản trị, thực thi
những việc được phép, rồi gửi báo cáo định kỳ cho chủ doanh nghiệp.

## Kiến trúc tổng quan

```
 [Nguồn dữ liệu]            [Bộ não]                    [Hành động]            [Báo cáo]
 Google Sheets/Drive  --->  Repo GitHub này      --->   Gửi email, tạo nội dung,  ---> Email Gmail
 TikTok Ads / Shop          (CLAUDE.md, quy tắc,        điều chỉnh quảng cáo,          Google Docs
 Facebook/Meta, Zalo OA     kỹ năng, mẫu báo cáo)       cập nhật bảng tính             Dashboard
 Phần mềm bán hàng          + Lịch chạy tự động
```

Giai đoạn đầu **không cần mua máy chủ hay phần cứng**: Claude Code on the web chạy trên
máy ảo đám mây, dữ liệu nằm trên Google Drive/Sheets, lịch chạy bằng "scheduled tasks".

## Giai đoạn 0 — Xác định kinh doanh (1–3 ngày, việc của chủ doanh nghiệp)

- [ ] Ngành hàng, sản phẩm chủ lực, giá bán, biên lợi nhuận
- [ ] Khách hàng mục tiêu, đối thủ chính
- [ ] Kênh bán hàng hiện có (cửa hàng, website, Shopee, TikTok Shop, Facebook, Zalo…)
- [ ] Mục tiêu 3 tháng tới, đo bằng con số (doanh thu, đơn hàng, chi phí/đơn…)
- [ ] Ngân sách marketing theo tháng
- [ ] **Giới hạn quyền tự động**: việc nào Claude tự làm, việc nào phải chờ duyệt

## Giai đoạn 1 — Nền tảng dữ liệu (tuần 1)

Tạo trên Google Drive một thư mục `HE-THONG-KINH-DOANH` gồm:

| Tệp | Nội dung |
|---|---|
| `Ho-so-doanh-nghiep` (Doc) | Thương hiệu, giọng văn, khách hàng, đối thủ, mục tiêu |
| `Ban-hang` (Sheet) | Ngày, sản phẩm, số lượng, doanh thu, kênh |
| `San-pham` (Sheet) | Mã, tên, giá vốn, giá bán, tồn kho |
| `Chi-phi-marketing` (Sheet) | Ngày, kênh, chiến dịch, chi phí, kết quả |
| `Khach-hang` (Sheet) | Không ghi dữ liệu nhạy cảm không cần thiết |
| `KPI` (Sheet) | Chỉ tiêu và kết quả theo tuần/tháng |
| `Bao-cao/` | Nơi Claude lưu các báo cáo đã tạo |

Nguyên tắc: một cấu trúc cột cố định, cập nhật đều đặn (tay hoặc xuất từ phần mềm bán hàng).

## Giai đoạn 2 — Kết nối (tuần 1–2)

Đã kết nối sẵn trong phiên làm việc: **Gmail, Google Drive, Google Sheets, TikTok for Business, GitHub**.

Cần cân nhắc thêm tuỳ kênh: Meta (Facebook/Instagram) Ads, Google Ads/Analytics,
Zalo OA, sàn TMĐT, phần mềm bán hàng (KiotViet, Sapo, Haravan…) — qua connector có sẵn,
API, hoặc xuất file định kỳ vào Drive.

Khoá API/mật khẩu **không bao giờ** ghi vào repo; đặt trong phần "secrets" của môi trường.

## Giai đoạn 3 — "Bộ não" trong repo (tuần 2)

```
CLAUDE.md                  # Bối cảnh doanh nghiệp + quy tắc làm việc + giới hạn quyền
docs/                      # Lộ trình, hồ sơ, quy trình
.claude/skills/
  bao-cao-tuan/            # Cách lập báo cáo tuần
  de-xuat-ke-hoach/        # Cách phân tích dữ liệu và đề xuất kế hoạch
  toi-uu-quang-cao/        # Quy tắc tăng/giảm/tắt quảng cáo
templates/                 # Mẫu báo cáo, mẫu kế hoạch
```

## Giai đoạn 4 — Tự động hoá theo lịch (tuần 3)

| Lịch | Việc |
|---|---|
| Hằng ngày 8h | Đọc số liệu bán hàng + quảng cáo, cảnh báo bất thường qua email |
| Thứ Hai 8h | Báo cáo tuần + đề xuất kế hoạch tuần (chờ duyệt) |
| Ngày 1 hằng tháng | Báo cáo tháng, so sánh KPI, kế hoạch tháng mới |

## Giai đoạn 5 — Mở rộng quyền thực thi (từ tháng 2)

1. **Chỉ đọc & báo cáo** — Claude phân tích, không thay đổi gì.
2. **Đề xuất & chờ duyệt** — bạn trả lời email/duyệt, Claude mới làm.
3. **Tự thực thi trong giới hạn** — ví dụ: tự tắt quảng cáo lỗ quá X đồng, tự điều chỉnh
   ngân sách trong ±20%, mọi thay đổi đều ghi nhật ký và báo cáo lại.

## Khi nào cần máy chủ riêng (VPS)?

Chỉ khi cần: nhận webhook thời gian thực (đơn hàng mới, tin nhắn khách), chạy cơ sở dữ liệu
riêng, công cụ như n8n, hoặc chatbot trả lời khách 24/7. Cấu hình khởi điểm: VPS 2 vCPU /
4 GB RAM, Ubuntu (khoảng 5–15 USD/tháng). Chưa cần ở giai đoạn đầu.

## Chi phí dự kiến giai đoạn đầu

- Gói Claude có Claude Code (Pro hoặc Max)
- Google Workspace (tuỳ chọn; Gmail miễn phí vẫn dùng được)
- Ngân sách quảng cáo (như hiện tại)

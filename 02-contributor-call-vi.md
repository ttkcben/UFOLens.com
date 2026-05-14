# GitHub — Bài đăng 2/3 · Kêu gọi đóng góp / "good first issues"

**Sử dụng làm:** một Discussion được ghim ("Đóng góp & good first issues") hoặc phần giới thiệu trong CONTRIBUTING.md.
**Từ khóa:** mã nguồn mở, đóng góp, good first issue, i18n, bản địa hóa, OCR, Python, TypeScript, Vitest, pytest, khả năng truy cập, UAP, dữ liệu mở
**Siêu liên kết:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Đóng góp cho ufolens.com

[ufolens.com](https://www.ufolens.com) biến [kho lưu trữ PURSUE UAP](https://www.war.gov/ufo) của Bộ Chiến tranh Hoa Kỳ thành một nền tảng đa ngôn ngữ, có thể tìm kiếm với một [API công khai](https://www.ufolens.com/api/v1). Dự án gồm hai nửa — một pipeline nhập liệu bằng Python cục bộ (`pipeline/`) và một ứng dụng biên bằng TypeScript/Hono (`worker/`) — gặp nhau tại một giao diện duy nhất: một gói SQL + tài sản đã được xuất bản.

Bạn không cần bất kỳ thông tin đăng nhập đám mây nào để đóng góp. Các mô-đun lõi của pipeline chỉ dùng thư viện chuẩn và các bài kiểm thử của Worker chạy dựa trên bộ nhớ trong.

### Cài đặt

```bash
# pipeline
python3 -m pytest pipeline/tests/          # tất cả nên xanh, không cần cài đặt pip

# worker
cd worker && npm install
npm run typecheck && npm test              # tsc --noEmit + vitest
npm run dev                                # localhost:8787
```

### Những lĩnh vực cần sự giúp đỡ nhất

**i18n / bản địa hóa** — `worker/src/i18n/ui-strings.json` là nguồn của các chuỗi giao diện người dùng. Việc đánh giá từ người bản ngữ cho bất kỳ ngôn ngữ nào không phải tiếng Anh đều rất có giá trị: phát hiện các bản dịch máy ngượng nghịu, sửa lỗi RTL/bố cục, cải thiện các trường hợp đặc biệt về đàm phán ngôn ngữ.

**Chất lượng OCR** — xử lý trước tốt hơn các bản quét đánh máy cũ trước khi OCR; một bộ công cụ đánh giá so sánh engine mã nguồn mở với Tesseract dự phòng trên các trang mẫu.

**Khả năng truy cập** — kiểm tra các trang được kết xuất (`worker/src/render/`) theo tiêu chuẩn WCAG; CSP rất nghiêm ngặt (không có `unsafe-inline`), vì vậy các giải pháp phải hoạt động trong khuôn khổ đó.

**Tính tiện dụng của API** — `worker/src/routes/` — phân trang, lọc, mô tả OpenAPI, các client ví dụ.

**Tính bền vững của Pipeline** — thêm các hướng giảm cấp mượt mà, báo cáo tiến độ tốt hơn, các trường hợp đặc biệt trong phát hiện thay đổi (`pipeline/lib/delta.py`).

**Tài liệu** — `docs/20260511/` (繁體中文; `00-*` là chỉ mục). Chúng tôi hoan nghênh các bản dịch tài liệu thiết kế sang tiếng Anh.

### Quy tắc cơ bản

- Tất cả các đường dẫn đều là tương đối — dự án phải có thể di chuyển được giữa các máy. Không có đường dẫn tuyệt đối được mã hóa cứng.
- Không thêm phụ thuộc pip vào một mô-đun *cốt lõi* của pipeline. Các giai đoạn tùy chọn có thể sử dụng các gói tùy chọn, và phải tự giảm cấp một cách mượt mà khi không có chúng.
- Không làm yếu đi máy trạng thái chỉ tiến — đó là giới hạn chi phí.
- Không giới thiệu các biểu trưng chính thức của chính phủ Hoa Kỳ, và không thêm bất cứ thứ gì đảo ngược các phần đã được biên tập lại trong nguồn.
- Các thay đổi về schema D1 ảnh hưởng đến **hai** tệp: `pipeline/lib/manifest_schema.sql` và `db/schema.sql`.
- Viết test cho mã nguồn mới. Sử dụng thông điệp commit theo quy ước (Conventional Commits).

Hãy đọc `CLAUDE.md` và `docs/20260511/00-*` trước, sau đó mở một issue để thảo luận về bất kỳ điều gì có tính cấu trúc trước khi tạo PR.

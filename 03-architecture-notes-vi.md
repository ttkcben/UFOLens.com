# GitHub — Bài đăng 3/3 · Ghi chú kiến trúc (Discussion kiểu ADR)

**Sử dụng làm:** một Discussion dưới mục "Show and tell" / "Architecture", hoặc làm tài liệu ADR ban đầu trong `docs/`.
**Từ khóa:** kiến trúc, ADR, máy trạng thái chỉ tiến, LLM cục bộ, Ollama, OCR, điện toán biên, CSP, tiêu đề bảo mật, pipeline dữ liệu, kỹ thuật chi phí, tệp kê khai SQLite, D1, R2, KV
**Siêu liên kết:** https://www.ufolens.com · https://www.ufolens.com/api/v1 · https://www.war.gov/ufo

---

## Tại sao ufolens.com được xây dựng theo cách này

Ghi chú về ba quyết định đã định hình [ufolens.com](https://www.ufolens.com) (bản dựng lại đa ngôn ngữ, có thể tìm kiếm của [kho lưu trữ PURSUE UAP](https://www.war.gov/ufo)). Hoan nghênh các bình luận / phản biện.

### 1. Pipeline là một máy trạng thái chỉ tiến — một cách có chủ đích

Các trạng thái: `discovered → downloaded → ocr_done → translated → published`. Một tài liệu chỉ di chuyển về phía trước, và chỉ khi có việc cần làm. Nội dung đã xuất bản không bao giờ được xử lý lại trừ khi một bộ phát hiện thay đổi thấy nguồn thực sự đã thay đổi.

**Tại sao:** OCR + dịch thuật là các hoạt động tốn kém, và kho lưu trữ ngày càng lớn. Một pipeline "chạy lại tất cả để cho chắc" có chi phí không giới hạn. Việc làm cho các chuyển đổi ngược là không thể khiến cho việc phát sinh hóa đơn không kiểm soát là không thể. Giới hạn chi phí là một thuộc tính của đồ thị trạng thái, chứ không phải sự cảnh giác của người vận hành.

**Đánh đổi:** việc di chuyển schema và xử lý lại một cách có chủ đích được cố tình làm cho khó khăn. Một sự đánh đổi chấp nhận được.

### 2. OCR và dịch thuật chạy trên một LLM cục bộ, không phải qua API đám mây

OCR: engine mã nguồn mở, Tesseract CLI dự phòng. Dịch thuật + NER: Gemma qua Ollama, trên một laptop Apple Silicon.

**Tại sao:** chi phí biên cho mỗi tài liệu bằng không; có thể tái tạo (mô hình + prompt cố định); và bước tìm nạp đã phải chạy từ một IP dân cư (nguồn nằm sau Akamai Bot Manager — `curl` nhận lỗi 403), vì vậy dù sao cũng cần một chiếc laptop trong vòng lặp.

**Đánh đổi:** chất lượng dịch thuật thấp hơn so với một mô hình hàng đầu. Đối với một kho dữ liệu tham khảo nơi bản gốc tiếng Anh luôn chỉ cách một cú nhấp chuột, điều đó là ổn. Chúng tôi không khẳng định các bản dịch là có thẩm quyền.

### 3. Hai nửa của dự án chia sẻ chính xác một giao diện duy nhất: một gói đã xuất bản

Pipeline không bao giờ ghi trực tiếp vào cơ sở dữ liệu sản phẩm. Nó tạo ra `{ SQL, tệp kê khai tài sản, danh sách xóa cache }`. "Xuất bản" = áp dụng gói đó về phía trước (đẩy SQL lên SQL DB biên, đồng bộ tài sản lên lưu trữ đối tượng, xóa các khóa cache được đặt tên).

**Tại sao:** phía cục bộ và phía biên có thể phát triển độc lập; gói có thể được xem xét; và "triển khai dữ liệu" có cùng một hình dạng mỗi lần. Worker là một ứng dụng TypeScript/Hono nhỏ — CSP nghiêm ngặt (không có `unsafe-inline`; JSON-LD nội tuyến được ghim bằng sha256), đàm phán `Accept-Language` + quốc gia→ngôn ngữ, bộ đệm trang KV 30 ngày, cron dọn dẹp hàng ngày — và nó không bao giờ cần biết dữ liệu được tạo ra như thế nào.

**Đánh đổi:** một thay đổi schema D1 ảnh hưởng đến hai tệp (`pipeline/lib/manifest_schema.sql`, `db/schema.sql`). Một sự bảo hiểm rẻ tiền.

### Những điều không thể thỏa hiệp đã được tích hợp vào hành vi

- Không liên kết với chính phủ Hoa Kỳ; không có biểu trưng chính thức.
- Các phần biên tập trong nguồn được bảo tồn, không bao giờ bị đảo ngược.
- Video được ghi nhận thuộc về DVIDS / AARO.
- `Tdm-Reservation: 1` + `X-Robots-Tag: noai, noimageai` trên toàn trang — có thể được lập chỉ mục tìm kiếm, đã chọn không tham gia thu thập dữ liệu cho AI.

Trực tuyến: https://www.ufolens.com · API: https://www.ufolens.com/api/v1

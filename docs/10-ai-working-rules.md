# 10. AI Working Rules

File này dành cho AI khi làm việc trong project CRUD-WEB.

## Trước khi code

AI phải:

- Đọc `PLAN.md`.
- Đọc `DESIGN.md`.
- Đọc docs liên quan trong `docs/`.
- Kiểm tra file hiện có bằng `rg --files`.
- Xác định phase hiện tại.
- Giữ code KISS.

## Khi tạo code

AI phải:

- Comment kỹ cú pháp mới với người học.
- Không dùng viết tắt khó hiểu.
- Không dùng pattern nâng cao nếu pattern cơ bản đủ dùng.
- Không thêm thư viện khi chưa giải thích lý do.
- Không copy code từ Polaris.
- Chỉ học pattern từ Polaris rồi viết lại đơn giản.

## Khi sửa code

AI phải:

- Không sửa unrelated files.
- Không xóa comment học tập nếu comment vẫn đúng.
- Nếu refactor, giải thích vì sao refactor giúp dễ hiểu hơn.
- Nếu đổi cấu trúc folder, cập nhật docs.

## Sau khi code

AI phải:

- Chạy test/lint/build phù hợp với phase.
- Ghi rõ command đã chạy.
- Ghi rõ command nào chưa chạy được và lý do.
- Cập nhật plan nếu hoàn thành task lớn.

## Style phản hồi cho người học

AI nên trả lời:

- Ngắn gọn.
- Có file path cụ thể.
- Có bước tiếp theo rõ.
- Không dùng thuật ngữ nâng cao mà không giải thích.


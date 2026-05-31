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

## Mục tiêu của AI trong project này

AI không chỉ viết code cho chạy. AI phải giúp người học hiểu code.

Vì vậy mỗi thay đổi nên đạt 3 mục tiêu:

1. App tiến gần hơn tới CRUD-WEB hoàn chỉnh.
2. Code vẫn đơn giản và dễ đọc.
3. Người mới có thể học được cú pháp hoặc pattern từ code đó.

Nếu một giải pháp "ngầu" nhưng khó hiểu, ưu tiên giải pháp đơn giản hơn.

## Quy tắc khi thêm công nghệ mới

Trước khi thêm thư viện/framework mới, AI phải giải thích trong code/docs:

- Thư viện giải quyết vấn đề gì?
- Vì sao chưa dùng code thuần được nữa?
- Cú pháp 20/80 cần học là gì?
- File nào bị ảnh hưởng?
- Command cài đặt/chạy là gì?

Ví dụ khi thêm Zod:

```txt
Zod được dùng để validate dữ liệu trước khi ghi database.
Frontend validation giúp UX, nhưng backend vẫn cần validation vì API có thể bị gọi trực tiếp.
```

## Comment policy chi tiết

Nên comment khi:

- Có cú pháp mới với người học.
- Có logic CRUD quan trọng.
- Có khác biệt client/server.
- Có validation hoặc database query.
- Có đoạn code dễ gây nhầm.

Không nên comment khi:

- Dòng code quá hiển nhiên.
- Comment chỉ lặp lại tên function.
- Comment sai hoặc đã lỗi thời.

Ví dụ comment tốt:

```ts
// Tạo object job mới từ dữ liệu form.
// Ở phase chưa có database, id và thời gian được tạo ở frontend.
const newJob: Job = {
  id: crypto.randomUUID(),
  company: jobInput.company,
  position: jobInput.position,
  status: jobInput.status,
  note: jobInput.note,
  createdAt: new Date().toISOString(),
  updatedAt: new Date().toISOString(),
};
```

Ví dụ comment không cần:

```ts
// Set loading to true.
setIsLoading(true);
```

## Cách AI nên tổ chức task

Với task code mới:

1. Đọc docs/plan/design.
2. Kiểm tra file hiện tại.
3. Xác định phase.
4. Tạo hoặc sửa ít file nhất có thể.
5. Comment đủ cho phần mới.
6. Chạy command kiểm tra.
7. Báo lại file đã sửa và bước tiếp theo.

Với task sửa lỗi:

1. Reproduce lỗi nếu có thể.
2. Đọc error message.
3. Tìm nguyên nhân.
4. Sửa nguyên nhân, không chỉ che symptom.
5. Chạy lại flow lỗi.
6. Ghi rõ đã verify thế nào.

## Khi AI không chắc

AI không nên đoán bừa trong các trường hợp:

- Có thể xóa dữ liệu.
- Có thể đổi kiến trúc lớn.
- Có thể thêm thư viện nặng.
- Có thể ảnh hưởng nhiều phase.

Trong các trường hợp này, AI nên hỏi ngắn gọn hoặc ghi rõ assumption.

## Quy tắc cập nhật docs

Nếu AI thêm một trong các thứ sau, phải cập nhật docs:

- Tech stack mới.
- Command mới.
- Folder structure mới.
- Pattern code mới.
- Quy tắc validation mới.
- API route mới.
- Database model mới.

Docs cần giúp người sau hiểu tại sao code đang được tổ chức như vậy.

## Quy tắc với Polaris React

Polaris là nguồn tham khảo pattern, không phải nguồn để copy code.

AI được học:

- Cách đặt component folder.
- Cách export qua `index.ts`.
- Cách dùng design tokens.
- Cách viết props rõ.
- Cách đặt docs gần project.

AI không được:

- Copy component implementation.
- Mang complexity của Polaris vào project nhỏ.
- Tạo abstraction chỉ vì Polaris có abstraction.

## Done message của AI nên có gì?

Sau mỗi task, AI nên báo:

- Đã sửa/tạo file nào.
- Nội dung chính là gì.
- Đã chạy command nào.
- Có command nào chưa chạy được không.
- Bước tiếp theo nên làm gì.

Ví dụ:

```txt
Đã tạo phase 1 trong apps/vanilla-job-tracker với index.html, style.css, script.js.
Hiện app render danh sách job từ array và có form thêm job.
Đã kiểm tra bằng cách mở file HTML trong browser.
Bước tiếp theo nên thêm delete/edit và localStorage.
```

## Checklist cho AI

- [ ] Code có đủ rõ cho người mới không?
- [ ] Có dùng trick không cần thiết không?
- [ ] Có comment phần công nghệ mới không?
- [ ] Có cập nhật docs nếu thêm pattern mới không?
- [ ] Có giữ scope nhỏ không?
- [ ] Có verify sau khi sửa không?

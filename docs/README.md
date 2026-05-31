# Docs README

Thư mục này chứa tài liệu học nền tảng cho project CRUD-WEB. Tất cả tài liệu viết bằng Tiếng Việt, hướng tới người mới biết HTML, CSS, JavaScript cơ bản.

## Thứ tự đọc

1. `01-environment.md`
2. `02-git-and-github.md`
3. `03-html-css-js-crud.md`
4. `04-react-20-80.md`
5. `05-typescript-20-80.md`
6. `06-nextjs-20-80.md`
7. `07-api-crud.md`
8. `08-database-prisma.md`
9. `09-testing-debugging.md`
10. `10-ai-working-rules.md`

## Cách dùng docs

- Đọc lý thuyết trước khi đọc code của phase tương ứng.
- Khi gặp cú pháp mới trong code, quay lại docs để xem giải thích.
- Khi AI thêm công nghệ mới, AI phải cập nhật docs tương ứng.
- Docs không cần quá học thuật; ưu tiên ví dụ nhỏ, dễ chạy, dễ hiểu.

## Nguyên tắc học

- Làm từng phase, không nhảy thẳng full-stack.
- Code chạy được trước, refactor sau.
- Mỗi ngày học một nhóm cú pháp nhỏ.
- Khi đọc code, hỏi 3 câu: dữ liệu ở đâu, ai thay đổi dữ liệu, UI render từ dữ liệu nào.

## Không cần đọc hết một lượt

Bạn không nên đọc toàn bộ docs từ đầu đến cuối rồi mới code. Cách đó dễ bị ngợp vì mỗi file thuộc một giai đoạn khác nhau.

Cách học đề xuất:

1. Đọc file liên quan đến phase hiện tại.
2. Code một tính năng nhỏ.
3. Chạy thử.
4. Gặp lỗi thì quay lại docs.
5. Khi phase ổn, mới đọc file của phase tiếp theo.

Ví dụ:

- Đang làm HTML/CSS/JS thuần: đọc `03-html-css-js-crud.md`.
- Đang chuyển sang React: đọc `04-react-20-80.md`.
- Đang thêm TypeScript: đọc `05-typescript-20-80.md`.
- Đang thêm API/database: đọc `07-api-crud.md` và `08-database-prisma.md`.

## Mục tiêu từng file

### `01-environment.md`

Đọc khi chuẩn bị máy và project.

Bạn cần hiểu:

- Node.js dùng để làm gì.
- `package.json` là gì.
- Vì sao không commit `node_modules`.
- `.env.example` khác `.env.local` thế nào.
- Vì sao project JavaScript/TypeScript không cần Python `venv` cho app chính.

### `02-git-and-github.md`

Đọc trước khi commit code.

Bạn cần hiểu:

- `git status`, `git add`, `git commit`, `git push`.
- `.gitignore`.
- Vì sao nên commit nhỏ.
- Cách đưa project lên GitHub để dùng cho CV.

### `03-html-css-js-crud.md`

Đọc trước khi code phase 1.

Bạn cần hiểu:

- CRUD là gì.
- Data là nguồn sự thật.
- Render list từ array.
- Thêm/sửa/xóa bằng JavaScript.
- Search/filter không phá mảng gốc.
- Lưu dữ liệu vào `localStorage`.

### `04-react-20-80.md`

Đọc sau khi CRUD thuần chạy được.

Bạn cần hiểu:

- Component.
- JSX.
- Props.
- State.
- Event.
- Controlled input.
- Callback từ component con lên component cha.

### `05-typescript-20-80.md`

Đọc khi đã có React component cơ bản.

Bạn cần hiểu:

- `type`.
- Union type.
- Type cho props.
- Type cho function.
- `Job` khác `JobInput` thế nào.
- Vì sao không nên lạm dụng `any`.

### `06-nextjs-20-80.md`

Đọc khi chuẩn bị chuyển app sang Next.js.

Bạn cần hiểu:

- File-based routing.
- `page.tsx`, `layout.tsx`, `route.ts`.
- Server component và client component.
- Khi nào cần `'use client'`.
- API route trong Next.js.

### `07-api-crud.md`

Đọc trước khi frontend gọi API.

Bạn cần hiểu:

- HTTP method.
- Request/response.
- Status code.
- Validate input ở backend.
- Response format thống nhất.

### `08-database-prisma.md`

Đọc trước khi thêm database.

Bạn cần hiểu:

- Database lưu dữ liệu lâu dài.
- Table/row/column.
- Primary key.
- Prisma schema.
- Migration.
- Prisma Client CRUD query.

### `09-testing-debugging.md`

Đọc song song trong quá trình code.

Bạn cần hiểu:

- Debug theo lớp: UI, state, API, database, environment.
- Dùng Network tab.
- Log có mục đích.
- Test tay trước, automated test sau.

### `10-ai-working-rules.md`

Đọc nếu bạn dùng AI để code cùng.

Bạn cần hiểu:

- AI phải giữ code đơn giản.
- AI phải comment cú pháp mới.
- AI phải cập nhật docs khi thêm pattern/công nghệ mới.
- AI không copy code từ Polaris.

## Mỗi phase nên dừng ở đâu?

### Phase 1 dừng khi

- CRUD thuần thêm/sửa/xóa được.
- Search/filter được.
- Reload không mất dữ liệu vì có `localStorage`.
- Bạn giải thích được `renderJobs()` làm gì.

### Phase 2 dừng khi

- CRUD chạy bằng React state.
- Không còn dùng `document.querySelector` để cập nhật UI.
- Component đã được tách vừa phải.
- Bạn giải thích được props/state/callback.

### Phase 3 dừng khi

- Có type `Job`, `JobInput`, `JobStatus`.
- Props có type.
- Không dùng `any` bừa bãi.
- TypeScript báo lỗi khi truyền sai status.

### Phase 4 dừng khi

- UI responsive cơ bản.
- Form/table/search/filter nhìn rõ.
- Có empty/loading/error state.
- Không có text tràn khỏi button/input.

### Phase 5 dừng khi

- Next.js app chạy được.
- `/jobs` hiển thị danh sách.
- Frontend gọi được `/api/jobs`.
- API có GET/POST/PATCH/DELETE cơ bản.

### Phase 6 dừng khi

- Dữ liệu lưu trong database.
- Tắt server mở lại vẫn còn dữ liệu.
- Prisma schema được commit.
- `.env.example` có `DATABASE_URL`.

## Câu hỏi tự kiểm tra khi đọc code

Khi mở một file code, hãy tự hỏi:

1. File này chịu trách nhiệm gì?
2. Dữ liệu chính trong file là gì?
3. Dữ liệu đó đến từ đâu?
4. Ai được phép thay đổi dữ liệu đó?
5. Khi dữ liệu đổi, UI cập nhật bằng cách nào?
6. Nếu lỗi xảy ra, lỗi có thể nằm ở UI, state, API hay database?

Nếu trả lời được các câu này, bạn không chỉ copy code mà đang thật sự hiểu project.

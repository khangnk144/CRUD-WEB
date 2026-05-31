# CRUD-WEB Pet Project Plan

Mục tiêu của project này là xây một website CRUD nhỏ nhưng đủ giống dự án thực tế để đưa vào CV. Người học hiện chỉ biết HTML, CSS, JavaScript cơ bản, vì vậy mọi công nghệ mới phải được dùng theo hướng KISS: code rõ ràng, ít mẹo, ít viết tắt, comment kỹ các dòng khó hiểu.

## 1. Ý tưởng project

Tên đề xuất: **Job Application Tracker**.

Ứng dụng giúp quản lý danh sách công việc đã ứng tuyển:

- Xem danh sách job.
- Thêm job mới.
- Sửa thông tin job.
- Xóa job.
- Tìm kiếm theo công ty hoặc vị trí.
- Lọc theo trạng thái ứng tuyển.
- Lưu dữ liệu tạm ở frontend trước, sau đó chuyển dần sang API và database.

Lý do chọn project này:

- CRUD rõ ràng, dễ hiểu.
- Có form, table/list, filter, search, validation, API, database.
- Dễ giải thích trong CV.
- Có thể mở rộng thêm auth, dashboard, export CSV khi đã vững.

## 2. Tech stack cuối cùng

Project sẽ đi theo từng phase, không học tất cả cùng lúc.

| Phase | Công nghệ | Lý do dùng |
| --- | --- | --- |
| 1 | HTML, CSS, JavaScript | Ôn CRUD bằng DOM thuần, hiểu bản chất trước framework |
| 2 | React | Học component, props, state, event, form |
| 3 | TypeScript | Học type cho dữ liệu, props, function |
| 4 | Tailwind CSS | Style nhanh, dễ maintain, phổ biến thực tế |
| 5 | Next.js | Có routing, page, API routes/server actions, cấu trúc gần thực tế |
| 6 | Prisma + SQLite trước, PostgreSQL sau | Học database và ORM với độ phức tạp tăng dần |
| 7 | Auth.js hoặc custom auth đơn giản | Mỗi user có danh sách job riêng |
| 8 | Vitest/React Testing Library/Playwright | Test logic, component, flow CRUD |

Ghi chú môi trường:

- Với Node.js project, cách cô lập chính là dùng `node_modules` riêng trong project, lockfile, `.nvmrc` hoặc Volta để khóa version Node.
- `venv` là khái niệm của Python. Project này không cần Python venv cho app chính.
- Nếu sau này có script Python phụ trợ, tạo `.venv` trong thư mục `tools/` hoặc root project và ghi rõ trong docs.

## 3. Quy tắc code bắt buộc

Các quy tắc này dành cho cả người học và AI.

- Ưu tiên code dễ đọc hơn code ngắn.
- Không dùng trick, không lạm dụng one-liner.
- Không viết tắt tên biến gây khó hiểu.
- Mỗi file chỉ nên có một trách nhiệm chính.
- Function nhỏ, tên function nói rõ việc nó làm.
- Khi dùng công nghệ mới, comment kỹ dòng code quan trọng.
- Comment giải thích **vì sao** và **dòng này làm gì**, không comment kiểu thừa.
- Không thêm thư viện nếu JavaScript/React/Next đã đủ giải quyết.
- Không tối ưu sớm.
- Làm xong mỗi phase phải có checklist test thủ công.

Ví dụ style comment mong muốn khi học React:

```tsx
// type Job mô tả hình dạng của một job trong ứng dụng.
// TypeScript sẽ báo lỗi nếu thiếu field hoặc truyền sai kiểu dữ liệu.
type Job = {
  id: string;
  company: string;
  position: string;
  status: 'Applied' | 'Interview' | 'Offer' | 'Rejected';
};

// useState lưu danh sách job trong bộ nhớ của component.
// Khi gọi setJobs, React sẽ render lại UI để hiển thị dữ liệu mới.
const [jobs, setJobs] = useState<Job[]>([]);
```

## 4. Cấu trúc thư mục mục tiêu

```txt
CRUD-WEB/
  PLAN.md
  DESIGN.md
  README.md
  docs/
    README.md
    01-environment.md
    02-git-and-github.md
    03-html-css-js-crud.md
    04-react-20-80.md
    05-typescript-20-80.md
    06-nextjs-20-80.md
    07-api-crud.md
    08-database-prisma.md
    09-testing-debugging.md
    10-ai-working-rules.md
  apps/
    vanilla-job-tracker/
    web/
  packages/
    ui/
    shared/
  prisma/
    schema.prisma
  public/
  scripts/
  .env.example
  .editorconfig
  .prettierrc
  package.json
  pnpm-lock.yaml
```

Giải thích:

- `docs/`: tài liệu học lý thuyết bằng Tiếng Việt.
- `apps/vanilla-job-tracker/`: bản HTML/CSS/JS thuần để học bản chất CRUD.
- `apps/web/`: bản Next.js chính để đưa vào CV.
- `packages/ui/`: component UI tái sử dụng, lấy cảm hứng từ Polaris nhưng đơn giản hơn.
- `packages/shared/`: type, constant, helper dùng chung.
- `prisma/`: database schema và migration.
- `public/`: ảnh, favicon, asset tĩnh.
- `scripts/`: script phụ trợ nếu thật sự cần.

## 5. Pattern học từ Polaris React

Tham khảo `D:\CRUD-WEB\polaris-react`, áp dụng ở mức đơn giản:

- Component có thư mục riêng.
- Mỗi component export qua `index.ts`.
- Type/props đặt gần component.
- Style được gom rõ ràng, không rải lung tung.
- Có docs/design decision để người sau hiểu lý do.
- Có format/lint/test script ngay từ đầu.
- Dùng design token cho màu, spacing, radius thay vì hard-code khắp nơi.

Không áp dụng:

- Không làm monorepo phức tạp ngay từ đầu nếu chưa cần.
- Không tự viết design system lớn.
- Không dùng Storybook trong phase đầu.
- Không copy component Polaris nguyên xi.
- Không dùng abstraction chỉ để trông "pro".

Chi tiết xem [DESIGN.md](./DESIGN.md).

## 6. Roadmap theo phase

### Phase 0: Setup nền tảng

Mục tiêu: có repo sạch, quy ước rõ, tài liệu rõ.

Task:

- [ ] Tạo `README.md` giới thiệu project.
- [ ] Tạo `docs/` và đọc các file nền tảng.
- [ ] Tạo `.editorconfig`, `.prettierrc`, `.gitignore`.
- [ ] Chọn package manager: ưu tiên `pnpm`.
- [ ] Tạo `.nvmrc` hoặc dùng Volta để khóa Node version.
- [ ] Tạo `.env.example`.
- [ ] Tạo checklist command trong README.

Người học cần hiểu:

- Node.js là runtime để chạy JavaScript ngoài browser.
- npm/pnpm là công cụ cài package.
- `package.json` giống "bảng điều khiển" của project.
- `node_modules` là thư viện cài riêng cho project.
- Lockfile giúp người khác cài đúng version thư viện.

Done khi:

- Chạy được command kiểm tra format/lint cơ bản.
- Repo có docs và plan.
- Người khác clone về đọc README là biết bắt đầu từ đâu.

### Phase 1: CRUD bằng HTML/CSS/JavaScript thuần

Thư mục: `apps/vanilla-job-tracker/`.

Mục tiêu: hiểu CRUD không cần framework.

Tính năng:

- [ ] Render danh sách job từ array.
- [ ] Form thêm job.
- [ ] Nút sửa job.
- [ ] Nút xóa job.
- [ ] Select đổi status.
- [ ] Search theo company/position.
- [ ] Filter theo status.
- [ ] Lưu vào `localStorage`.

Cú pháp 20/80 cần học:

- `document.querySelector`
- `addEventListener`
- `array.map`
- `array.filter`
- `array.find`
- `array.push`
- `array.splice` hoặc `filter` để xóa
- `localStorage.getItem`
- `localStorage.setItem`
- `JSON.stringify`
- `JSON.parse`
- `event.preventDefault`

Quy tắc comment:

- Comment rõ chỗ lấy DOM.
- Comment rõ chỗ render lại UI.
- Comment rõ chỗ cập nhật array.
- Comment rõ chỗ đọc/ghi `localStorage`.

Done khi:

- Người học thêm/sửa/xóa job được.
- Reload page không mất dữ liệu.
- Code không có function quá dài.

### Phase 2: CRUD bằng React

Thư mục tạm: `apps/react-job-tracker/` hoặc chuyển thẳng vào `apps/web/` nếu dùng Next.js.

Mục tiêu: học component, props, state, event.

Component đề xuất:

```txt
src/
  components/
    JobForm/
      JobForm.tsx
      index.ts
    JobList/
      JobList.tsx
      index.ts
    JobItem/
      JobItem.tsx
      index.ts
    StatusBadge/
      StatusBadge.tsx
      index.ts
  types/
    job.ts
  App.tsx
```

Tính năng:

- [ ] Tách UI thành component.
- [ ] Dùng `useState` để lưu jobs.
- [ ] Truyền dữ liệu bằng props.
- [ ] Truyền callback từ cha xuống con.
- [ ] Làm form controlled input.
- [ ] Tách type `Job`.

Cú pháp 20/80 cần học:

- `function ComponentName()`
- JSX
- props
- `useState`
- `onClick`
- `onChange`
- `onSubmit`
- conditional rendering
- list rendering với `.map`
- `key`

Done khi:

- Không dùng `document.querySelector` trong React.
- UI render từ state.
- Tất cả thao tác CRUD cập nhật state.

### Phase 3: TypeScript căn bản

Mục tiêu: dùng TypeScript để tránh lỗi dữ liệu CRUD.

Type cần có:

```ts
export type JobStatus = 'Applied' | 'Interview' | 'Offer' | 'Rejected';

export type Job = {
  id: string;
  company: string;
  position: string;
  status: JobStatus;
  note: string;
  createdAt: string;
  updatedAt: string;
};
```

Cú pháp 20/80 cần học:

- `type`
- `interface`
- union type
- array type: `Job[]`
- function parameter type
- function return type
- optional property: `note?: string`
- generic cơ bản: `useState<Job[]>([])`

Done khi:

- Không còn dùng `any` nếu không có lý do rõ.
- Props của component đều có type.
- Function CRUD có parameter type rõ ràng.

### Phase 4: Tailwind CSS và UI system nhỏ

Mục tiêu: giao diện sạch, dễ đọc, giống app quản trị thực tế.

Không làm landing page. Màn hình đầu tiên là dashboard CRUD.

UI cần có:

- Header đơn giản.
- Toolbar search/filter.
- Form thêm/sửa job.
- Table hoặc list responsive.
- Empty state.
- Loading state.
- Error state.
- Confirm trước khi xóa.

Design hướng admin tool:

- Màu nền trung tính.
- Spacing đều.
- Button rõ primary/secondary/danger.
- Table dễ scan.
- Không dùng hero lớn.
- Không dùng gradient trang trí.
- Không dùng card lồng card.

Done khi:

- Desktop và mobile không vỡ layout.
- Text không bị tràn khỏi button/input/card.
- Màu, spacing, radius được dùng nhất quán.

### Phase 5: Next.js app

Thư mục: `apps/web/`.

Mục tiêu: chuyển app thành web app thực tế.

Cấu trúc đề xuất:

```txt
apps/web/
  app/
    layout.tsx
    page.tsx
    jobs/
      page.tsx
    api/
      jobs/
        route.ts
  components/
  lib/
  styles/
```

Tính năng:

- [ ] Trang `/jobs` hiển thị CRUD.
- [ ] API route `GET /api/jobs`.
- [ ] API route `POST /api/jobs`.
- [ ] API route `PUT /api/jobs/:id` hoặc `PATCH`.
- [ ] API route `DELETE /api/jobs/:id`.
- [ ] Frontend gọi API bằng `fetch`.
- [ ] Loading/error khi gọi API.

Cú pháp 20/80 cần học:

- `app/page.tsx`
- `layout.tsx`
- client component với `'use client'`
- server route handler
- `Request`
- `Response.json`
- `fetch`
- async/await
- try/catch

Done khi:

- Refresh page vẫn lấy dữ liệu từ API.
- API có status code đúng cơ bản.
- Frontend không còn tự giả lập toàn bộ dữ liệu.

### Phase 6: Database với Prisma

Mục tiêu: dữ liệu lưu thật.

Bắt đầu bằng SQLite để dễ setup, sau đó đổi PostgreSQL.

Model Prisma dự kiến:

```prisma
model Job {
  id        String   @id @default(cuid())
  company   String
  position  String
  status    String
  note      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

Task:

- [ ] Cài Prisma.
- [ ] Tạo `schema.prisma`.
- [ ] Tạo migration.
- [ ] Viết helper `prisma`.
- [ ] API dùng `findMany`.
- [ ] API dùng `create`.
- [ ] API dùng `update`.
- [ ] API dùng `delete`.
- [ ] Validate input trước khi ghi database.

Cú pháp 20/80 cần học:

- model
- field type
- `@id`
- `@default`
- migration
- `prisma.job.findMany`
- `prisma.job.create`
- `prisma.job.update`
- `prisma.job.delete`

Done khi:

- Tắt server mở lại vẫn còn dữ liệu.
- Database schema được commit.
- `.env.example` có `DATABASE_URL`.

### Phase 7: Form validation

Mục tiêu: tránh dữ liệu rác.

Thư viện đề xuất:

- `zod` cho validation schema.
- React Hook Form chỉ thêm sau khi đã hiểu controlled form cơ bản.

Rule:

- Company không được rỗng.
- Position không được rỗng.
- Status phải thuộc danh sách cho phép.
- Note có giới hạn ký tự.

Done khi:

- Lỗi hiển thị gần field.
- API cũng validate, không chỉ frontend.
- Người dùng không tạo được job thiếu dữ liệu bắt buộc.

### Phase 8: Auth và user riêng

Mục tiêu: mỗi user có dữ liệu riêng.

Chỉ làm sau khi CRUD + DB đã vững.

Tính năng:

- [ ] Đăng ký.
- [ ] Đăng nhập.
- [ ] Đăng xuất.
- [ ] Job thuộc về user.
- [ ] User A không xem/sửa/xóa job của user B.

Done khi:

- API kiểm tra user trước khi trả dữ liệu.
- Database có `User` và relation `Job.userId`.

### Phase 9: Test và chất lượng

Mục tiêu: có project CV đáng tin.

Test tối thiểu:

- Unit test helper filter/search.
- Component test form.
- API test CRUD.
- E2E test flow thêm/sửa/xóa job.

Command mục tiêu:

```bash
pnpm lint
pnpm type-check
pnpm test
pnpm test:e2e
pnpm build
```

Done khi:

- Các command trên pass.
- README ghi rõ cách chạy test.
- Có screenshot hoặc GIF demo.

## 7. Checklist AI trước khi sửa code

AI phải làm các bước này trước khi tạo/sửa code:

- [ ] Đọc `PLAN.md`.
- [ ] Đọc `DESIGN.md`.
- [ ] Đọc docs liên quan trong `docs/`.
- [ ] Kiểm tra cấu trúc hiện tại bằng `rg --files`.
- [ ] Không sửa file ngoài phạm vi task.
- [ ] Nếu thêm tech mới, cập nhật docs tương ứng.
- [ ] Nếu code dùng cú pháp mới với người học, thêm comment dễ hiểu.
- [ ] Sau khi sửa, chạy command kiểm tra phù hợp.
- [ ] Cập nhật checklist trong plan hoặc issue nếu task hoàn thành.

## 8. Definition of Done cho project CV

Project đủ đưa vào CV khi có:

- CRUD job đầy đủ.
- UI responsive.
- Database thật.
- Validation frontend và backend.
- README có hướng dẫn chạy.
- `.env.example`.
- Ảnh screenshot.
- Ít nhất vài test quan trọng.
- Deploy demo hoặc video demo.
- Code có comment học tập ở phần công nghệ mới.
- Không chứa secret trong repo.

## 9. Thứ tự đọc tài liệu

Người học nên đọc theo thứ tự:

1. [docs/README.md](./docs/README.md)
2. [docs/01-environment.md](./docs/01-environment.md)
3. [docs/02-git-and-github.md](./docs/02-git-and-github.md)
4. [docs/03-html-css-js-crud.md](./docs/03-html-css-js-crud.md)
5. [docs/04-react-20-80.md](./docs/04-react-20-80.md)
6. [docs/05-typescript-20-80.md](./docs/05-typescript-20-80.md)
7. [docs/06-nextjs-20-80.md](./docs/06-nextjs-20-80.md)
8. [docs/07-api-crud.md](./docs/07-api-crud.md)
9. [docs/08-database-prisma.md](./docs/08-database-prisma.md)
10. [docs/09-testing-debugging.md](./docs/09-testing-debugging.md)
11. [docs/10-ai-working-rules.md](./docs/10-ai-working-rules.md)


# DESIGN.md

File này ghi lại hướng thiết kế, pattern code và quyết định kiến trúc cho CRUD-WEB. Mục tiêu là giúp người học và AI không phải đoán lại từ đầu mỗi lần sửa project.

## 1. Tham khảo Polaris React

Nguồn tham khảo local: `D:\CRUD-WEB\polaris-react`.

Những điểm học được:

- Polaris tổ chức code theo component, mỗi component có thư mục riêng.
- Component public export qua `index.ts`.
- Props được type rõ ràng.
- Style được gom trong file riêng, ví dụ CSS module.
- Component có test/story gần nơi định nghĩa.
- Repo có quy ước format, lint, test, build.
- Design token giúp màu, spacing, border, radius nhất quán.
- Documentation là một phần của project, không phải phụ kiện.

Những điểm không áp dụng ngay:

- Không dùng monorepo phức tạp ở phase đầu.
- Không tạo component library lớn.
- Không dùng Storybook khi chưa cần.
- Không copy cách viết type quá nâng cao.
- Không viết utility trừu tượng nếu chỉ dùng một lần.

## 2. Nguyên tắc thiết kế UI

CRUD-WEB là admin/productivity app, không phải landing page.

Ưu tiên:

- Giao diện gọn, rõ, dễ scan.
- Table/list là trung tâm.
- Form dễ nhập liệu.
- Action rõ ràng: Add, Edit, Delete, Save, Cancel.
- Trạng thái rõ: loading, empty, error, success.
- Responsive đủ tốt cho mobile.

Tránh:

- Hero section lớn.
- Gradient trang trí.
- Card lồng card.
- Animation không cần thiết.
- Text mô tả dài trong UI.
- Màu quá sặc sỡ.

## 3. Design tokens đơn giản

Nếu dùng Tailwind, vẫn nên thống nhất các token trong `tailwind.config`.

Token đề xuất:

```ts
// Các token này giúp UI nhất quán.
// Khi muốn đổi theme, sửa một chỗ thay vì sửa từng component.
const themeTokens = {
  colors: {
    background: '#f6f7f9',
    surface: '#ffffff',
    text: '#1f2937',
    mutedText: '#6b7280',
    border: '#d1d5db',
    primary: '#2563eb',
    danger: '#dc2626',
    success: '#16a34a',
    warning: '#d97706',
  },
  radius: {
    small: '4px',
    medium: '8px',
  },
};
```

Quy tắc:

- Radius card/input/button tối đa khoảng `8px`, trừ khi có lý do.
- Không dùng nhiều biến thể cùng một màu làm UI bị một màu.
- Button nguy hiểm dùng màu danger.
- Text phụ dùng muted color.
- Border nhẹ để tách vùng, không lạm dụng shadow.

## 4. Component pattern

Pattern đề xuất:

```txt
components/
  JobForm/
    JobForm.tsx
    index.ts
  JobTable/
    JobTable.tsx
    index.ts
  StatusBadge/
    StatusBadge.tsx
    index.ts
```

Ví dụ:

```tsx
// JobFormProps mô tả dữ liệu component cần nhận từ component cha.
// Viết rõ props giúp người mới biết component này phụ thuộc vào gì.
type JobFormProps = {
  initialJob?: Job;
  onSubmit: (jobInput: JobInput) => void;
  onCancel: () => void;
};

// Component chỉ lo hiển thị form và gửi dữ liệu ra ngoài.
// Component này không tự gọi API để giữ trách nhiệm đơn giản.
export function JobForm({initialJob, onSubmit, onCancel}: JobFormProps) {
  // Code form sẽ nằm ở đây.
}
```

Quy tắc:

- Component UI không tự làm quá nhiều việc.
- Page/container chịu trách nhiệm gọi API và giữ state lớn.
- Component nhận dữ liệu qua props.
- Component gửi event ra ngoài qua callback.
- Không dùng global state khi `useState` đủ dùng.

## 5. Naming convention

Tên file:

- Component: `JobForm.tsx`, `StatusBadge.tsx`.
- Type: `job.ts`.
- Helper: `formatDate.ts`, `filterJobs.ts`.
- API helper: `jobsApi.ts`.

Tên biến:

- Dùng tên đầy đủ: `selectedJob`, không dùng `sj`.
- Boolean bắt đầu bằng `is`, `has`, `can`: `isLoading`, `hasError`, `canSubmit`.
- Event handler bắt đầu bằng `handle`: `handleSubmit`, `handleDeleteJob`.
- Function thao tác dữ liệu dùng động từ rõ: `createJob`, `updateJob`, `deleteJob`.

## 6. Data model

Model học tập:

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

export type JobInput = {
  company: string;
  position: string;
  status: JobStatus;
  note: string;
};
```

Tách `Job` và `JobInput` vì:

- `Job` là dữ liệu đã lưu, có `id`, `createdAt`, `updatedAt`.
- `JobInput` là dữ liệu người dùng nhập từ form, chưa có `id`.

## 7. API design

Route mục tiêu:

| Method | Path | Ý nghĩa |
| --- | --- | --- |
| GET | `/api/jobs` | Lấy danh sách job |
| POST | `/api/jobs` | Tạo job mới |
| GET | `/api/jobs/:id` | Lấy chi tiết một job |
| PATCH | `/api/jobs/:id` | Sửa một phần job |
| DELETE | `/api/jobs/:id` | Xóa job |

Response nên thống nhất:

```ts
type ApiSuccess<T> = {
  data: T;
};

type ApiError = {
  error: {
    message: string;
  };
};
```

Không cần làm quá phức tạp ở phase đầu. Chỉ cần status code đúng và error message dễ hiểu.

## 8. Error, loading, empty state

Mỗi màn hình gọi API nên có:

- `isLoading`: đang tải dữ liệu.
- `errorMessage`: có lỗi.
- empty state: danh sách rỗng.
- success state: hiển thị dữ liệu.

Không giấu lỗi bằng `console.log` duy nhất. UI cần báo lỗi ngắn gọn cho user.

## 9. Accessibility cơ bản

Tối thiểu phải có:

- Input có `label`.
- Button có text rõ.
- Không dùng màu là cách duy nhất để hiểu trạng thái.
- Focus state nhìn thấy được.
- Confirm trước khi xóa.
- Table có heading rõ.

## 10. Comment style cho người mới

Nên comment:

- Lần đầu dùng hook mới.
- Lần đầu dùng TypeScript type mới.
- Logic CRUD.
- Logic validation.
- Logic gọi API.
- Logic database query.

Không nên comment:

```ts
// Tăng i lên 1
i = i + 1;
```

Nên comment:

```ts
// Tạo object job mới từ dữ liệu form.
// id và thời gian được tạo ở đây vì dữ liệu trong phase này chưa có database.
const newJob: Job = {
  id: crypto.randomUUID(),
  company: formValues.company,
  position: formValues.position,
  status: formValues.status,
  note: formValues.note,
  createdAt: new Date().toISOString(),
  updatedAt: new Date().toISOString(),
};
```

## 11. Folder decision

Phase đầu nên giữ đơn giản:

```txt
apps/web/
  app/
  components/
  lib/
  types/
```

Chỉ tách `packages/ui` khi:

- Có ít nhất 5 component dùng lại.
- Component không phụ thuộc business logic.
- Việc tách giúp code dễ hiểu hơn.

## 12. Chất lượng trước khi commit

Trước khi commit, chạy:

```bash
pnpm lint
pnpm type-check
pnpm test
pnpm build
```

Nếu command chưa tồn tại ở phase hiện tại, ghi rõ trong README command nào đang dùng được.


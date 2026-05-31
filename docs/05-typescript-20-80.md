# 05. TypeScript 20/80

Mục tiêu: dùng TypeScript đủ để code React/Next an toàn hơn.

## Type là gì?

Type mô tả hình dạng dữ liệu.

```ts
// JobStatus chỉ cho phép 4 giá trị này.
export type JobStatus = 'Applied' | 'Interview' | 'Offer' | 'Rejected';

// Job mô tả object job đầy đủ trong app.
export type Job = {
  id: string;
  company: string;
  position: string;
  status: JobStatus;
  note: string;
};
```

## Function type

```ts
// company phải là string.
// Function trả về string.
function normalizeCompanyName(company: string): string {
  return company.trim();
}
```

## Array type

```ts
// jobs là mảng gồm nhiều Job.
const jobs: Job[] = [];
```

## Optional property

```ts
type JobInput = {
  company: string;
  position: string;
  note?: string;
};
```

`note?` nghĩa là có thể có hoặc không.

## Generic trong useState

```tsx
// <Job[]> nói với TypeScript rằng state này là mảng Job.
const [jobs, setJobs] = useState<Job[]>([]);
```

## Tránh any

Không dùng:

```ts
function saveJob(job: any) {}
```

Nên dùng:

```ts
function saveJob(job: JobInput) {}
```

## Quy tắc học

- Type dữ liệu chính trước: `Job`, `JobInput`, `JobStatus`.
- Type props cho component.
- Type parameter cho function.
- Chỉ dùng type nâng cao khi thật sự cần.

## TypeScript giải quyết vấn đề gì?

JavaScript rất linh hoạt. Linh hoạt giúp code nhanh, nhưng cũng dễ sai.

Ví dụ JavaScript không báo lỗi sớm:

```js
const job = {
  company: 'Google',
  position: 'Intern',
};

console.log(job.status.toLowerCase());
```

Code này có thể lỗi khi chạy vì `status` không tồn tại.

Với TypeScript:

```ts
type Job = {
  company: string;
  position: string;
  status: string;
};

const job: Job = {
  company: 'Google',
  position: 'Intern',
};
```

TypeScript sẽ báo lỗi ngay vì thiếu `status`.

## Type annotation

Type annotation là phần sau dấu `:`.

```ts
const company: string = 'Google';
const applicationCount: number = 3;
const isRemote: boolean = true;
```

Trong thực tế, không cần annotate mọi biến nếu TypeScript tự đoán được.

```ts
const company = 'Google';
```

TypeScript tự hiểu `company` là string.

Nên annotate khi:

- Function parameter.
- Function return type quan trọng.
- State React với mảng rỗng.
- Object data model.

## Union type

Union type giới hạn giá trị được phép.

```ts
type JobStatus = 'Applied' | 'Interview' | 'Offer' | 'Rejected';
```

Lợi ích:

```ts
const status: JobStatus = 'Applying';
```

TypeScript báo lỗi vì `Applying` không nằm trong danh sách hợp lệ.

Union type rất hữu ích cho status, role, tab, filter.

## `type` và `interface`

Cả hai đều mô tả object.

```ts
type Job = {
  id: string;
  company: string;
};
```

```ts
interface Job {
  id: string;
  company: string;
}
```

Trong project này, ưu tiên `type` cho đơn giản và nhất quán. Sau này khi gặp library dùng `interface`, chỉ cần hiểu ý nghĩa tương tự.

## Type cho props React

```tsx
type JobRowProps = {
  job: Job;
  onDelete: (jobId: string) => void;
};

function JobRow({job, onDelete}: JobRowProps) {
  return (
    <button
      onClick={function () {
        onDelete(job.id);
      }}
    >
      Delete
    </button>
  );
}
```

Giải thích:

- `job: Job`: component cần một object job.
- `onDelete: (jobId: string) => void`: component cần một function nhận id và không return gì quan trọng.

## Type cho form input

Nên tách `Job` và `JobInput`.

```ts
type Job = {
  id: string;
  company: string;
  position: string;
  status: JobStatus;
  createdAt: string;
  updatedAt: string;
};

type JobInput = {
  company: string;
  position: string;
  status: JobStatus;
};
```

Vì sao?

- Khi user nhập form, chưa có `id`.
- `id`, `createdAt`, `updatedAt` thường do app/database tạo.
- Tách type giúp code rõ hơn.

## Optional và nullable

Optional:

```ts
type JobInput = {
  note?: string;
};
```

Nghĩa là `note` có thể không tồn tại.

Nullable:

```ts
type JobInput = {
  note: string | null;
};
```

Nghĩa là `note` luôn có field, nhưng value có thể là `null`.

Với người mới, ưu tiên dùng một kiểu nhất quán. Trong project CRUD này, form có thể dùng string rỗng `''` cho note để đơn giản.

## Narrowing

Narrowing là kiểm tra trước để TypeScript hiểu dữ liệu chắc chắn hơn.

```ts
function printNote(note: string | undefined) {
  if (note === undefined) {
    return;
  }

  console.log(note.toUpperCase());
}
```

Giải thích:

- Ban đầu `note` có thể là `string` hoặc `undefined`.
- Sau `if`, TypeScript biết `note` chắc chắn là string.

## Type assertion nên hạn chế

Type assertion:

```ts
const status = value as JobStatus;
```

Câu này nói với TypeScript: "Tin tôi đi, value là JobStatus."

Vấn đề: nếu bạn sai, TypeScript không cứu được nữa.

Nên validate rõ:

```ts
function isJobStatus(value: string): value is JobStatus {
  return (
    value === 'Applied' ||
    value === 'Interview' ||
    value === 'Offer' ||
    value === 'Rejected'
  );
}
```

## Lỗi TypeScript thường gặp

1. Mảng rỗng bị hiểu sai type.

```tsx
const [jobs, setJobs] = useState([]);
```

Nên viết:

```tsx
const [jobs, setJobs] = useState<Job[]>([]);
```

2. Props thiếu field.

3. API trả dữ liệu không đúng type mình nghĩ.

4. Dùng `any` để né lỗi quá sớm.

## Bài tập nhỏ

1. Tạo `JobStatus`.
2. Tạo `Job`.
3. Tạo `JobInput`.
4. Viết function `createJob(input: JobInput): Job`.
5. Viết function `isJobStatus(value: string): value is JobStatus`.
6. Type props cho `JobRow`.

## Checklist tự kiểm tra

- [ ] Biết type giúp bắt lỗi trước khi chạy.
- [ ] Biết union type dùng cho status.
- [ ] Biết type props component.
- [ ] Biết type function parameter và return.
- [ ] Biết vì sao cần `JobInput`.
- [ ] Biết hạn chế dùng `any`.
- [ ] Biết type assertion không phải validation thật.

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


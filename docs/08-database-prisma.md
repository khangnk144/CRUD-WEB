# 08. Database and Prisma

Mục tiêu: hiểu database và ORM đủ để lưu dữ liệu CRUD.

## Database là gì?

Database lưu dữ liệu lâu dài. Nếu chỉ dùng state hoặc array trong server, tắt app là mất dữ liệu.

## Prisma là gì?

Prisma là ORM. ORM giúp code TypeScript nói chuyện với database bằng object/function thay vì viết SQL trực tiếp ở mọi nơi.

## Prisma schema

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

Giải thích:

- `model Job`: tạo bảng Job.
- `String`: kiểu chuỗi.
- `String?`: có thể null.
- `@id`: khóa chính.
- `@default(cuid())`: tự tạo id.
- `@default(now())`: tự tạo thời gian hiện tại.
- `@updatedAt`: tự cập nhật thời gian khi record thay đổi.

## Query 20/80

```ts
// Lấy tất cả job.
const jobs = await prisma.job.findMany();

// Tạo job mới.
const createdJob = await prisma.job.create({
  data: {
    company: 'Google',
    position: 'Intern',
    status: 'Applied',
  },
});

// Sửa job.
const updatedJob = await prisma.job.update({
  where: {
    id: jobId,
  },
  data: {
    status: 'Interview',
  },
});

// Xóa job.
await prisma.job.delete({
  where: {
    id: jobId,
  },
});
```

## SQLite trước, PostgreSQL sau

Nên học SQLite trước vì:

- Không cần cài database server riêng.
- Dễ chạy local.
- Đủ cho phase học CRUD.

Sau khi hiểu Prisma, chuyển PostgreSQL để giống production hơn.

## Vì sao cần database?

Trong phase đầu, dữ liệu có thể nằm ở:

- Array trong JavaScript.
- `localStorage`.
- Array tạm trong API route.

Những cách đó tốt để học, nhưng không đủ cho app thực tế.

Vấn đề:

- Array trong browser mất khi refresh nếu không lưu.
- `localStorage` chỉ nằm trên máy user.
- Array trong server mất khi server restart.
- Không nhiều user dùng chung dữ liệu được.

Database giải quyết việc lưu dữ liệu lâu dài và truy vấn có cấu trúc.

## Table, row, column

Nếu dùng database quan hệ như SQLite/PostgreSQL:

- Table giống một bảng.
- Row là một dòng dữ liệu.
- Column là một cột.

Ví dụ table `Job`:

| id | company | position | status |
| --- | --- | --- | --- |
| job_1 | Google | Intern | Applied |
| job_2 | Shopify | Frontend | Interview |

## Primary key

Primary key là định danh duy nhất cho mỗi row.

Trong model Job:

```prisma
id String @id @default(cuid())
```

Giải thích:

- `id`: tên field.
- `String`: kiểu dữ liệu.
- `@id`: đây là primary key.
- `@default(cuid())`: Prisma tự tạo id nếu bạn không truyền.

Không nên dùng company hoặc position làm primary key vì nhiều job có thể cùng company/position.

## Migration là gì?

Migration là lịch sử thay đổi cấu trúc database.

Ví dụ:

1. Ban đầu tạo bảng Job.
2. Sau đó thêm field `note`.
3. Sau đó thêm bảng User.

Mỗi lần thay đổi schema, migration giúp database cập nhật có kiểm soát.

Command thường gặp:

```bash
pnpm prisma migrate dev
```

Ý nghĩa:

- Đọc `schema.prisma`.
- Tạo migration mới nếu schema thay đổi.
- Áp dụng migration vào database local.
- Cập nhật Prisma Client.

## Prisma Client là gì?

Prisma Client là code được Prisma generate để TypeScript gọi database.

Ví dụ:

```ts
const jobs = await prisma.job.findMany();
```

Bạn không tự viết SQL cho query đơn giản. Prisma tạo method dựa trên model trong schema.

## Tạo Prisma helper

Trong Next.js, thường tạo helper để tránh tạo quá nhiều Prisma Client khi dev server hot reload.

Ví dụ sau này có thể nằm ở `lib/prisma.ts`:

```ts
import {PrismaClient} from '@prisma/client';

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined;
};

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: ['query', 'error', 'warn'],
  });

if (process.env.NODE_ENV !== 'production') {
  globalForPrisma.prisma = prisma;
}
```

Đoạn này hơi nâng cao. Khi mới học, chỉ cần hiểu:

- `PrismaClient` là object dùng để query database.
- `prisma.job.findMany()` nghĩa là query bảng Job.
- Phần `globalForPrisma` giúp tránh tạo nhiều connection khi dev.

## CRUD với Prisma chi tiết

Create:

```ts
const createdJob = await prisma.job.create({
  data: {
    company: jobInput.company,
    position: jobInput.position,
    status: jobInput.status,
    note: jobInput.note,
  },
});
```

Read:

```ts
const jobs = await prisma.job.findMany({
  orderBy: {
    createdAt: 'desc',
  },
});
```

Update:

```ts
const updatedJob = await prisma.job.update({
  where: {
    id: jobId,
  },
  data: {
    company: jobInput.company,
    position: jobInput.position,
    status: jobInput.status,
    note: jobInput.note,
  },
});
```

Delete:

```ts
await prisma.job.delete({
  where: {
    id: jobId,
  },
});
```

Giải thích:

- `data`: dữ liệu muốn ghi.
- `where`: điều kiện tìm record.
- `orderBy`: sắp xếp kết quả.

## Prisma error cần biết

Nếu update/delete một id không tồn tại, Prisma có thể throw error.

API nên xử lý:

```ts
try {
  const deletedJob = await prisma.job.delete({
    where: {
      id: jobId,
    },
  });

  return Response.json({
    data: deletedJob,
  });
} catch (error) {
  return Response.json(
    {
      error: {
        message: 'Job not found',
      },
    },
    {
      status: 404,
    },
  );
}
```

Phase đầu có thể xử lý đơn giản như trên. Sau này học kỹ hơn sẽ phân loại error cụ thể.

## SQLite và PostgreSQL

SQLite:

- Dữ liệu nằm trong một file.
- Dễ setup.
- Phù hợp học local.

PostgreSQL:

- Database server thật.
- Phổ biến trong production.
- Phù hợp CV/deploy nghiêm túc hơn.

Roadmap:

1. Học Prisma với SQLite.
2. Làm CRUD ổn.
3. Đổi sang PostgreSQL.
4. Deploy.

## Seed data

Seed nghĩa là tạo dữ liệu mẫu.

Ví dụ jobs mẫu:

- Google - Frontend Intern - Applied.
- Shopify - Junior Developer - Interview.
- Atlassian - React Developer - Rejected.

Seed giúp:

- Dev app không cần nhập lại dữ liệu mỗi lần.
- Demo CV có dữ liệu đẹp hơn.
- Test UI empty/non-empty dễ hơn.

## Bài tập nhỏ

1. Vẽ bảng Job ra giấy.
2. Viết model Prisma Job.
3. Chạy migration.
4. Mở Prisma Studio.
5. Tạo một job bằng Prisma Studio.
6. Query job bằng API.
7. Tạo job từ form frontend.

## Checklist tự kiểm tra

- [ ] Biết database lưu dữ liệu lâu dài.
- [ ] Biết table/row/column là gì.
- [ ] Biết primary key dùng để định danh row.
- [ ] Biết Prisma schema mô tả database.
- [ ] Biết migration dùng để cập nhật database.
- [ ] Biết query CRUD bằng Prisma Client.
- [ ] Biết SQLite dễ học trước, PostgreSQL học sau.

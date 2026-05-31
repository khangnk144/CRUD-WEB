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


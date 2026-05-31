# 02. Git and GitHub

Mục tiêu: biết dùng Git đủ để quản lý project CV.

## Git là gì?

Git lưu lịch sử thay đổi của code. Khi code hỏng, có thể xem lại đã sửa gì.

## GitHub là gì?

GitHub là nơi lưu repo online. Nhà tuyển dụng có thể xem code của bạn trên GitHub.

## Cú pháp 20/80

```bash
git status
git add .
git commit -m "Add vanilla CRUD plan"
git log --oneline
git branch
git switch -c feature/job-form
git push
```

Giải thích:

- `git status`: xem file nào đã thay đổi.
- `git add .`: đưa file vào vùng chuẩn bị commit.
- `git commit -m`: lưu một mốc thay đổi.
- `git log --oneline`: xem lịch sử ngắn gọn.
- `git branch`: xem nhánh.
- `git switch -c`: tạo nhánh mới và chuyển sang nhánh đó.
- `git push`: đẩy code lên GitHub.

## Commit tốt là gì?

Commit nên nhỏ và rõ.

Ví dụ tốt:

```txt
Add job form component
Implement localStorage persistence
Add Prisma job model
Fix job status filter
```

Ví dụ chưa tốt:

```txt
update
fix
abc
final
```

## Quy tắc branch

Với project học tập:

- `main`: code ổn định.
- `feature/vanilla-crud`: làm CRUD thuần.
- `feature/react-crud`: làm React CRUD.
- `feature/prisma-database`: thêm database.

Không cần workflow phức tạp.


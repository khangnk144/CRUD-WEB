# 07. API CRUD

Mục tiêu: hiểu frontend và backend nói chuyện với nhau như thế nào.

## API là gì?

API là cổng để frontend gửi request và nhận response từ backend.

Ví dụ frontend gửi:

```txt
GET /api/jobs
```

Backend trả:

```json
{
  "data": []
}
```

## HTTP method

- `GET`: lấy dữ liệu.
- `POST`: tạo dữ liệu.
- `PATCH`: sửa một phần dữ liệu.
- `PUT`: thay toàn bộ dữ liệu.
- `DELETE`: xóa dữ liệu.

## Status code cơ bản

- `200`: thành công.
- `201`: tạo mới thành công.
- `400`: request sai.
- `401`: chưa đăng nhập.
- `403`: không có quyền.
- `404`: không tìm thấy.
- `500`: lỗi server.

## Response format

Thành công:

```json
{
  "data": {
    "id": "job_1",
    "company": "Google"
  }
}
```

Thất bại:

```json
{
  "error": {
    "message": "Company is required"
  }
}
```

## Quy tắc CRUD API

- Validate input trước khi lưu.
- Không tin dữ liệu từ frontend.
- Trả status code đúng.
- Error message ngắn và dễ hiểu.
- API không nên trả stack trace cho user.


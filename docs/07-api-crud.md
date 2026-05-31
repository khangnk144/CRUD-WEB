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

## Frontend và backend nói chuyện như thế nào?

Khi user bấm nút "Add job", frontend không tự ghi database. Frontend gửi request đến backend.

Luồng:

```txt
User submit form
  -> frontend tạo request POST /api/jobs
  -> backend nhận body
  -> backend validate body
  -> backend lưu database
  -> backend trả response
  -> frontend cập nhật UI
```

API là hợp đồng giữa frontend và backend. Nếu hợp đồng rõ, code dễ sửa hơn.

## Request gồm những gì?

Một request thường có:

- Method: `GET`, `POST`, `PATCH`, `DELETE`.
- URL: `/api/jobs`.
- Headers: metadata, ví dụ `Content-Type`.
- Body: dữ liệu gửi lên, thường dùng với POST/PATCH.

Ví dụ frontend gửi POST:

```ts
const response = await fetch('/api/jobs', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    company: 'Google',
    position: 'Intern',
    status: 'Applied',
  }),
});
```

Giải thích:

- `method: 'POST'`: tạo dữ liệu mới.
- `Content-Type`: nói với server body là JSON.
- `JSON.stringify`: đổi object JavaScript thành chuỗi JSON.

## Response gồm những gì?

Một response thường có:

- Status code.
- Headers.
- Body JSON.

Ví dụ:

```json
{
  "data": {
    "id": "job_1",
    "company": "Google",
    "position": "Intern",
    "status": "Applied"
  }
}
```

Frontend đọc:

```ts
const responseBody = await response.json();
```

## Status code nên dùng trong CRUD-WEB

GET thành công:

```txt
200 OK
```

POST tạo thành công:

```txt
201 Created
```

Input sai:

```txt
400 Bad Request
```

Không tìm thấy job:

```txt
404 Not Found
```

Lỗi bất ngờ:

```txt
500 Internal Server Error
```

Không cần học hết mọi status code ngay. Nắm nhóm trên là đủ cho CRUD cơ bản.

## Validate ở frontend chưa đủ

Frontend validation giúp user thấy lỗi nhanh. Nhưng backend vẫn phải validate.

Lý do:

- User có thể gọi API bằng Postman/curl.
- Browser code có thể bị sửa.
- Bug frontend có thể gửi dữ liệu sai.

Quy tắc: dữ liệu vào database phải đi qua backend validation.

## Thiết kế API cho Job

Create job request:

```json
{
  "company": "Google",
  "position": "Frontend Intern",
  "status": "Applied",
  "note": "Applied through careers page"
}
```

Create job response:

```json
{
  "data": {
    "id": "clx123",
    "company": "Google",
    "position": "Frontend Intern",
    "status": "Applied",
    "note": "Applied through careers page",
    "createdAt": "2026-05-31T10:00:00.000Z",
    "updatedAt": "2026-05-31T10:00:00.000Z"
  }
}
```

Error response:

```json
{
  "error": {
    "message": "Company is required"
  }
}
```

## `PUT` và `PATCH` khác nhau thế nào?

`PUT` thường nghĩa là thay toàn bộ resource.

`PATCH` thường nghĩa là sửa một phần resource.

Với CRUD-WEB, dùng `PATCH` cho edit job vì user có thể chỉ đổi status hoặc note.

Ví dụ:

```json
{
  "status": "Interview"
}
```

## Idempotent là gì?

Khái niệm này chưa cần quá sâu, nhưng nên biết:

- Gọi `GET /api/jobs` nhiều lần không làm thay đổi dữ liệu.
- Gọi `DELETE /api/jobs/1` nhiều lần về ý nghĩa vẫn là job đó bị xóa.
- Gọi `POST /api/jobs` nhiều lần thường tạo nhiều job mới.

Điều này giúp hiểu vì sao method HTTP có ý nghĩa riêng.

## Test API bằng Postman hoặc browser

GET có thể test bằng browser:

```txt
http://localhost:3000/api/jobs
```

POST/PATCH/DELETE nên test bằng:

- Postman.
- VS Code REST Client.
- curl.
- Frontend form.

## Lỗi API thường gặp

1. Quên `Content-Type: application/json`.

2. Quên `JSON.stringify`.

3. Backend quên `await request.json()`.

4. Response format lúc thì `{data}`, lúc thì trả thẳng object, làm frontend rối.

5. Không xử lý trường hợp không tìm thấy job.

6. API trả `200` dù request sai.

## Bài tập nhỏ

1. Viết bảng route CRUD cho Job.
2. Viết request body cho create job.
3. Viết response success.
4. Viết response error.
5. Test GET bằng browser.
6. Test POST bằng Postman.

## Checklist tự kiểm tra

- [ ] Biết API là hợp đồng frontend/backend.
- [ ] Biết method HTTP cơ bản.
- [ ] Biết request body cần `JSON.stringify`.
- [ ] Biết backend phải validate lại.
- [ ] Biết response nên có format thống nhất.
- [ ] Biết dùng status code cơ bản.

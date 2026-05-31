# 09. Testing and Debugging

Mục tiêu: biết kiểm tra app không chỉ bằng mắt.

## Debugging cơ bản

Công cụ:

- Browser DevTools.
- `console.log`.
- Network tab.
- React DevTools.
- Terminal error.

Quy tắc:

- Đọc error message từ trên xuống.
- Xác định lỗi ở frontend, API hay database.
- Kiểm tra request/response trong Network tab.
- Log dữ liệu tại điểm nghi ngờ, không log tràn lan.

## Test cần có

Unit test:

- Test function filter jobs.
- Test function validate job input.

Component test:

- Render form.
- Nhập input.
- Submit form.

E2E test:

- Mở trang jobs.
- Thêm job.
- Sửa job.
- Xóa job.

## Command mục tiêu

```bash
pnpm test
pnpm test:e2e
```

## Vì sao test quan trọng cho CV?

Test cho thấy bạn không chỉ code cho chạy, mà còn biết giữ chất lượng khi project lớn dần.


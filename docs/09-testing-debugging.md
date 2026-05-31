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

## Debug theo lớp

Khi app lỗi, đừng sửa ngẫu nhiên. Hãy xác định lỗi thuộc lớp nào.

Các lớp thường gặp:

1. **UI layer**
   - Button không click được.
   - Form không nhập được.
   - CSS vỡ layout.

2. **State layer**
   - Dữ liệu không cập nhật.
   - Xóa rồi nhưng item vẫn hiện.
   - Search/filter sai.

3. **API layer**
   - Request sai method.
   - Body gửi lên sai format.
   - API trả status code lỗi.

4. **Database layer**
   - Query sai.
   - Migration chưa chạy.
   - `DATABASE_URL` sai.

5. **Environment layer**
   - Chưa cài package.
   - Sai Node version.
   - Biến môi trường thiếu.

Khi có lỗi, hỏi:

- Lỗi xuất hiện trên màn hình hay terminal?
- Network request có chạy không?
- API có trả response không?
- Database có record không?

## Cách đọc error message

Người mới hay nhìn error và hoảng. Hãy đọc theo thứ tự:

1. Dòng đầu: lỗi gì?
2. File nào?
3. Line nào?
4. Stack trace có nhắc đến code của mình không?
5. Lỗi xảy ra sau hành động nào?

Ví dụ:

```txt
TypeError: Cannot read properties of undefined (reading 'status')
```

Nghĩa là bạn đang gọi `.status` trên một biến `undefined`.

Cần kiểm tra:

- Object đó đến từ đâu?
- Có thể không tìm thấy job không?
- Có cần `if (!job) return` không?

## Network tab

Khi frontend gọi API, mở DevTools -> Network.

Kiểm tra:

- Request URL có đúng không?
- Method có đúng không?
- Status code là gì?
- Request payload gửi gì?
- Response trả gì?

Ví dụ lỗi:

Frontend gọi:

```txt
POST /api/job
```

Nhưng API thật là:

```txt
POST /api/jobs
```

Network tab sẽ giúp thấy sai URL.

## Console log đúng cách

Không log lung tung quá nhiều. Log tại điểm cần kiểm tra.

Ví dụ:

```ts
console.log('Job input before submit:', jobInput);
```

Tên log nên rõ:

- Log dữ liệu gì?
- Log ở bước nào?

Sau khi sửa xong, xóa log không cần thiết.

## Testing pyramid đơn giản

Không cần học test quá phức tạp ngay. Chia thành 3 loại:

1. **Unit test**
   - Test function nhỏ.
   - Ví dụ: `filterJobs`, `validateJobInput`.

2. **Component test**
   - Test component React.
   - Ví dụ: render form, nhập input, submit.

3. **E2E test**
   - Test như user thật.
   - Ví dụ: mở browser, thêm job, sửa job, xóa job.

## Unit test nên bắt đầu từ đâu?

Hãy tách logic thuần ra function.

Ví dụ:

```ts
export function filterJobs(jobs: Job[], keyword: string): Job[] {
  const normalizedKeyword = keyword.trim().toLowerCase();

  return jobs.filter(function (job) {
    return (
      job.company.toLowerCase().includes(normalizedKeyword) ||
      job.position.toLowerCase().includes(normalizedKeyword)
    );
  });
}
```

Function này dễ test vì:

- Input rõ.
- Output rõ.
- Không phụ thuộc DOM.
- Không gọi API.

## Test case là gì?

Test case là một tình huống cụ thể.

Ví dụ cho `filterJobs`:

- Keyword rỗng -> trả tất cả jobs.
- Keyword khớp company -> trả job đúng.
- Keyword khớp position -> trả job đúng.
- Keyword không khớp -> trả mảng rỗng.
- Keyword có chữ hoa/thường khác nhau -> vẫn khớp.

## Manual test checklist

Trước khi có automated test đầy đủ, luôn có checklist test tay.

Ví dụ phase vanilla:

- [ ] Mở app không lỗi console.
- [ ] Thêm job hợp lệ.
- [ ] Không thêm được job thiếu company.
- [ ] Không thêm được job thiếu position.
- [ ] Xóa job.
- [ ] Sửa job.
- [ ] Search theo company.
- [ ] Filter theo status.
- [ ] Reload page vẫn còn dữ liệu.

## Khi nào viết test?

Với project học tập:

- Phase 1: test tay là đủ, có thể thêm unit test nếu muốn.
- Phase 2-3: bắt đầu test helper function.
- Phase 5-6: test API và flow CRUD quan trọng.
- Phase 9: bổ sung E2E để project CV chắc hơn.

## Bài tập nhỏ

1. Viết checklist test tay cho CRUD thuần.
2. Tạo function `validateJobInput`.
3. Liệt kê 5 test case cho function đó.
4. Khi học Vitest, viết test cho các case trên.

## Checklist tự kiểm tra

- [ ] Biết phân lớp lỗi UI/state/API/database/environment.
- [ ] Biết dùng Network tab kiểm tra API.
- [ ] Biết log có mục đích.
- [ ] Biết unit test khác E2E test.
- [ ] Biết viết checklist test tay.
- [ ] Biết ưu tiên test logic quan trọng trước.

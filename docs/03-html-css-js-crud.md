# 03. HTML CSS JavaScript CRUD

Mục tiêu: hiểu CRUD bằng JavaScript thuần trước khi học React.

## CRUD là gì?

CRUD gồm:

- Create: tạo dữ liệu mới.
- Read: đọc và hiển thị dữ liệu.
- Update: sửa dữ liệu.
- Delete: xóa dữ liệu.

## Dữ liệu ban đầu

```js
// Mảng jobs là nơi lưu dữ liệu tạm trong bộ nhớ.
// Mỗi object trong mảng là một job.
let jobs = [
  {
    id: 1,
    company: 'Google',
    position: 'Intern',
    status: 'Applied',
  },
];
```

## Render list

```js
// Hàm renderJobs chịu trách nhiệm biến dữ liệu jobs thành HTML.
// Mỗi khi jobs thay đổi, gọi lại hàm này để giao diện cập nhật.
function renderJobs() {
  const jobListElement = document.querySelector('#job-list');

  jobListElement.innerHTML = jobs
    .map(function (job) {
      return `
        <li>
          ${job.company} - ${job.position} - ${job.status}
        </li>
      `;
    })
    .join('');
}
```

## Thêm dữ liệu

```js
// preventDefault ngăn browser reload page khi submit form.
formElement.addEventListener('submit', function (event) {
  event.preventDefault();

  const newJob = {
    id: Date.now(),
    company: companyInput.value,
    position: positionInput.value,
    status: statusInput.value,
  };

  jobs.push(newJob);
  renderJobs();
});
```

## Xóa dữ liệu

```js
// filter tạo mảng mới chỉ gồm các job không bị xóa.
function deleteJob(jobId) {
  jobs = jobs.filter(function (job) {
    return job.id !== jobId;
  });

  renderJobs();
}
```

## LocalStorage

```js
// JSON.stringify đổi array/object thành string để lưu vào localStorage.
localStorage.setItem('jobs', JSON.stringify(jobs));

// JSON.parse đổi string từ localStorage về array/object.
const savedJobs = JSON.parse(localStorage.getItem('jobs'));
```

## Cần nắm chắc

- DOM là cây HTML mà JavaScript có thể đọc/sửa.
- Dữ liệu thay đổi trước, UI render lại sau.
- Không trộn quá nhiều HTML string phức tạp nếu có thể tách function.
- Validate input trước khi thêm vào array.

## Tư duy quan trọng: data là nguồn sự thật

Trong CRUD app, đừng nghĩ UI là nơi lưu dữ liệu. UI chỉ là thứ hiển thị dữ liệu.

Luồng đúng:

```txt
User thao tác
  -> JavaScript cập nhật data
  -> JavaScript render lại UI từ data mới
```

Ví dụ khi xóa job:

1. User bấm Delete.
2. Code xóa job khỏi mảng `jobs`.
3. Code gọi `renderJobs()`.
4. UI hiển thị danh sách mới.

Không nên xóa mỗi dòng HTML khỏi màn hình mà không cập nhật mảng `jobs`, vì khi render lại, dữ liệu cũ có thể quay lại.

## HTML structure đề xuất

File `index.html` nên có cấu trúc rõ:

```html
<!doctype html>
<html lang="vi">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Job Tracker</title>
    <link rel="stylesheet" href="./style.css" />
  </head>
  <body>
    <main class="app">
      <h1>Job Application Tracker</h1>

      <form id="job-form">
        <label>
          Company
          <input id="company-input" type="text" />
        </label>

        <label>
          Position
          <input id="position-input" type="text" />
        </label>

        <label>
          Status
          <select id="status-input">
            <option value="Applied">Applied</option>
            <option value="Interview">Interview</option>
            <option value="Offer">Offer</option>
            <option value="Rejected">Rejected</option>
          </select>
        </label>

        <button type="submit">Add job</button>
      </form>

      <ul id="job-list"></ul>
    </main>

    <script src="./script.js"></script>
  </body>
</html>
```

Điểm cần chú ý:

- `id` dùng để JavaScript tìm element.
- `label` giúp form dễ dùng hơn và tốt cho accessibility.
- `type="submit"` làm button submit form.
- Script đặt cuối `body` để HTML load trước rồi JavaScript mới chạy.

## Lấy element từ DOM

```js
const formElement = document.querySelector('#job-form');
const companyInput = document.querySelector('#company-input');
const positionInput = document.querySelector('#position-input');
const statusInput = document.querySelector('#status-input');
const jobListElement = document.querySelector('#job-list');
```

Giải thích:

- `document` đại diện cho trang HTML hiện tại.
- `querySelector` tìm element đầu tiên khớp selector.
- `#job-form` nghĩa là tìm element có `id="job-form"`.

Lỗi thường gặp:

- Gõ sai id trong HTML hoặc JS.
- Script chạy trước khi HTML được tạo.
- Quên dấu `#` khi tìm theo id.

## Tách function để dễ hiểu

Thay vì viết tất cả trong một file dài không tổ chức, nên tách function:

```js
function createJobFromForm() {
  return {
    id: Date.now(),
    company: companyInput.value.trim(),
    position: positionInput.value.trim(),
    status: statusInput.value,
  };
}

function addJob(job) {
  jobs.push(job);
}

function renderJobs() {
  // Render UI ở đây.
}
```

Lợi ích:

- Mỗi function làm một việc.
- Khi lỗi, dễ biết lỗi nằm ở đâu.
- Sau này chuyển sang React sẽ dễ hơn vì đã quen tách logic.

## Validate input cơ bản

Không nên cho phép thêm job thiếu company hoặc position.

```js
function validateJob(job) {
  if (job.company === '') {
    return 'Company is required';
  }

  if (job.position === '') {
    return 'Position is required';
  }

  return '';
}
```

Cách dùng:

```js
const newJob = createJobFromForm();
const errorMessage = validateJob(newJob);

if (errorMessage !== '') {
  alert(errorMessage);
  return;
}

addJob(newJob);
renderJobs();
```

Giải thích:

- Nếu có lỗi, function trả về message.
- Nếu không lỗi, trả về chuỗi rỗng.
- `return` trong event handler giúp dừng xử lý sớm.

## Edit job nên làm đơn giản trước

Cách đơn giản cho người mới:

1. Bấm Edit.
2. Đưa dữ liệu job lên form.
3. Lưu `editingJobId`.
4. Submit form thì nếu có `editingJobId`, sửa job thay vì tạo job mới.

Ví dụ:

```js
let editingJobId = null;

function startEditJob(jobId) {
  const jobToEdit = jobs.find(function (job) {
    return job.id === jobId;
  });

  if (!jobToEdit) {
    return;
  }

  editingJobId = jobToEdit.id;
  companyInput.value = jobToEdit.company;
  positionInput.value = jobToEdit.position;
  statusInput.value = jobToEdit.status;
}
```

Giải thích:

- `editingJobId` là biến nhớ app đang sửa job nào.
- `find` tìm một job theo id.
- Nếu không tìm thấy job, dừng function.

## Search và filter

Search và filter không nên thay đổi mảng gốc `jobs`. Chúng chỉ tạo danh sách hiển thị.

```js
function getVisibleJobs() {
  const keyword = searchInput.value.trim().toLowerCase();
  const status = statusFilter.value;

  return jobs.filter(function (job) {
    const companyMatches = job.company.toLowerCase().includes(keyword);
    const positionMatches = job.position.toLowerCase().includes(keyword);
    const keywordMatches = companyMatches || positionMatches;

    const statusMatches = status === 'All' || job.status === status;

    return keywordMatches && statusMatches;
  });
}
```

Giải thích:

- `toLowerCase` giúp search không phân biệt chữ hoa/thường.
- `includes` kiểm tra chuỗi có chứa keyword không.
- `status === 'All'` nghĩa là không lọc theo status.

## Lưu localStorage đúng cách

Nên tạo function riêng:

```js
function saveJobsToLocalStorage() {
  localStorage.setItem('jobs', JSON.stringify(jobs));
}

function loadJobsFromLocalStorage() {
  const savedJobsText = localStorage.getItem('jobs');

  if (savedJobsText === null) {
    return [];
  }

  return JSON.parse(savedJobsText);
}
```

Khi thêm/sửa/xóa:

```js
saveJobsToLocalStorage();
renderJobs();
```

Lỗi thường gặp:

- Quên `JSON.stringify` khi lưu object/array.
- Quên `JSON.parse` khi đọc.
- Dữ liệu localStorage bị cũ hoặc sai format.

Nếu localStorage lỗi trong lúc học, có thể mở DevTools -> Application -> Local Storage -> xóa key `jobs`.

## Bài tập theo thứ tự

1. Tạo HTML form và danh sách rỗng.
2. Render một mảng jobs có sẵn.
3. Thêm job bằng form.
4. Validate company/position.
5. Xóa job.
6. Sửa job.
7. Search theo company/position.
8. Filter theo status.
9. Lưu localStorage.
10. Refactor function nếu file quá dài.

## Checklist tự kiểm tra

- [ ] Hiểu data nằm trong mảng `jobs`.
- [ ] Hiểu render là biến data thành HTML.
- [ ] Biết dùng `querySelector`.
- [ ] Biết dùng `addEventListener`.
- [ ] Biết `preventDefault` dùng để làm gì.
- [ ] Biết thêm/sửa/xóa bằng array method.
- [ ] Biết localStorage chỉ lưu string.
- [ ] Biết search/filter không nên phá mảng gốc.

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


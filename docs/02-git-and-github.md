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

## Git theo tư duy người mới

Git không phải chỉ để "upload code". Git giúp bạn lưu lại các mốc thay đổi.

Hãy tưởng tượng bạn đang chơi game và có nhiều save point:

- Code đang chạy tốt: commit một lần.
- Thêm tính năng mới: commit một lần.
- Sửa bug: commit một lần.

Khi code hỏng, bạn có thể xem lại mình đã thay đổi gì.

## Ba vùng quan trọng trong Git

Git có 3 vùng cần hiểu:

1. **Working tree**
   - File thật trong thư mục project.
   - Khi bạn sửa code trong VS Code, file nằm ở vùng này.

2. **Staging area**
   - Vùng chuẩn bị commit.
   - `git add` đưa file vào vùng này.

3. **Repository history**
   - Lịch sử commit.
   - `git commit` lưu snapshot vào vùng này.

Luồng cơ bản:

```bash
git status
git add .
git commit -m "Add job list rendering"
```

## Đọc `git status`

`git status` là command nên chạy rất thường xuyên.

Một số trạng thái thường gặp:

```txt
Untracked files
```

Nghĩa là file mới tạo, Git chưa theo dõi.

```txt
Changes not staged for commit
```

Nghĩa là file đã sửa nhưng chưa `git add`.

```txt
Changes to be committed
```

Nghĩa là file đã được `git add`, sẵn sàng commit.

```txt
nothing to commit, working tree clean
```

Nghĩa là không còn thay đổi chưa commit.

## `.gitignore`

`.gitignore` nói với Git bỏ qua file/thư mục không nên commit.

Project này nên ignore:

```gitignore
node_modules/
.next/
dist/
build/
.env.local
.env
polaris-react/
```

Giải thích:

- `node_modules/`: cài lại được.
- `.next/`, `dist/`, `build/`: output build, không phải source code.
- `.env.local`, `.env`: có thể chứa secret.
- `polaris-react/`: repo tham khảo local, không phải source của CRUD-WEB.

## Nếu lỡ commit file không nên commit

Ví dụ lỡ commit `polaris-react/`, dùng:

```bash
git rm --cached -r polaris-react
```

Giải thích:

- `git rm`: bảo Git ngừng theo dõi file.
- `--cached`: chỉ gỡ khỏi Git, không xóa file trên máy.
- `-r`: áp dụng cho cả thư mục.

Sau đó thêm vào `.gitignore`, rồi commit:

```bash
git add .gitignore
git commit -m "Ignore local Polaris reference"
```

## Remote GitHub

Sau khi tạo repo rỗng trên GitHub:

```bash
git remote add origin https://github.com/USERNAME/CRUD-WEB.git
git push -u origin main
```

Giải thích:

- `remote`: địa chỉ repo online.
- `origin`: tên mặc định thường dùng cho remote chính.
- `push`: đẩy commit từ máy lên GitHub.
- `-u origin main`: lần đầu liên kết branch local `main` với branch remote `main`.

Sau lần đầu, các lần sau chỉ cần:

```bash
git push
```

## Commit message nên viết thế nào?

Commit message nên trả lời: "Commit này làm gì?"

Ví dụ tốt:

```txt
Add vanilla job tracker HTML structure
Render jobs from JavaScript array
Persist jobs to localStorage
Add job status filter
```

Không nên:

```txt
update
fix bug
final final
asdf
```

## Quy trình làm mỗi task

Nên dùng quy trình này:

1. Chạy `git status`.
2. Tạo/sửa code.
3. Chạy app/test.
4. Chạy `git diff` để xem đã sửa gì.
5. Chạy `git add`.
6. Chạy `git commit`.
7. Chạy `git status` để đảm bảo sạch.

## Bài tập nhỏ

Tạo một file thử nghiệm:

```bash
echo "# Test" > test.md
git status
git add test.md
git status
git commit -m "Add test markdown file"
git status
```

Sau khi hiểu flow, có thể xóa file bằng commit riêng.

## Checklist tự kiểm tra

- [ ] Hiểu `git status` nói gì.
- [ ] Biết khác nhau giữa `git add` và `git commit`.
- [ ] Biết vì sao cần `.gitignore`.
- [ ] Biết không commit `node_modules` và secret.
- [ ] Biết push repo lên GitHub.
- [ ] Biết commit message nên rõ ràng.

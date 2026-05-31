# 01. Environment

Mục tiêu: hiểu môi trường chạy project web hiện đại.

## Node.js

Node.js cho phép chạy JavaScript ngoài browser. React, Next.js, Tailwind, Prisma đều cần Node để chạy tooling.

Cần biết:

- `node -v`: xem version Node.
- `npm -v`: xem version npm.
- `pnpm -v`: xem version pnpm nếu dùng pnpm.

Project này nên dùng Node LTS mới, ví dụ Node 20+.

## Package manager

Package manager dùng để cài thư viện.

Các lựa chọn phổ biến:

- `npm`: có sẵn khi cài Node.
- `pnpm`: nhanh, tiết kiệm ổ cứng, phù hợp dự án hiện đại.
- `yarn`: cũng phổ biến nhưng project này không ưu tiên.

Project này ưu tiên `pnpm`.

## Vì sao không dùng Python venv cho app chính?

`venv` dùng để cô lập package Python. App này là JavaScript/TypeScript project nên không dùng Python package để chạy app chính.

Cách cô lập đúng cho Node project:

- Mỗi project có `node_modules` riêng.
- Có `package.json` ghi dependencies.
- Có lockfile để khóa version.
- Có `.nvmrc` hoặc Volta để khóa Node version.
- Có `.env.local` cho biến môi trường máy cá nhân.

Nếu sau này có Python script, mới tạo `.venv`.

## Biến môi trường

Không commit file chứa secret thật.

Nên có:

```txt
.env.example
.env.local
```

Trong đó:

- `.env.example`: commit lên GitHub, chỉ chứa tên biến và value giả.
- `.env.local`: dùng trên máy cá nhân, không commit.

Ví dụ:

```env
DATABASE_URL="file:./dev.db"
```

## Command cơ bản

```bash
pnpm install
pnpm dev
pnpm lint
pnpm type-check
pnpm test
pnpm build
```

Ý nghĩa:

- `install`: cài thư viện.
- `dev`: chạy server khi code.
- `lint`: kiểm tra lỗi style/code smell.
- `type-check`: kiểm tra lỗi TypeScript.
- `test`: chạy test.
- `build`: build app như production.

## Nên hiểu project web hiện đại như một hệ thống nhỏ

Khi mới học web, bạn thường chỉ mở file `index.html` trong browser là chạy được. Nhưng khi dùng React, Next.js, TypeScript hoặc Tailwind CSS, project cần thêm một lớp tooling.

Hãy hình dung project có 4 phần:

1. **Source code**
   - Đây là code bạn viết.
   - Ví dụ: `.html`, `.css`, `.js`, `.ts`, `.tsx`.

2. **Dependencies**
   - Đây là thư viện người khác viết, project của bạn dùng lại.
   - Ví dụ: `react`, `next`, `typescript`, `tailwindcss`.
   - Các thư viện này được ghi trong `package.json`.

3. **Tooling**
   - Đây là công cụ giúp code chạy hoặc kiểm tra code.
   - Ví dụ: Vite, Next.js dev server, ESLint, Prettier, TypeScript compiler.

4. **Runtime**
   - Đây là nơi code chạy.
   - JavaScript frontend chạy trong browser.
   - Next.js API/database code chạy trong Node.js server.

Điểm quan trọng: không phải file nào cũng chạy ở cùng một nơi. Code React có thể chạy trên browser, còn API route chạy trên server. Sau này khi dùng Next.js, phải luôn tự hỏi: "Dòng code này chạy ở client hay server?"

## `package.json` là gì?

`package.json` là file mô tả project Node.js.

Ví dụ tối giản:

```json
{
  "name": "crud-web",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "lint": "eslint ."
  },
  "dependencies": {
    "next": "latest",
    "react": "latest",
    "react-dom": "latest"
  },
  "devDependencies": {
    "typescript": "latest"
  }
}
```

Giải thích:

- `name`: tên project.
- `private`: nếu `true`, tránh publish nhầm package lên npm.
- `scripts`: danh sách command bạn có thể chạy bằng `pnpm ten-script`.
- `dependencies`: thư viện app cần khi chạy thật.
- `devDependencies`: thư viện chỉ cần trong lúc code, test, build.

Ví dụ:

```bash
pnpm dev
```

Nghĩa là pnpm tìm script tên `dev` trong `package.json` rồi chạy command bên phải.

## `node_modules` là gì?

`node_modules` là thư mục chứa toàn bộ thư viện đã cài.

Không nên commit `node_modules` lên GitHub vì:

- Rất nặng.
- Có thể cài lại bằng `pnpm install`.
- Mỗi máy có thể có hệ điều hành khác nhau.

Vì vậy `.gitignore` nên có:

```gitignore
node_modules/
```

## Lockfile là gì?

Nếu dùng pnpm, lockfile là `pnpm-lock.yaml`.

Lockfile ghi chính xác version của thư viện đã được cài. Nhờ đó:

- Máy của bạn và máy người khác cài ra cùng version.
- Deploy server ít bị lỗi "máy tôi chạy được, máy khác không chạy".
- Project CV trông nghiêm túc hơn.

Quy tắc:

- Commit lockfile.
- Không tự sửa lockfile bằng tay.
- Khi cài/gỡ thư viện, pnpm tự cập nhật lockfile.

## `.env` và secret

File `.env.local` thường chứa thông tin nhạy cảm:

```env
DATABASE_URL="postgresql://username:password@localhost:5432/crud_web"
AUTH_SECRET="some-secret-value"
```

Không commit `.env.local`.

Nên commit `.env.example`:

```env
DATABASE_URL="file:./dev.db"
AUTH_SECRET="replace-this-value"
```

Mục đích của `.env.example` là cho người khác biết project cần biến môi trường nào, nhưng không lộ secret thật.

## Khi nào cần cài thư viện?

Không cài thư viện chỉ vì thấy người khác dùng.

Nên tự hỏi:

- JavaScript/React/Next có làm được không?
- Thư viện này giải quyết vấn đề thật hay chỉ làm code trông hiện đại?
- Người mới đọc có hiểu được không?
- Thư viện có phổ biến và còn được maintain không?

Ví dụ hợp lý:

- Dùng `zod` để validate dữ liệu API vì validation là việc quan trọng và dễ sai.
- Dùng `prisma` để học ORM/database.

Ví dụ chưa cần:

- Dùng global state library khi app chỉ có một trang CRUD.
- Dùng animation library khi UI chưa ổn.

## Bài tập nhỏ

Trước khi code app, hãy tự chạy và ghi lại kết quả:

```bash
node -v
npm -v
pnpm -v
git --version
```

Nếu command nào lỗi:

- Ghi lại error message.
- Cài công cụ thiếu.
- Mở terminal mới rồi thử lại.

## Checklist tự kiểm tra

- [ ] Biết Node.js dùng để làm gì.
- [ ] Biết `package.json` chứa gì.
- [ ] Biết vì sao không commit `node_modules`.
- [ ] Biết lockfile dùng để làm gì.
- [ ] Biết khác nhau giữa `.env.local` và `.env.example`.
- [ ] Biết project này không cần Python `venv` cho app chính.

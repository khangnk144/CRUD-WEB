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


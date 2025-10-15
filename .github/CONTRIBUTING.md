# Đóng Góp Cho OpenCut

Cảm ơn bạn đã quan tâm đến việc đóng góp cho OpenCut! Tài liệu này cung cấp hướng dẫn và chỉ dẫn để đóng góp.

## Bắt Đầu

### Yêu Cầu Tiên Quyết

- [Node.js](https://nodejs.org/en/) (v18 trở lên)
- [Bun](https://bun.sh/docs/installation)
  (thay thế cho `npm`)
- [Docker](https://docs.docker.com/get-docker/) và [Docker Compose](https://docs.docker.com/compose/install/)

> **Lưu ý:** Docker là tùy chọn, nhưng nó rất cần thiết để chạy database và Redis services cục bộ. Nếu bạn đang có kế hoạch đóng góp cho các tính năng frontend, bạn có thể bỏ qua việc cài đặt Docker. Nếu bạn đã làm theo các bước bên dưới trong [Setup](#setup), bạn đã sẵn sàng!

### Cài Đặt (Setup)

1. Fork repository
2. Clone fork của bạn về máy cục bộ
3. Di chuyển đến thư mục web app: `cd apps/web`
4. Copy `.env.example` thành `.env.local`:

   ```bash
   # Unix/Linux/Mac
   cp .env.example .env.local

   # Windows Command Prompt
   copy .env.example .env.local

   # Windows PowerShell
   Copy-Item .env.example .env.local
   ```

5. Cài đặt dependencies: `bun install`
6. Khởi động development server: `bun run dev`

> **Lưu ý:** Nếu bạn gặp lỗi như `Unsupported URL Type "workspace:*"` khi chạy `npm install`, bạn có hai lựa chọn:
>
> 1. Nâng cấp lên phiên bản npm gần đây (v9 trở lên), có hỗ trợ đầy đủ workspace protocol.
> 2. Sử dụng package manager thay thế như **bun** hoặc **pnpm**.

## Nên Tập Trung Vào Đâu

**🎯 Các Lĩnh Vực Tốt Để Đóng Góp:**

- Chức năng timeline và cải thiện UI
- Tính năng quản lý project
- Tối ưu hóa hiệu suất
- Sửa bug trong chức năng hiện có
- Cải thiện UI/UX
- Documentation và testing

**⚠️ Các Lĩnh Vực Nên Tránh:**

- Cải thiện preview panel (text fonts, stickers, effects)
- Cải thiện chức năng export
- Tối ưu hóa preview rendering

**Tại sao?** Chúng tôi hiện đang lên kế hoạch refactor lớn cho hệ thống preview. Preview hiện tại render các phần tử DOM (HTML), nhưng chúng tôi đang chuyển sang phương pháp binary rendering tương tự như CapCut. Hệ thống mới này sẽ đảm bảo tính nhất quán giữa preview và export, đồng thời cung cấp hiệu suất và chất lượng tốt hơn nhiều.

Preview dựa trên HTML hiện tại về cơ bản là một prototype - phương pháp binary sẽ là "sản phẩm thực sự". Để tránh lãng phí công sức, vui lòng tập trung vào các lĩnh vực khác của ứng dụng cho đến khi refactor này hoàn thành.

Nếu bạn không chắc chắn liệu ý tưởng của mình có thuộc về danh mục preview hay không, hãy hỏi chúng tôi [trực tiếp trong discord](https://discord.gg/zmR9N35cjK) hoặc tạo một GitHub issue!

## Cài Đặt Development

### Local Development

1. Khởi động database và Redis services:

   ```bash
   # Từ thư mục gốc của project
   docker-compose up -d
   ```

2. Di chuyển đến thư mục web app:

   ```bash
   cd apps/web
   ```

3. Copy `.env.example` thành `.env.local`:

   ```bash
   # Unix/Linux/Mac
   cp .env.example .env.local

   # Windows Command Prompt
   copy .env.example .env.local

   # Windows PowerShell
   Copy-Item .env.example .env.local
   ```

4. Cấu hình các biến môi trường bắt buộc trong `.env.local`:

   **Biến Bắt Buộc:**

   ```bash
   # Database (khớp với docker-compose.yaml)
   DATABASE_URL="postgresql://opencut:opencutthegoat@localhost:5432/opencut"

   # Tạo một secret bảo mật cho Better Auth
   BETTER_AUTH_SECRET="your-generated-secret-here"
   NEXT_PUBLIC_BETTER_AUTH_URL="http://localhost:3000"

   # Redis (khớp với docker-compose.yaml)
   UPSTASH_REDIS_REST_URL="http://localhost:8079"
   UPSTASH_REDIS_REST_TOKEN="example_token"

   # Development
   NODE_ENV="development"
   ```

   **Tạo BETTER_AUTH_SECRET:**

   ```bash
   # Unix/Linux/Mac
   openssl rand -base64 32

   # Windows PowerShell (phương pháp đơn giản)
   [System.Web.Security.Membership]::GeneratePassword(32, 0)

   # Cross-platform (sử dụng Node.js)
   node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"

   # Hoặc sử dụng trình tạo online: https://generate-secret.vercel.app/32
   ```

5. Chạy database migrations: `bun run db:migrate`
6. Khởi động development server: `bun run dev`

## Cách Đóng Góp

### Báo Cáo Bug

- Sử dụng template bug report
- Bao gồm các bước để tái hiện
- Cung cấp screenshot nếu có thể

### Đề Xuất Tính Năng

- Sử dụng template feature request
- Giải thích use case
- Cân nhắc chi tiết implementation

### Đóng Góp Code

1. Tạo một branch mới: `git checkout -b feature/your-feature-name`
2. Thực hiện các thay đổi của bạn
3. Di chuyển đến thư mục web app: `cd apps/web`
4. Chạy linter: `bun run lint`
5. Format code của bạn: `bunx biome format --write .`
6. Commit các thay đổi của bạn với message mô tả rõ ràng
7. Push lên fork của bạn và tạo pull request

## Code Style

- Chúng tôi sử dụng Biome cho code formatting và linting
- Chạy `bunx biome format --write .` từ thư mục `apps/web` để format code
- Chạy `bun run lint` từ thư mục `apps/web` để kiểm tra linting issues
- Tuân theo các mẫu code hiện có

## Quy Trình Pull Request

1. Điền đầy đủ pull request template
2. Liên kết bất kỳ issue liên quan nào
3. Đảm bảo CI pass
4. Yêu cầu review từ maintainers
5. Giải quyết mọi feedback

## Cộng Đồng

- Tôn trọng và hòa nhập
- Tuân theo Code of Conduct của chúng tôi
- Giúp đỡ người khác trong discussions và issues

Cảm ơn bạn đã đóng góp!

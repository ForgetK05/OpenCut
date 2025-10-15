<table width="100%">
  <tr>
    <td align="left" width="120">
      <img src="apps/web/public/logo.png" alt="OpenCut Logo" width="100" />
    </td>
    <td align="right">
      <h1>OpenCut</span></h1>
      <h3 style="margin-top: -10px;">Trình chỉnh sửa video miễn phí, mã nguồn mở cho web, desktop và mobile.</h3>
    </td>
  </tr>
</table>

## Tại sao?

- **Quyền riêng tư**: Video của bạn được lưu trữ trên thiết bị của bạn
- **Tính năng miễn phí**: Mọi tính năng cơ bản của CapCut hiện đều phải trả phí
- **Đơn giản**: Mọi người muốn các trình chỉnh sửa dễ sử dụng - CapCut đã chứng minh điều đó

## Tính năng

- Chỉnh sửa dựa trên timeline
- Hỗ trợ đa track
- Preview theo thời gian thực
- Không watermark hoặc subscription
- Analytics được cung cấp bởi [Databuddy](https://www.databuddy.cc?utm_source=opencut), 100% Ẩn danh & Không xâm phạm.
- Blog được hỗ trợ bởi [Marble](https://marblecms.com?utm_source=opencut), Headless CMS.

## Cấu trúc dự án

- `apps/web/` – Ứng dụng web Next.js chính
- `src/components/` – Các component UI và editor
- `src/hooks/` – Custom React hooks
- `src/lib/` – Logic tiện ích và API
- `src/stores/` – Quản lý state (Zustand, v.v.)
- `src/types/` – TypeScript types

## Bắt đầu

### Yêu cầu

Trước khi bắt đầu, hãy đảm bảo bạn đã cài đặt những thứ sau trên hệ thống của mình:

- [Node.js](https://nodejs.org/en/) (v18 hoặc mới hơn)
- [Bun](https://bun.sh/docs/installation)
  (thay thế cho `npm`)
- [Docker](https://docs.docker.com/get-docker/) và [Docker Compose](https://docs.docker.com/compose/install/)

> **Lưu ý:** Docker là tùy chọn, nhưng nó rất cần thiết để chạy database và Redis services cục bộ. Nếu bạn chỉ định chạy frontend hoặc muốn đóng góp cho các tính năng frontend, bạn có thể bỏ qua việc cài đặt Docker. Nếu bạn đã làm theo các bước bên dưới trong [Cài đặt](#cài-đặt), bạn đã sẵn sàng!

### Cài đặt

1. Fork repository
2. Clone fork của bạn về máy cục bộ
3. Di chuyển đến thư mục web app: `cd apps/web`
4. Sao chép `.env.example` sang `.env.local`:

   ```bash
   # Unix/Linux/Mac
   cp .env.example .env.local

   # Windows Command Prompt
   copy .env.example .env.local

   # Windows PowerShell
   Copy-Item .env.example .env.local
   ```

5. Cài đặt dependencies: `bun install`
6. Khởi động development server: `bun dev`

## Thiết lập Development

### Local Development

1. Khởi động database và Redis services:

   ```bash
   # Từ thư mục gốc dự án
   docker-compose up -d
   ```

2. Di chuyển đến thư mục web app:

   ```bash
   cd apps/web
   ```

3. Sao chép `.env.example` sang `.env.local`:

   ```bash
   # Unix/Linux/Mac
   cp .env.example .env.local

   # Windows Command Prompt
   copy .env.example .env.local

   # Windows PowerShell
   Copy-Item .env.example .env.local
   ```

4. Cấu hình các biến môi trường cần thiết trong `.env.local`:

   **Các biến bắt buộc:**

   ```bash
   # Database (khớp với docker-compose.yaml)
   DATABASE_URL="postgresql://opencut:opencutthegoat@localhost:5432/opencut"

   # Tạo một secret an toàn cho Better Auth
   BETTER_AUTH_SECRET="your-generated-secret-here"
   BETTER_AUTH_URL="http://localhost:3000"

   # Redis (khớp với docker-compose.yaml)
   UPSTASH_REDIS_REST_URL="http://localhost:8079"
   UPSTASH_REDIS_REST_TOKEN="example_token"

   # Marble Blog
   MARBLE_WORKSPACE_KEY=cm6ytuq9x0000i803v0isidst # example organization key
   NEXT_PUBLIC_MARBLE_API_URL=https://api.marblecms.com

   # Development
   NODE_ENV="development"
   ```

   **Tạo BETTER_AUTH_SECRET:**

   ```bash
   # Unix/Linux/Mac
   openssl rand -base64 32

   # Windows PowerShell (phương pháp đơn giản)
   [System.Web.Security.Membership]::GeneratePassword(32, 0)

   # Đa nền tảng (sử dụng Node.js)
   node -e "console.log(require('crypto').randomBytes(32).toString('base64'))"

   # Hoặc sử dụng trình tạo online: https://generate-secret.vercel.app/32
   ```

5. Chạy database migrations: `bun run db:migrate` từ (bên trong apps/web)
6. Khởi động development server: `bun run dev` từ (bên trong apps/web)

Ứng dụng sẽ có sẵn tại [http://localhost:3000](http://localhost:3000).

## Đóng góp

Chúng tôi hoan nghênh các đóng góp! Trong khi chúng tôi đang tích cực phát triển và tái cấu trúc một số khu vực nhất định, có rất nhiều cơ hội để đóng góp hiệu quả.

**🎯 Các lĩnh vực trọng tâm:** Chức năng timeline, quản lý dự án, hiệu suất, sửa lỗi và cải thiện UI bên ngoài preview panel.

**⚠️ Tránh hiện tại:** Cải tiến preview panel (fonts, stickers, effects) và chức năng export - chúng tôi đang tái cấu trúc những phần này với phương pháp rendering binary mới.

Xem [Hướng dẫn đóng góp](.github/CONTRIBUTING.md) của chúng tôi để biết hướng dẫn thiết lập chi tiết, nguyên tắc development và hướng dẫn lĩnh vực trọng tâm đầy đủ.

**Bắt đầu nhanh cho contributors:**

- Fork repo và clone về máy cục bộ
- Làm theo hướng dẫn thiết lập trong CONTRIBUTING.md
- Tạo feature branch và submit một PR

## Nhà tài trợ

<a href="https://fal.ai">
  <img alt="Powered by fal.ai" src="https://img.shields.io/badge/Powered%20by-fal.ai-000000?style=flat&logo=data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTEyIDJMMTMuMDkgOC4yNkwyMCAxMEwxMy4wOSAxNS43NEwxMiAyMkwxMC45MSAxNS43NEw0IDEwTDEwLjkxIDguMjZMMTIgMloiIGZpbGw9IndoaXRlIi8+Cjwvc3ZnPgo=" />
</a>

---

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FOpenCut-app%2FOpenCut&project-name=opencut&repository-name=opencut)

## Giấy phép

[MIT LICENSE](LICENSE)

---

![Star History Chart](https://api.star-history.com/svg?repos=opencut-app/opencut&type=Date)

# CLAUDE.md

File này cung cấp hướng dẫn cho Claude Code (claude.ai/code) khi làm việc với code trong repository này.

## Tổng Quan Dự Án

OpenCut là một video editor miễn phí, open-source được xây dựng với Next.js, tập trung vào privacy (không xử lý trên server), multi-track timeline editing, và real-time preview. Dự án là một monorepo sử dụng Turborepo với nhiều apps bao gồm web application, desktop app (Tauri), background remover tools, và transcription services.

## Các Lệnh Cơ Bản

**Development:**
```bash
# Root level development
bun dev                    # Khởi động tất cả apps ở development mode
bun build                  # Build tất cả apps
bun lint                   # Lint tất cả code sử dụng Ultracite
bun format                 # Format tất cả code sử dụng Ultracite

# Web app specific (từ apps/web/)
cd apps/web
bun run dev                # Khởi động Next.js development server với Turbopack
bun run build              # Build cho production
bun run lint               # Chạy Biome linting
bun run lint:fix           # Tự động fix các linting issues
bun run format             # Format code với Biome

# Database operations (từ apps/web/)
bun run db:generate        # Generate Drizzle migrations
bun run db:migrate         # Chạy migrations
bun run db:push:local      # Push schema đến local development database
bun run db:push:prod       # Push schema đến production database
```

**Testing:**
- Hiện chưa có test commands thống nhất được cấu hình
- Các apps riêng lẻ có thể có test setups riêng

## Kiến Trúc & Các Component Chính

### State Management
Application sử dụng **Zustand** cho state management với các stores riêng biệt cho các mối quan tâm khác nhau:
- **editor-store.ts**: Canvas presets, layout guides, app initialization
- **timeline-store.ts**: Timeline tracks, elements, playback state
- **media-store.ts**: Media files và asset management
- **playback-store.ts**: Video playback controls và timing
- **project-store.ts**: Project-level data và persistence
- **panel-store.ts**: UI panel visibility và layout
- **keybindings-store.ts**: Keyboard shortcut management
- **sounds-store.ts**: Audio effects và sound management
- **stickers-store.ts**: Sticker/graphics management

### Storage System
**Cách tiếp cận multi-layer storage:**
- **IndexedDB**: Projects, saved sounds, và structured data
- **OPFS (Origin Private File System)**: Large media files để có performance tốt hơn
- **Storage Service** (`lib/storage/`): Abstraction layer quản lý cả hai loại storage

### Editor Architecture
**Core editor components:**
- **Timeline Canvas**: Custom canvas-based timeline với tracks và elements
- **Preview Panel**: Real-time video preview (hiện tại là DOM-based, có kế hoạch binary refactor)
- **Media Panel**: Asset management với drag-and-drop support
- **Properties Panel**: Context-sensitive element properties

### Media Processing
- **FFmpeg Integration**: Client-side video processing sử dụng @ffmpeg/ffmpeg
- **Background Removal**: Python-based tools với nhiều AI models (U2Net, SAM, Gemini)
- **Transcription**: Service riêng cho audio-to-text conversion

## Các Lĩnh Vực Phát Triển Trọng Tâm

**✅ Các lĩnh vực đóng góp được khuyến nghị:**
- Timeline functionality và UI improvements
- Project management features
- Performance optimizations
- Bug fixes trong existing functionality
- UI/UX improvements bên ngoài preview panel
- Documentation và testing

**⚠️ Các lĩnh vực nên tránh (đang chờ refactor):**
- Preview panel enhancements (fonts, stickers, effects)
- Export functionality improvements
- Preview rendering optimizations

**Lý do:** Preview system đang có kế hoạch refactor lớn từ DOM-based rendering sang binary rendering để đồng nhất với export và có performance tốt hơn.

## Các Tiêu Chuẩn Code Quality

**Linting & Formatting:**
- Sử dụng **Biome** cho JavaScript/TypeScript linting và formatting
- Extends **Ultracite** configuration cho strict type safety và AI-friendly code
- Comprehensive accessibility (a11y) rules được enforced
- Zero configuration approach với subsecond performance

**Các coding standards chính từ Ultracite:**
- Strict TypeScript không có `any` types
- Không React imports (sử dụng automatic JSX runtime)
- Comprehensive accessibility requirements
- Sử dụng `for...of` thay vì `Array.forEach`
- Không TypeScript enums, sử dụng const objects
- Luôn bao gồm error handling với try-catch

## Cài Đặt Environment

**Các environment variables bắt buộc (apps/web/.env.local):**
```bash
# Database
DATABASE_URL="postgresql://opencut:opencutthegoat@localhost:5432/opencut"

# Authentication
BETTER_AUTH_SECRET="your-generated-secret-here"
BETTER_AUTH_URL="http://localhost:3000"

# Redis
UPSTASH_REDIS_REST_URL="http://localhost:8079"
UPSTASH_REDIS_REST_TOKEN="example_token"

# Content Management
MARBLE_WORKSPACE_KEY="workspace-key"
NEXT_PUBLIC_MARBLE_API_URL="https://api.marblecms.com"
```

**Docker services:**
```bash
# Khởi động local database và Redis
docker-compose up -d
```

## Cấu Trúc Dự Án

**Monorepo layout:**
- `apps/web/` - Main Next.js application
- `apps/desktop/` - Tauri desktop application
- `apps/bg-remover/` - Python background removal tools
- `apps/transcription/` - Audio transcription service
- `packages/` - Shared packages (auth, database)

**Web app structure:**
- `src/components/` - React components được tổ chức theo feature
- `src/stores/` - Zustand state management
- `src/hooks/` - Custom React hooks
- `src/lib/` - Utility functions và services
- `src/types/` - TypeScript type definitions
- `src/app/` - Next.js app router pages và API routes

## Các Patterns Phổ Biến

**Error handling:**
```typescript
try {
  const result = await processData();
  return { success: true, data: result };
} catch (error) {
  console.error('Operation failed:', error);
  return { success: false, error: error.message };
}
```

**Store usage:**
```typescript
const { tracks, addTrack, updateTrack } = useTimelineStore();
```

**Media processing:**
```typescript
import { processVideo } from '@/lib/ffmpeg-utils';
const processedVideo = await processVideo(inputFile, options);
```
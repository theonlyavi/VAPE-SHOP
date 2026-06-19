# VapeShop — Setup Guide

## Stack
- **Frontend**: Next.js 14 (App Router) · TypeScript · TailwindCSS · Zustand · Framer Motion · next-intl
- **Backend**: NestJS · Prisma ORM · PostgreSQL · JWT Auth
- **Infrastructure**: Docker · Nginx
- **i18n**: English + Russian

---

## Quick Start (Development)

### 1. Start PostgreSQL
```bash
docker compose -f docker-compose.dev.yml up -d
```

### 2. Backend
```bash
cd backend
npm install
npx prisma migrate dev --name init
npx prisma generate
npm run prisma:seed          # Seeds admin user + demo products
npm run start:dev            # → http://localhost:4000
```

**Admin credentials (from seed):**
- Email: `admin@vapeshop.com`
- Password: `Admin123!`

### 3. Frontend
```bash
cd frontend
npm install
npm run dev                  # → http://localhost:3000
```

---

## URLs
| Service      | URL                             |
|--------------|---------------------------------|
| Frontend     | http://localhost:3000/en        |
| Backend API  | http://localhost:4000/api       |
| Swagger Docs | http://localhost:4000/api/docs  |
| Admin Panel  | http://localhost:3000/en/admin  |

---

## Production (Docker)
```bash
# Copy and edit .env files first
cp backend/.env.example backend/.env
# Edit backend/.env with production values

docker compose up -d --build
```

---

## Key Features Implemented
- ✅ Dark cyber/premium theme
- ✅ Landing page (Hero, Features, Featured Products, News)
- ✅ Product catalog with search, filters, pagination
- ✅ Inventory management (IN_STOCK / LOW_STOCK / OUT_OF_STOCK)
- ✅ Configurable OOS behavior: hide product OR disable button
- ✅ Order flow via Telegram / Instagram / Discord (no payment required)
- ✅ News/Events/Promotions section
- ✅ Admin dashboard with CRUD for all entities
- ✅ EN + RU multilingual support (next-intl)
- ✅ JWT authentication for admin
- ✅ Prisma ORM + PostgreSQL
- ✅ Docker + Nginx production setup
- ✅ Swagger API docs
- ✅ Stock auto-sync endpoint
- ✅ Promo banner (configurable from admin)
- ✅ Age verification setting
- ✅ Maintenance mode setting
- ✅ Image upload system

---

## Project Structure
```
vape-shop/
├── backend/                 # NestJS API
│   ├── prisma/
│   │   ├── schema.prisma    # Full DB schema
│   │   └── seed.ts          # Demo data + admin user
│   └── src/
│       ├── modules/
│       │   ├── auth/        # JWT auth
│       │   ├── products/    # Product CRUD
│       │   ├── categories/  # Category management
│       │   ├── inventory/   # Stock management
│       │   ├── news/        # News/events
│       │   ├── upload/      # File uploads
│       │   ├── settings/    # Key-value config store
│       │   └── analytics/   # Dashboard stats
│       └── common/          # Guards, filters, interceptors
├── frontend/                # Next.js 14
│   ├── app/[locale]/        # i18n App Router pages
│   ├── components/          # UI, layout, landing, catalog, news, admin
│   ├── lib/                 # API client, Zustand stores, utils
│   ├── messages/            # EN + RU translations
│   └── types/               # TypeScript interfaces
├── nginx/                   # Reverse proxy config
├── docker-compose.yml       # Production
└── docker-compose.dev.yml   # Dev (DB only)
```

---

## API Endpoints (Public)
| Method | Endpoint               | Description               |
|--------|------------------------|---------------------------|
| GET    | /api/products          | List products (filterable) |
| GET    | /api/products/:id      | Single product            |
| GET    | /api/categories        | List categories           |
| GET    | /api/news              | List news/events          |
| GET    | /api/news/:id          | Single post               |
| GET    | /api/settings/public   | Public settings           |

## API Endpoints (Admin — requires Bearer token)
| Method | Endpoint                    | Description           |
|--------|-----------------------------|-----------------------|
| POST   | /api/auth/login             | Admin login           |
| GET    | /api/auth/me                | Current user          |
| POST   | /api/products               | Create product        |
| PUT    | /api/products/:id           | Update product        |
| PATCH  | /api/products/:id/visibility| Toggle visibility     |
| DELETE | /api/products/:id           | Delete product        |
| GET    | /api/inventory              | All inventory         |
| PUT    | /api/inventory/:productId   | Update stock          |
| POST   | /api/inventory/bulk         | Bulk update           |
| POST   | /api/inventory/sync         | Auto-sync statuses    |
| GET    | /api/analytics/dashboard    | Dashboard stats       |
| POST   | /api/settings/bulk          | Update settings       |
| POST   | /api/upload/image           | Upload image          |

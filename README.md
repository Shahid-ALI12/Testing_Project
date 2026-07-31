# 🐄 Cattle Feed ERP

> **Production-grade, full-stack business management platform built for feed mills and cattle feed distributors.**

[![Next.js](https://img.shields.io/badge/Next.js-14-black?logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-green?logo=supabase&logoColor=white)](https://supabase.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3-38BDF8?logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-latest-000000?logo=shadcnui&logoColor=white)](https://ui.shadcn.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📖 Overview

**Cattle Feed ERP** unifies sales, purchases, inventory, customer credit (khata), custom mix-order billing, cash ledger, and day reconciliation into a single dual-portal web application — with admin and customer logins, atomic PostgreSQL RPCs for data integrity, bcrypt-secured authentication, and WhatsApp-delivered PDF invoices.

The system was hardened from a mock-data prototype into a real, deployable financial application with server-only secrets, role-based access, and reconciled cash flows. Every business operation is wrapped in atomic database transactions to guarantee consistency — no partial writes, no orphaned ledgers, no stock drift.

---

## ✨ Key Features

| Module | Description |
|--------|-------------|
| 📊 **Real-time Dashboard** | Daily sales, expenses, cash position, and customer balances at a glance |
| 🛒 **Point of Sale** | Atomic `create_sale` RPC with stock-aware inventory |
| 🐄 **Custom Mix Orders** | Ingredient-level feed formulation with auto-calculated Rate/Bag |
| 📦 **Purchases & Stock** | GRN-based intake, supplier ledger, `product_stock` tracking |
| 💵 **Customer Khata** | Running balance, per-customer ledger, PDF billing |
| 💰 **Cash Management** | Multi-account ledger, transfers, correction entries |
| 🧾 **Day Reconciliation** | Automated sales / expense / cash close |
| 📲 **WhatsApp Billing** | Auto-attached PDF invoices via `wa.me` deep links |
| 👥 **Dual Portal** | Admin + Customer login (bcrypt, httpOnly cookies) |
| 🔐 **Hardened Auth** | Server-only JWT secret, no `NEXT_PUBLIC_` leakage |
| 📄 **PDF & HTML Bills** | jsPDF + printable HTML bills for every order type |
| 🗄️ **Supabase + PostgreSQL** | 13 tables, 7 atomic RPC functions, RLS-ready |
| 👷 **Labour Khata** | Daily wages, monthly summaries, payment tracking |

---

## 🛠️ Tech Stack

### Frontend
- **Next.js 14** (App Router) · **React 18**
- **TypeScript** · strict mode
- **Tailwind CSS** · **shadcn/ui** · **Radix UI**

### Backend
- **Next.js Route Handlers** (API routes)
- **Supabase** (PostgreSQL) · **Prisma** ORM
- **Atomic RPC functions** for all financial mutations

### Auth & Security
- **bcryptjs** (12 salt rounds) for password hashing
- **Server-only JWT** signed with `CUSTOMER_TOKEN_SECRET`
- **httpOnly cookies** — no client-side token access
- No `NEXT_PUBLIC_` secrets in auth flow

### Billing & Messaging
- **jsPDF** for PDF bill generation
- **HTML print** bills for every order type
- **WhatsApp `wa.me` deep links** for invoice delivery

### DevOps
- **ESLint** · **Caddy** reverse proxy · **bun** support
- Database backup / restore (JSON export)

---

## 🚀 Getting Started

### Prerequisites

- Node.js ≥ 18.x (or [bun](https://bun.sh/))
- A Supabase project (free tier works)
- PostgreSQL 15+ (provided by Supabase)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/Shahid-ALI12/Juniors_Project.git
cd juniors_project

# 2. Install dependencies
npm install
# or
bun install

# 3. Set up environment variables
cp .env.example .env.local
# Fill in your Supabase URL, anon key, service role key, and CUSTOMER_TOKEN_SECRET

# 4. Apply database schema
# Run supabase/schema.sql in your Supabase SQL editor
# Then run any supabase/*.sql migrations in order

# 5. Start the dev server
npm run dev
# or
bun dev
---

## 📂 Project Structure

```
.
├── src/
│   ├── app/                    # Next.js App Router
│   │   ├── admin/              # Admin portal
│   │   ├── customer/           # Customer portal
│   │   ├── api/                # API route handlers
│   │   │   ├── sales/
│   │   │   ├── purchases/
│   │   │   ├── mix-orders/
│   │   │   ├── cash/
│   │   │   ├── reports/
│   │   │   ├── customers/
│   │   │   ├── products/
│   │   │   ├── expenses/
│   │   │   ├── auth/
│   │   │   └── database/
│   │   └── layout.tsx
│   ├── components/
│   │   ├── pages/              # Page-level components
│   │   ├── ui/                 # shadcn/ui primitives
│   │   ├── shared/             # Shared components
│   │   ├── layout/             # Layout components
│   │   └── auth/               # Auth providers
│   ├── lib/
│   │   ├── data/               # Data access layer
│   │   ├── supabase/           # Supabase clients
│   │   ├── auth/               # Auth utilities
│   │   ├── generate-*.ts       # PDF / bill generators
│   │   ├── share-whatsapp.ts   # WhatsApp integration
│   │   └── cache.ts            # Caching layer
│   ├── hooks/                  # React Query hooks
│   ├── store/                  # Zustand store
│   ├── types/                  # TypeScript types
│   └── middleware.ts           # Auth middleware
├── supabase/
│   ├── schema.sql              # 13-table schema
│   ├── all-rpc-functions.sql   # 7 atomic RPC functions
│   └── *.sql                   # Migrations & fixes
├── prisma/
│   └── schema.prisma
├── public/
├── scripts/                    # Verification scripts
├── Caddyfile                   # Reverse proxy config
└── package.json
```

---

## 🗄️ Database Schema

The schema consists of **13 tables** and **7 atomic RPC functions**:

### Tables
`products` · `locations` · `customers` · `suppliers` · `sales` · `mix_orders` · `expenses` · `purchases` · `product_stock` · `cash_accounts` · `cash_ledger` · `cash_transfers` · `app_customers`

### Atomic RPC Functions
| Function | Purpose |
|----------|---------|
| `create_sale` | Atomic sale creation with stock update |
| `create_mix_order` | Custom mix order with ingredients |
| `record_purchase` | GRN-based stock intake |
| `record_expense` | Expense entry with cash ledger update |
| `transfer_cash` | Inter-account cash transfer |
| `correct_cash_balance` | Cash correction entry |
| `verify_customer_login` | Customer auth verification |

All mutations run inside PostgreSQL transactions — no partial writes.

---

## 🔐 Security

This project was hardened from a prototype into a production-ready financial application:

- ✅ **Password hashing** — bcryptjs with 12 salt rounds
- ✅ **Server-only JWT secret** — `CUSTOMER_TOKEN_SECRET`, never exposed to client
- ✅ **httpOnly cookies** — tokens not accessible via JavaScript
- ✅ **No `NEXT_PUBLIC_` leakage** in auth flow
- ✅ **Role-based access** — admin vs customer portals
- ✅ **Atomic transactions** — no partial financial writes
- ✅ **Rate limiting** — on auth endpoints
- ✅ **Soft deletes** — for customers and products

---

## 📲 WhatsApp Billing

Invoices are generated as PDFs via jsPDF and delivered through WhatsApp using `wa.me` deep links. The customer's phone number and a pre-filled message with the PDF attachment are passed to WhatsApp, where the admin selects the chat and sends.

---

## 🧾 Bill Types

The system generates bills for every order type:

- **Mix Order Bill** (PDF + HTML print) — ingredient-level breakdown with Rate/Bag
- **Customer Khata Bill** (PDF) — running ledger summary
- **Purchase Bill** (PDF) — supplier GRN receipt
- **Buy Product Bill** (PDF) — direct product sale

---

## 🤝 Dual Portal Architecture

### Admin Portal (`/admin`)
- Dashboard, sales, purchases, stock, mix orders
- Customer & product management
- Cash management & day reconciliation
- Labour khata & wages
- Database backup / restore

### Customer Portal (`/customer`)
- Login with phone + password
- View own khata / ledger
- Download PDF bills
- View outstanding balance

---

## 📊 Day Reconciliation

Automated end-of-day close that verifies:

- Total sales vs cash received
- Total expenses vs cash disbursed
- Cash account balances across all accounts
- Customer payment reconciliation

Discrepancies are flagged for review.

---

## 📦 Database Migrations

Migrations live in `supabase/` and should be applied in order:

```bash
# 1. Base schema
supabase/schema.sql

# 2. RPC functions
supabase/all-rpc-functions.sql

# 3. Feature migrations (in order)
supabase/add-customer-soft-delete.sql
supabase/add-product-soft-delete.sql
supabase/add-customer-balance-rpc.sql
supabase/add-dashboard-reconciliation-rpc.sql
supabase/add-customer-opening-balance.sql
supabase/add-customer-advance-payments.sql
supabase/add-labour-location.sql
supabase/add-labour-daily-wages.sql
supabase/labours-khata.sql
supabase/disable-sale-stock-decrement.sql
# ... and others
```

---

## 🧪 Scripts

Utility scripts in `scripts/`:

- `generate-summary.js` — DB summary report
- `verify-dashboard-rpc.ts` — Dashboard RPC verification
- `verify-reconciliation-rpc.ts` — Reconciliation RPC verification
- `verify-customer-balance-rpc.ts` — Customer balance RPC verification
- `verify-cache.ts` — Cache layer verification
- `supabase-fix-all.py` — Bulk Supabase fixes

---

## 🚢 Deployment

### Production Build

```bash
npm run build
npm start
```

### Reverse Proxy (Caddy)

A `Caddyfile` is included for production HTTPS termination:

```bash
caddy run --config Caddyfile
```

---

## 📝 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

Built with ❤️ for feed mill businesses.

- GitHub: [@Shahid-ALI12](https://github.com/Shahid-ALI12)
- Email: shahidshafaqat2007@gmail.com

---

## 🙏 Acknowledgments

- [Next.js](https://nextjs.org/) — React framework
- [Supabase](https://supabase.com/) — PostgreSQL backend
- [shadcn/ui](https://ui.shadcn.com/) — UI components
- [Tailwind CSS](https://tailwindcss.com/) — Styling
- [jsPDF](https://github.com/parallax/jsPDF) — PDF generation

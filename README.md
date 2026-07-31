Cattle Feed ERP is a production-grade, full-stack business management platform built for feed mills and cattle feed distributors. It unifies sales, purchases, inventory, customer credit (khata), custom mix-order billing, cash ledger, and day reconciliation into a single dual-portal web application — with admin and customer logins, atomic PostgreSQL RPCs for data integrity, bcrypt-secured authentication, and WhatsApp-delivered PDF invoices. The system was hardened from a mock-data prototype into a real, deployable financial application with server-only secrets, role-based access, and reconciled cash flows.
Key Features
📊 Real-time dashboard — daily sales, expenses, cash position, customer balances
🛒 Point of Sale — atomic create_sale RPC with stock-aware inventory
🐄 Custom Mix Orders — ingredient-level feed formulation with auto-calculated Rate/Bag
📦 Purchases & Stock — GRN-based intake, supplier ledger, product_stock tracking
💵 Customer Khata — running balance, per-customer ledger, PDF billing
💰 Cash Management — multi-account ledger, transfers, correction entries
🧾 Day Reconciliation — automated sales/expense/cash close
📲 WhatsApp Billing — auto-attached PDF invoices via wa.me deep links
👥 Dual Portal — admin + customer login (bcrypt, httpOnly cookies)
🔐 Hardened Auth — server-only JWT secret, no NEXT_PUBLIC_ leakage
📄 PDF & HTML Bills — jsPDF + printable HTML bills for every order type
🗄️ Supabase + PostgreSQL — 13 tables, 7 atomic RPC functions, RLS-ready

Tech Stack
Frontend: Next.js 14 (App Router) · React 18 · TypeScript · Tailwind CSS · shadcn/ui · Radix UI
Backend: Next.js Route Handlers · Supabase (PostgreSQL) · Prisma
Auth: bcryptjs · server-only JWT · httpOnly cookies
Billing: jsPDF · HTML print · WhatsApp wa.me deep links
DevOps: ESLint · Caddy reverse proxy · bun support

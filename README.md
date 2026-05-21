# 💰 SpendWise

A personal finance & budgeting web app that tracks every rupee across **wallets** and **budget categories** at the same time — so you always know both *where* your money is and *what* it's for.

**🔗 Live demo:** [chansenevirathna.github.io/SpendWise](https://chansenevirathna.github.io/SpendWise/)

---

## ✨ Features

- **Google Sign-In & email auth** — secure OAuth 2.0 login with session persistence
- **Multi-wallet tracking** — Cash In Hand, bank accounts & more, with auto-calculated balances
- **Dual allocation** — split each income across wallets *and* budget categories, with live validation ensuring `Income = Σ Wallets = Σ Categories`
- **Budget categories** — track spending against allocated budgets, with overspend alerts
- **Expense tracking** — each expense deducts from one wallet and one category simultaneously
- **Wallet & category management** — add, rename, recolour, and delete (with safe balance transfer on delete)
- **Visual dashboard** — live donut charts for balance-by-wallet and spending-by-category
- **Transaction history** — full chronological log of income and expenses
- **Cloud sync** — all data stored securely and synced per user
- **Dark, mobile-friendly UI** — glassmorphism design with an animated login screen

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | HTML, CSS, Vanilla JavaScript (no framework) |
| **Charts** | Chart.js |
| **Icons** | Tabler Icons |
| **Backend** | Supabase (Backend-as-a-Service) |
| **Database** | PostgreSQL (managed by Supabase) |
| **Authentication** | Supabase Auth + Google OAuth 2.0 |
| **Security** | Postgres Row Level Security (RLS) |
| **Hosting** | GitHub Pages |

> **Architecture:** A serverless / BaaS web app — a real backend and database without a self-managed server. Supabase handles auth, the auto-generated REST API, and the database; the frontend talks to it directly via the Supabase JS client.

---

## 🗄️ Data Model

| Table | Purpose |
|-------|---------|
| `profiles` | User details (name, email, avatar), extends `auth.users` |
| `wallets` | Where money is stored (cash, bank accounts, etc.) |
| `categories` | Budget categories (Travel, Food, etc.) |
| `incomes` | Recorded income entries |
| `expenses` | Recorded expenses (linked to a wallet + category) |
| `allocations` | Income split slices across wallets & categories |
| `transfers` | Wallet-to-wallet transfers (and balance moves on deletion) |

Two helper views — `wallet_balances` and `category_budgets` — compute live balances.

**Balance logic**
```
Wallet balance   = income allocated to wallet + transfers in − transfers out − expenses
Category remaining = income allocated to category − expenses
```

Row Level Security ensures every user can only ever access their own rows.

---

## 🚀 Getting Started

### Prerequisites
- A free [Supabase](https://supabase.com) project
- A [Google Cloud Console](https://console.cloud.google.com) project (for Google Sign-In)

### 1. Set up the database
In your Supabase dashboard, open **SQL Editor → New query**, paste the contents of `schema.sql`, and run it. This creates all tables, RLS policies, and the new-user trigger.

### 2. Configure the app
In `index.html`, set your Supabase credentials near the top of the `<script>` section:
```js
const SUPABASE_URL  = 'https://YOUR_PROJECT.supabase.co';
const SUPABASE_ANON = 'your-anon-public-key';
```
> The **anon public** key is safe in frontend code — Row Level Security is what protects your data. Never use the `service_role` key here.

### 3. Enable Google Sign-In (optional)
1. In **Google Cloud Console**, create an OAuth 2.0 Web Client ID.
2. Add your site to **Authorized JavaScript origins** and your Supabase callback URL (`https://YOUR_PROJECT.supabase.co/auth/v1/callback`) to **Authorized redirect URIs**.
3. Paste the Client ID & Secret into **Supabase → Authentication → Providers → Google**.
4. Set your **Site URL** and **Redirect URLs** under **Authentication → URL Configuration**.

### 4. Deploy
Push `index.html` to your repo and enable **GitHub Pages** (Settings → Pages → deploy from `main` branch, root folder).

---

## 📋 Requirements Coverage

This prototype implements the core functional requirements:

- **FR-001/002** — Google & email auth, session management
- **FR-010–014** — Create, rename, delete wallets (delete requires balance transfer); wallet transfers
- **FR-020–022** — Create, rename, delete categories (delete requires budget reassignment)
- **FR-030–033** — Add income with multi-level wallet & category allocation + validation
- **FR-040–042** — Add expense, deducting from wallet & category
- **FR-050/052** — Dashboard overview & transaction history
- **FR-070** — Cloud sync via Supabase

---

## 🗺️ Roadmap

- [ ] Wallet-to-wallet transfers (standalone, outside deletion)
- [ ] Excel report export (expense / category / wallet-wise) — *FR-053*
- [ ] Date-range filtering on the dashboard
- [ ] Recurring expenses & EMI tracking
- [ ] Receipt scanning (OCR) and bank SMS parsing
- [ ] Multi-currency support
- [ ] AI financial assistant & spending predictions

---

## 📄 License

This project is a personal prototype. Add a license of your choice (e.g. MIT) if you plan to share it.

---

*Built as a personal finance prototype · Currency: LKR · 2026*

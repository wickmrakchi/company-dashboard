# Ouaoua Decor v5.8.1 — Décor Management System

> A full-stack ERP platform for managing a décor business: clients, services, cheques, payments, staff, and notifications.
> Built with **Node.js + Express + MongoDB + EJS**, supporting **Arabic 🇸🇦 · French 🇫🇷 · English 🇺🇸**, **Dark/Light mode**, and a custom **Cheque Reminder System** with email alerts.

---

## 📋 Table of Contents

1. [About the Project](#-about-the-project)
2. [Features](#-features)
3. [Tech Stack](#-tech-stack)
4. [Requirements](#-requirements)
5. [Installation & Setup](#-installation--setup)
6. [Email & Reminders](#-email--reminders)
7. [Project Structure](#-project-structure)
8. [Database](#-database)
9. [Routes](#-routes)
10. [Middleware](#-middleware)
11. [Security](#-security)
12. [Dashboard](#-dashboard)
13. [API Reference](#-api-reference)
14. [Scripts](#-scripts)
15. [Development & Contributing](#-development--contributing)
16. [Responsive Design](#-responsive-design)
17. [License](#-license)

---

## 🚀 About the Project

**Ouaoua Decor v5.8.1** is a complete management system (ERP) for décor companies and shops. It enables client registration, service tracking, payment & cheque management, employee administration, and real-time reporting — all in a branded, gold-accented interface.

### Target Audience
- Décor companies and design offices
- Furniture and home furnishing stores
- Interior design contractors

### Why This System?
- **Fully Cloud** — Runs on MongoDB Atlas, no local server required
- **Multilingual** — 3 languages with automatic RTL for Arabic
- **Dual Theme** — Dark & Light mode with persistent preferences
- **Automated Cheque Reminders** — Email + in-app alerts for upcoming cheques
- **Ready to Use** — Pre-seeded data, login and go
- **Secure** — Protected against CSRF, XSS, NoSQL Injection, Rate Limiting, Helmet headers, and more

---

## ✨ Features

### 🔐 Authentication & Authorization
| Feature | Details |
|---------|---------|
| Login | Username **or** email in a single field |
| 3 Role Levels | `admin` (full access), `manager` (full except staff delete), `employee` (read + edit) |
| Registration Locked | Only while no admin exists — the first admin registers, then it closes |
| Session Security | MongoDB session store + HttpOnly + SameSite Lax cookies |
| Admin Bootstrap | `npm run create-admin` — non-destructive admin creation script |

### 🏦 Cheque Management
- Register cheques with payee, amount, unique number, type (`PRS` / `Company`), date
- Statuses: `Paid` / `Pending` / `Cancelled`
- **Quick status actions** — mark Paid or Cancelled directly from the list & dashboard
- **Urgency row coloring** — due-soon rows highlighted, overdue pending rows flagged
- **Per-cheque reminder mute** — toggle email/in-app reminders for any cheque
- **Pending-cheque badge** in the sidebar nav (live counter)

### ⏰ Cheque Reminder System (v5.x)
- **Automated daily scan** at `REMINDER_HOUR` (default 09:00) via `node-cron`
- Looks at **Pending** cheques due within **0–3 days**
- **In-app notifications** for all active admins (danger = ≤1 day, warning = 2–3 days)
- **HTML email alerts** through Brevo SMTP — color-coded by urgency:
  - 🔴 Due today / tomorrow — red
  - 🟠 Due in 2 days — orange
  - 🟡 Due in 3 days — amber
- **Deduplication** — each cheque+due-date reminder is sent only once (persisted `key` field)
- Email includes payee, amount, number, type, status and a direct link to the cheque list

### 👥 Client Management
- Add, edit, delete clients
- Fields: name, location, phone, email, description, **devis number**, delivery date, notes, status
- **File attachments** — up to 5 uploads per client (JPEG/PNG/GIF/WebP, max 5MB each)
- Statuses: `pending` / `in progress` / `delivered`
- Paginated table with filters (status, search, location, devis, delivery date range)

### 🛠️ Service Management (per staff member)
- Add services to any staff member using a **client dropdown** (auto-fills phone)
- Fields: client, job type, description, total amount, dates, status
- Statuses: `en attente` / `en cours` / `delivery` / `paid`
- Paid/remaining balances computed live from linked payments

### 💳 Payments
- Record payments against any service (amount, date, method, notes)
- Service status auto-updates: full payment → `paid`, partial → `en cours`
- Full payment history per service

### 👨‍💼 Staff Management
- Add, edit, delete staff members (job types, salary, hire date, location, status)
- Detail view with per-staff services and payment aggregation
- Delete restricted to `admin` role

### 🔔 Notifications
- Auto-generated on every create/update/delete/status action
- **Unread badge** on the topbar bell + **pending-cheques badge** in the sidebar
- **Live polling every 30 seconds** — new notifications trigger a toast
- Dedicated notifications page with mark-as-read and mark-all-read
- Last 50 notifications shown, newest first

### 🔍 Search
- Global autocomplete across clients, staff, services, cheques
- Dedicated `/search/results` page with pagination
- XSS-safe rendering (`textContent`, escaped regex)

### 📊 Reports & Export
- Live dashboard statistics + doughnut charts
- Export lists (cheques / clients / staffs) to **Excel (.xlsx)** or **PDF**
- Branded per-staff and per-service **PDF reports** (A4, logo, tables)

### 🎛️ Custom Select Component
- All native `<select>` elements upgraded to branded gold dropdowns
- Keyboard navigation (arrows, Enter, Space, Esc, Home, End)
- Fully RTL-aware, form validation support, zero dependencies
- Auto-initializes — no per-page wiring needed

### 🌐 Multi-Language & Theme
- 3 languages: Arabic (RTL) · French · English — flag dropdown in the topbar (France/Morocco/UK)
- 169 i18n keys; all views, forms, confirms, tooltips, pagination translate dynamically
- Dates format per locale via `Intl.DateTimeFormat` (fr-FR / ar-MA / en-GB)
- Dark/Light mode with localStorage persistence

### 📱 Fully Responsive Design
- Pure CSS media queries — no device-detection hacks
- Collapsible sidebar with overlay + hamburger toggle on mobile
- Sticky topbar with backdrop blur, stacked mobile forms, scrollable tables
- RTL mirrored for Arabic; iOS zoom prevented (`font-size: 16px` inputs)

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| Node.js 18+ | Runtime environment |
| Express.js 4 | Web framework |
| Mongoose 7 | MongoDB ODM |
| MongoDB Atlas | Cloud database |
| express-session + connect-mongo | Session management (MongoDB store) |
| EJS + express-ejs-layouts | Templating & layouts |
| helmet | HTTP security headers |
| express-rate-limit | Rate limiting |
| multer | File uploads |
| bcrypt | Password hashing (12 rounds) |
| method-override | PUT/DELETE support in forms |
| connect-flash | Flash messages |
| exceljs | Excel (.xlsx) export |
| pdfkit | PDF report generation |
| node-cron | Cheque reminder scheduler |
| nodemailer | SMTP email delivery (Brevo) |
| dotenv | Environment configuration |

### Frontend
| Technology | Purpose |
|------------|---------|
| Bootstrap 5 | UI framework (CDN) |
| Chart.js | Doughnut charts (CDN) |
| Font Awesome 6 | Icons (CDN) |
| Custom CSS | Dark/Light/RTL design system, gold theme, custom select |
| Vanilla JS | i18n, search, toasts, custom select, background slideshow |

---

## 📋 Requirements

- **Node.js** v18 or higher
- **npm** v9 or higher
- **MongoDB Atlas** account (free tier) or local MongoDB server
- Internet connection (for cloud database)
- *(Optional)* a **Brevo** (or any SMTP) account for cheque reminder emails

---

## ⚙️ Installation & Setup

### 1. Clone the repository
```bash
git clone <repository-url>
cd "Ouaoua New Version App"
```

### 2. Install dependencies
```bash
npm install
```

### 3. Configure environment variables
Create a `.env` file in the root directory:

```env
# Server
PORT=9001
SESSION_SECRET=your-secret-key-change-this-in-production
MONGODB_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/ouaoua_new
NODE_ENV=development

# Email (Brevo SMTP)
SMTP_HOST=smtp-relay.brevo.com
SMTP_PORT=587
SMTP_USER=your-brevo-smtp-login
SMTP_PASS=your-brevo-smtp-key
SENDER_EMAIL=from@example.com

# Cheque reminder scheduler
REMINDER_HOUR=9
APP_BASE_URL=http://localhost:9001
# Recipient for reminder emails (optional; defaults to admin users' emails)
NOTIFY_EMAIL=finance@example.com
```

### 4. Create the first admin (if the DB is empty)
```bash
npm run create-admin
```

### 5. (Optional) Seed demo data
```bash
npm run seed
```

### 6. Start the server
```bash
npm start
```

Server runs on **http://localhost:9001** 🚀

### 7. Login
| Field | Value |
|-------|-------|
| Username | `admin` |
| Password | `admin123` *(or whatever you set via `create-admin`)* |

> `create-admin` is non-destructive: if the user already exists it makes no changes. Credentials can be overridden with `ADMIN_USERNAME`, `ADMIN_EMAIL`, `ADMIN_PASSWORD` env vars.

---

## 📧 Email & Reminders

### SMTP
- Built with **nodemailer** — works with Brevo, Gmail, or any SMTP provider.
- Configure `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASS`, `SENDER_EMAIL` in `.env`.

### Verify delivery
```bash
npm run test-email                 # uses NOTIFY_EMAIL
npm run test-email someone@x.com   # or a specific address
```

### Reminder scheduler
- Runs **hourly** (`node-cron`) but only performs the daily scan when the hour matches `REMINDER_HOUR` (default `9`).
- On startup it also runs once immediately.
- Sends to `NOTIFY_EMAIL` if set, otherwise to all active admins' emails.
- Deduplicated per cheque + due date — safe to restart the server repeatedly.

---

## 📁 Project Structure

```
Ouaoua New Version App/
├── app.js                        # Entry point (express setup, rate limits, scheduler)
├── package.json
├── .env                          # Environment variables (gitignored)
├── .gitignore
├── README.md
│
├── config/
│   ├── db.js                     # MongoDB connection
│   └── constants.js              # JOB_TYPES, status enums, user roles
│
├── models/
│   ├── User.js                   # bcrypt hashing, comparePassword
│   ├── Client.js                 # + attachments, devisNumber
│   ├── Cheque.js                 # + notificationsEnabled
│   ├── Service.js
│   ├── Payment.js
│   ├── Staff.js
│   ├── Notification.js           # + key (reminder dedupe)
│   └── Log.js
│
├── middleware/
│   ├── auth.js                   # requireAuth + checkRole
│   ├── csrf.js                   # per-session CSRF tokens
│   ├── upload.js                 # extension + MIME whitelist (5MB)
│   └── helpers.js                # paginate, logActivity, createNotification
│
├── routes/
│   ├── auth.js                   # login / register / logout
│   ├── dashboard.js              # stats + upcoming cheques
│   ├── clients.js
│   ├── cheques.js                # + /:id/status, /:id/notifications
│   ├── staffs.js
│   ├── services.js               # nested under /staffs
│   ├── payments.js               # nested under /staffs
│   ├── logs.js
│   ├── notifications.js          # page + unread API
│   ├── search.js                 # autocomplete + results
│   ├── exportRoutes.js           # Excel / PDF reports
│   └── api.js                    # /api/cheques/pending-count
│
├── services/
│   ├── mailer.js                 # Brevo transport + HTML email templates
│   └── chequeReminder.js         # cron scheduler + reminder logic
│
├── views/
│   ├── layout.ejs                # sidebar, topbar, badges, csrf bridge
│   ├── login.ejs / register.ejs  # standalone (custom layout)
│   ├── dashboard.ejs             # stats, charts, upcoming cheques
│   ├── logs.ejs / notifications.ejs / search-results.ejs
│   ├── error.ejs / rate-limit.ejs
│   ├── partials/pagination.ejs
│   ├── cheques/ (list, add, edit)
│   ├── clients/ (list, add, edit)
│   ├── staffs/  (list, add, edit, services)
│   ├── services/ (add, edit)
│   └── payments/ (add)
│
├── public/
│   ├── css/style.css             # gold design system, RTL, responsive
│   ├── js/
│   │   ├── main.js               # toasts, badge polling, setChequeStatus
│   │   ├── i18n.js               # FR / AR / EN translation
│   │   ├── custom-select.js      # branded select component
│   │   ├── search.js             # global autocomplete
│   │   └── background.js         # background slideshow
│   ├── images/                   # slideshow assets
│   ├── uploads/                  # client attachments (gitignored)
│   ├── logo.png                  # brand logo / favicon
│   └── favicon.svg
│
├── scripts/
│   ├── seed.js                   # initial demo data
│   ├── seed-notifications.js     # sample notifications
│   ├── create-admin.js           # non-destructive admin creation
│   └── test-email.js             # SMTP delivery test
│
└── load-test.js                  # k6 load testing script
```

---

## 🗄️ Database

### MongoDB Atlas — Ouaoua New

#### Collections

| Collection | Key Fields |
|------------|------------|
| `users` | username, email, password (bcrypt), role, avatar, isActive, lastLogin |
| `clients` | name, location, phone, email, description, devisNumber, deliveryDate, status, attachments |
| `cheques` | name, amount, number (unique), type, date, deliveryDate, status, notificationsEnabled |
| `services` | staffId, clientName, clientPhone, jobType, description, totalAmount, date, deliveryDate, status |
| `payments` | serviceId, amount, date, method, notes |
| `staffs` | name, phone, email, location, jobType, salary, hireDate, status |
| `notifications` | user, type, title, message, link, key, isRead |
| `logs` | user, action, collection, documentId, details |
| `sessions` | connect-mongo session store |

---

## 🛣️ Routes

### Authentication (no auth required)
| Method | Path | Description | Protection |
|--------|------|-------------|------------|
| GET | `/login` | Login page | — |
| POST | `/login` | Login (username or email) | Rate: 10/15m |
| GET | `/register` | Register page (only while no admin exists) | Rate: 3/hour |
| POST | `/register` | Register first admin | Rate: 3/hour |
| POST | `/logout` | Logout | — |

### Dashboard, Logs & Notifications (auth)
| Method | Path | Description |
|--------|------|-------------|
| GET | `/` | Dashboard (stats, charts, upcoming cheques) |
| GET | `/logs` | Activity log |
| GET | `/notifications` | Notifications page |
| POST | `/notifications/read/:id` | Mark one as read |
| POST | `/notifications/read-all` | Mark all as read |

### Clients (auth)
| Method | Path | Roles |
|--------|------|-------|
| GET | `/clients` | All (filters: status, search, location, devis, date range) |
| GET/POST | `/clients/new` → `/clients` | All |
| GET/PUT | `/clients/:id/edit` → `/clients/:id` | All |
| DELETE | `/clients/:id` | admin, manager |

### Cheques (auth)
| Method | Path | Roles |
|--------|------|-------|
| GET | `/cheques` | All (filters: status, search, amount, date range) |
| GET/POST | `/cheques/new` → `/cheques` | All |
| GET/PUT | `/cheques/:id/edit` → `/cheques/:id` | All |
| POST | `/cheques/:id/status` | All — JSON status flip (Paid/Pending/Cancelled) |
| POST | `/cheques/:id/notifications` | All — toggle reminder mute |
| DELETE | `/cheques/:id` | admin, manager |

### Staff (auth)
| Method | Path | Roles |
|--------|------|-------|
| GET | `/staffs` | All (filters: status, search, location, jobType) |
| GET/POST | `/staffs/new` → `/staffs` | All |
| GET/PUT | `/staffs/:id/edit` → `/staffs/:id` | All |
| GET | `/staffs/:id` | Detail — services + payment aggregation |
| DELETE | `/staffs/:id` | **admin only** |

### Services & Payments (nested, auth)
| Method | Path | Roles |
|--------|------|-------|
| GET/POST | `/staffs/:staffId/services/new` → `/staffs/:staffId/services` | All |
| GET/PUT | `/staffs/:staffId/services/:serviceId/edit` → `...` | All |
| DELETE | `/staffs/:staffId/services/:serviceId` | admin, manager |
| GET/POST | `/staffs/:staffId/services/:serviceId/payments/new` → `.../payments` | All |
| DELETE | `/staffs/:staffId/services/:serviceId/payments/:paymentId` | All |

### Search & API (auth)
| Method | Path | Protection |
|--------|------|------------|
| GET | `/search` | Autocomplete JSON | Rate: 30/min |
| GET | `/search/results` | Paginated search page | Rate: 30/min |
| GET | `/api/cheques/pending-count` | `{ count }` for the nav badge | Rate: 30/min |
| GET | `/api/notifications/unread` | `{ count }` for the bell badge | Rate: 30/min |
| POST | `/api/lang` | Save language preference (no auth) | — |

### Export (auth)
| Method | Path | Protection |
|--------|------|------------|
| GET | `/export/:type/:format` | cheques/clients/staffs → xlsx or pdf | Rate: 10/15min |
| GET | `/export/staff/:id/pdf` | Branded staff report | Rate: 10/15min |
| GET | `/export/service/:serviceId/pdf` | Branded service + payments report | Rate: 10/15min |

---

## 🔧 Middleware

### `middleware/auth.js`
- **requireAuth** — Redirects to `/login` if no valid session
- **checkRole(...roles)** — Rejects if the user's role is not in the allowed list

### `middleware/csrf.js`
- Generates a unique CSRF token per session (32-byte hex)
- Injects `csrfToken` into all views via `res.locals`
- Validates the token on every POST/PUT/DELETE/PATCH via the `_csrf` hidden field **or** the `X-CSRF-Token` header
- Exposed to browser JS as `window.csrfToken` (used by quick-status fetch buttons)
- Returns 403 `Invalid or missing CSRF token` on failure

### `middleware/upload.js`
- Validates both extension **AND** MIME type (not OR)
- Whitelist: `image/jpeg`, `image/png`, `image/gif`, `image/webp`
- Max size: 5MB per file, up to 5 files
- Auto-creates `uploads/` directory

### `middleware/helpers.js`
- `paginate(model, query, page, perPage, sort)` — paginated queries
- `logActivity(action, details, req, collection)` — activity logging
- `createNotification(user, type, title, message, link)` — in-app notifications

---

## 🔐 Security

### Protected Against

| Vulnerability | Protection |
|---------------|------------|
| **CSRF** | Per-session token validated on all state-changing requests (form field + header) |
| **XSS** | `textContent` for autocomplete; EJS auto-escapes output |
| **NoSQL Injection** | Inputs cast to `String()` before building queries; escaped regex |
| **Account Enumeration** | Uniform error: "Invalid username or password" |
| **Brute Force** | Rate limiting on login (10/15m) and register (3/hour) |
| **Source Exposure** | `.gitignore` blocks `.env`, `node_modules/`, uploads, secrets |
| **Malicious Uploads** | Extension + MIME whitelist (AND logic) |
| **Error Leakage** | Generic error pages; stack only logged server-side |
| **Privilege Escalation** | `checkRole` on all delete routes |
| **Weak Sessions** | MongoDB store + HttpOnly + SameSite Lax + `secure` in production |
| **Sniffing** | Helmet security headers (CSP disabled for CDN assets) |

### Rate Limiting Policy
| Path | Limit | Window |
|------|-------|--------|
| Global (authenticated routes) | 200 | 15 min |
| Login | 10 | 15 min |
| Register | 3 | 1 hour |
| API (`/api/*`) | 30 | 1 min |
| Search | 30 | 1 min |
| Export | 10 | 15 min |

### Role Hierarchy
```
admin    → everything (create, read, update, delete)
manager  → everything except staff deletion
employee → create, read, update only (no delete)
```

---

## 📊 Dashboard

After login, the dashboard displays:

### Stat Cards
- 🏦 **Total Cheques** — with paid / pending / cancelled breakdown
- 👥 **Total Clients** — with pending / in progress / delivered breakdown
- 👨‍💼 **Total Staff** — with active / inactive breakdown
- 💰 **Total Revenue** — sum of all payments + paid/total services

### Upcoming Cheques
- The **5 oldest pending cheques** (including overdue ones)
- **Quick actions** — ✓ Mark Paid / ✕ Cancel inline (no page reload)
- **Urgency coloring** — due ≤ 3 days highlighted, overdue pending flagged

### Charts
- 🍩 **Cheques Overview** — paid / pending / cancelled doughnut (Chart.js)
- 🍩 **Clients Status** — pending / in progress / delivered doughnut

---

## 🌐 API Reference

### `POST /api/lang`
Save language preference to session.
```json
// Request
{ "lang": "ar" }
// Response
{ "ok": true }
```

### `GET /api/notifications/unread`
```json
{ "count": 3 }
```

### `GET /api/cheques/pending-count`
```json
{ "count": 2 }
```

### `POST /cheques/:id/status` (fetch, with `X-CSRF-Token`)
```json
// Request
{ "status": "Paid" }
// Response
{ "ok": true, "status": "Paid" }
```

---

## 🧰 Scripts

| Command | Description |
|---------|-------------|
| `npm start` | Run the server |
| `npm run dev` | Run with nodemon (auto-restart) |
| `npm run seed` | Seed initial demo data |
| `npm run create-admin` | Create the first admin (non-destructive) |
| `npm run test-email` | Send a test SMTP email to `NOTIFY_EMAIL` |
| `node scripts/seed-notifications.js` | Seed sample notifications |
| `k6 run load-test.js` | Run load tests (requires k6) |

---

## 👨‍💻 Development & Contributing

### Adding a new language
Edit `public/js/i18n.js` — add your translations to the three language objects.

### Adding a new route
1. Create a route file in `routes/`
2. Apply necessary middleware (auth, csrf, upload)
3. Register it in `app.js`
4. Create the EJS template in `views/`

### Scheduler notes
- Reminder hour is controlled by `REMINDER_HOUR` in `.env`
- The scheduler runs immediately on boot, then once daily at the configured hour

---

## 📱 Responsive Design

The UI is fully responsive using **pure CSS media queries** — no server-side device detection.

### Desktop
| Feature | Behavior |
|---------|----------|
| **Sidebar** | Fixed panel with brand logo, user card, nav indicator |
| **Stats grid** | 4 columns |
| **Topbar** | Search, language pills, notification bell, hamburger toggle |
| **Content** | Flows beside the fixed sidebar |

### Mobile
| Feature | Behavior |
|---------|----------|
| **Sidebar** | Slide-in drawer with dark overlay — hamburger opens, overlay closes |
| **Stats grid** | Collapses to compact cards |
| **Topbar** | Sticky with backdrop blur |
| **Tables** | Horizontal scroll with reduced padding |
| **Forms** | Single-column layout, full-width buttons, 16px inputs (no iOS zoom) |
| **Charts** | Resize to fit viewport |

### RTL Parity
All layouts mirror correctly for Arabic (`dir="rtl"`): sidebar, drawer direction, spacing, and toasts flip to match.

### Theme
Dark by default with a gold accent (`--gold: #d4a44a`); light mode via a CSS class and persisted in `localStorage`.

---

## 📝 Changelog

### v5.8.1 (August 2026)

**Bug Fixes (PDF Exports):**
- Fixed PDF export crash (`ReferenceError: pageNum is not defined` in `routes/exportRoutes.js`) that produced a broken/empty response (`ERR_INVALID_RESPONSE`) on all PDF downloads
- Centralized page numbering across `drawFirstPageHeader`, `drawTable`, and footers via a single module-level counter — headers, table pages, and footers now show consistent page numbers
- PDF exports verified working: `/export/cheques/pdf`, `/export/clients/pdf`, `/export/staffs/pdf`, `/export/staff/:id/pdf`, `/export/service/:serviceId/pdf`

**Other:**
- Added `scripts/seed-big.js` (`npm run seed:big`) — bulk seed generator (300 clients / 60 staff / 800 cheques / 1200 services / ~1800+ payments) with `--keep` (append) and `--clear` (wipe) modes

---

### v5.7.0 (August 2026)

**New Features:**
- **Flag-based language selector** — replaced text pills (FR/AR/EN) with a custom dropdown showing country flags (France 🇫🇷, Morocco 🇲🇦, UK 🇬🇧) via `flagcdn.com` CDN
- **Full i18n coverage** — all views now translate dynamically to FR/AR/EN (169 keys), including forms, empty states, filter options, confirm dialogs, tooltips, pagination, and error pages
- **Client-side date formatting** — dates now render in the correct locale (`fr-FR`, `ar-MA`, `en-GB`) based on selected language via `Intl.DateTimeFormat`, instead of always showing French
- **Light mode as default** — new users start in light theme; dark mode available via topbar toggle
- **Background blur filter** — post-login slideshow uses `blur(14px)` for a polished depth effect

**Bug Fixes:**
- Fixed dates always displaying in French regardless of language selection
- Fixed light theme contrast issues — darkened gold colors (`--gold: #a67c1e`) for readability on white backgrounds
- Fixed `data-i18n-placeholder` not syncing to custom-select dropdown labels
- Fixed theme toggle button not visible (was missing from DOM)
- Added `languagechange` custom event so custom-select dropdowns re-sync option text on language switch

**Improvements:**
- Custom-select dropdowns now re-render translated option text when language changes
- Confirm dialogs (delete cheque/client/staff/service) now show in the active language
- Tooltip titles on icon buttons (Edit, Delete, Mark Paid, etc.) now translate
- `window.translations` exposed for confirm-dialog handler
- Rate-limit and register pages now support language switching

---

## 📄 License

**All Rights Reserved © Ouaoua Decor** — This system is proprietary software for Ouaoua Décor store. Redistribution or resale is not permitted without written authorization.

---

> **Version:** 5.8.1
> **Last updated:** August 2026
> **Built with ❤️ using Node.js + MongoDB**

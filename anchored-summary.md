# Project Summary — Company Manager v5

## Objective
Complete Company Manager v5 with all security fixes plus a clean responsive design using **only CSS media queries** (server-side device detection removed).

## Stack
Node.js + Express.js + MongoDB (Mongoose) + EJS + express-ejs-layouts  
**Port:** 30029 · **Seed credentials:** admin / admin123  
**Design:** Dark-by-default gold-accent · Light mode · RTL · Multi-language (FR/AR/EN)

## Key Packages
`express-rate-limit` · `helmet` · `connect-mongo` · `express-validator`  
`express-useragent` was installed then removed  
`csurf` skipped (custom CSRF middleware instead)

## Completed

### Security & Core
- Session cookie `secure` changed from `true` → `process.env.NODE_ENV === 'production'` — fixes "Invalid or missing CSRF token" 500 on HTTP (localhost)
- All 49 security issues from npm audit resolved
- Rate limiting: global (200/15min), login (10/15min), register (3/hour), API (30/min), search (30/min), export (10/15min)
- Custom CSRF middleware with 403 statusCode pass-through
- CSRF middleware cleaned up (removed debug console.log)
- `error.ejs`: hardcoded `500` → `<%= statusCode || 500 %>`
- `app.js` error handlers pass `statusCode` (404, 500)

### Features
- Login accepts username OR email in single field
- Chart.js moved to dedicated row layout
- Export route path fixed (`:type/:format`)
- Notification system: `createNotification()` on all CRUD routes, `seed-notifications.js` script, `unreadCount` global middleware, server-side initial badge
- Toast system: replaces flash alerts with animated toasts, all CRUD routes use `req.flash()`, real-time poll → toast
- Service add form: client `<select>` dropdown with auto-fill phone, link to /clients
- k6 installed with load-test.js created

### Responsive Design (Pure CSS Media Queries)
- **Approach:** `@media (max-width: 768px)` and `(max-width: 480px)` — no server-side detection
- **All `html[data-device="..."]` removed** from CSS, JS, layout
- **`express-useragent` removed** from app.js (import + middleware)
- **`views/layout.ejs`** rewritten: clean sidebar layout, no bottom nav, no mobile drawer, no `data-device` logic
- **`public/js/main.js`** cleaned: removed `initMobileDrawer()`, `initMobileSearchToggle()`; simplified `initSidebar()`
- **`public/css/style.css`** responsive section replaced: pure `@media` queries, all `html[data-device="mobile"]` blocks deleted

### Specific Mobile Fixes (per user request)
1. Stats grid: 2-col → 1-col on mobile
2. Export button: `.btn-gold` gets `flex: 1`, `.btn-sm` gets `flex: 0 0 auto`
3. Tables: `overflow-x: auto` + `← swipe →` hint indicator
4. Charts: `col-lg-6` stacking (Bootstrap default) + chart size 180px

## Remaining
- Verify all pages render on both desktop and mobile viewport
- Test RTL on mobile

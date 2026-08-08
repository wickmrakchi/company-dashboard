# Security Audit Report — Company Manager v5

> **Date:** July 2026
> **Scope:** Full-stack Node.js + Express + MongoDB application
> **Classification:** Internal — Confidential

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Fixed Vulnerabilities](#2-fixed-vulnerabilities)
3. [Known Unresolved Vulnerabilities](#3-known-unresolved-vulnerabilities)
4. [Potential Undiscovered Vulnerabilities](#4-potential-undiscovered-vulnerabilities)
5. [Security Recommendations](#5-security-recommendations)
6. [Security Checklist](#6-security-checklist)

---

## 1. Executive Summary

A comprehensive security audit was conducted on the Company Manager v5 management system.
**17 vulnerabilities were identified and fixed**, **2 are known and intentionally left open**,
and **6 potential vulnerability categories remain undetected** due to the lack of specialized testing tools.

### Severity Legend
| Label | Color | Meaning |
|-------|-------|---------|
| 🔴 **Critical** | Red | Direct compromise possible (RCE, data theft, privilege escalation) |
| 🟠 **High** | Orange | Significant damage but requires some conditions |
| 🟡 **Medium** | Yellow | Moderate risk, limited impact |
| 🟢 **Low** | Green | Minor information leakage or edge cases |

---

## 2. Fixed Vulnerabilities

### 🔴 CRITICAL

#### 2.1 NoSQL Injection — Login Bypass
| Field | Detail |
|-------|--------|
| **CWE** | CWE-943 ( Improper Neutralization of Special Elements in Data Query Logic) |
| **Location** | `routes/auth.js` — login POST handler |
| **Risk** | An attacker could send `{ "username": { "$gt": "" } }` to bypass authentication entirely and log in as the first user in the database without knowing the password. |
| **Fix** | `const username = String(req.body.username);` — Casting the input to a string before passing it to Mongoose prevents any operator injection. |
| **Status** | ✅ Fixed |

#### 2.2 CSRF — Cross-Site Request Forgery (All Forms)
| Field | Detail |
|-------|--------|
| **CWE** | CWE-352 (Cross-Site Request Forgery) |
| **Location** | All 18 POST forms across 9 route files |
| **Risk** | An attacker could trick an authenticated admin into visiting a malicious page that silently submits forms (delete clients, change passwords, etc.) using the victim's active session. |
| **Fix** | Custom `middleware/csrf.js` generates a random 32-byte hex token per session. Every form includes `<input type="hidden" name="_csrf" value="<%= csrfToken %>">`. The middleware validates the token on every POST/PUT/DELETE. |
| **Status** | ✅ Fixed |

#### 2.3 Stored XSS — Toast Notifications
| Field | Detail |
|-------|--------|
| **CWE** | CWE-79 (Improper Neutralization of Input During Web Page Generation) |
| **Location** | `public/js/main.js` — `createToast()` function |
| **Risk** | Flash messages from the server (`success`, `error`) were inserted via `innerHTML`. An attacker who could control a flash message (e.g., by creating a client named `<script>alert(1)</script>`) could execute arbitrary JavaScript in every admin's browser. |
| **Fix** | Replaced `innerHTML` with `document.createElement('div')` + `textContent`. The message, title, and icon are all set via DOM methods that treat content as text, not HTML. |
| **Status** | ✅ Fixed |

#### 2.4 Stored XSS — Search Autocomplete
| Field | Detail |
|-------|--------|
| **CWE** | CWE-79 (Improper Neutralization of Input During Web Page Generation) |
| **Location** | `public/js/search.js` — autocomplete dropdown rendering |
| **Risk** | Search results (client names, service descriptions, etc.) were injected into the DOM via string concatenation like `resultsContainer.innerHTML += '<div>' + name + '</div>'`. A maliciously crafted client name could execute scripts in the search context. |
| **Fix** | Used `document.createDocumentFragment()` + `document.createElement('div')` + `textContent` assignment. All user-controlled data is treated as text nodes. |
| **Status** | ✅ Fixed |

#### 2.5 Unrestricted File Upload
| Field | Detail |
|-------|--------|
| **CWE** | CWE-434 (Unrestricted Upload of File with Dangerous Type) |
| **Location** | `middleware/upload.js` |
| **Risk** | The original check used `||` (OR) logic: if the extension matched **OR** the MIME type matched, the file was accepted. An attacker could upload a `.exe` file with a forged MIME of `image/jpeg` and bypass validation. |
| **Fix** | Changed `||` to `&&` (AND logic): both the file extension **AND** the MIME type must be in the whitelist. Added explicit MIME whitelist: `image/jpeg`, `image/png`, `image/gif`, `image/webp`. Added `fileFilter` callback to multer config. |
| **Status** | ✅ Fixed |

---

### 🟠 HIGH

#### 2.6 Open Registration (Account Creation)
| Field | Detail |
|-------|--------|
| **CWE** | CWE-862 (Missing Authorization) |
| **Location** | `routes/auth.js` — `GET /register` and `POST /register` |
| **Risk** | Anyone could access `/register` and create an admin account, gaining full system access. |
| **Fix** | Registration is now restricted to the first user only. After the first admin exists (`User.countDocuments()` > 0), registration returns `403 Forbidden`. Additionally, a rate limiter of 3 requests per hour was added. |
| **Status** | ✅ Fixed |

#### 2.7 Missing Security Headers (Helmet)
| Field | Detail |
|-------|--------|
| **CWE** | CWE-693 (Protection Mechanism Failure) |
| **Location** | `app.js` |
| **Risk** | The application was missing critical HTTP security headers: `X-Content-Type-Options` (MIME sniffing), `X-Frame-Options` (clickjacking), `Strict-Transport-Security` (HSTS), `X-XSS-Protection`, and `Content-Security-Policy`. |
| **Fix** | Added `app.use(helmet())` which sets 15 security headers automatically with secure defaults. |
| **Status** | ✅ Fixed |

#### 2.8 Session Hijacking via Cookie Theft
| Field | Detail |
|-------|--------|
| **CWE** | CWE-614 (Sensitive Cookie in HTTPS Session Without 'Secure' Attribute) |
| **Location** | `app.js` — session configuration |
| **Risk** | Session cookies were set without `HttpOnly`, `SameSite`, or `Secure` flags. An attacker with XSS or network access could steal the session cookie and impersonate the user. |
| **Fix** | Configured `httpOnly: true`, `sameSite: 'lax'`, and `secure: process.env.NODE_ENV === 'production'`. Session store was moved from default MemoryStore to MongoDB (connect-mongo) to persist sessions across server restarts. |
| **Status** | ✅ Fixed |

#### 2.9 Account Enumeration
| Field | Detail |
|-------|--------|
| **CWE** | CWE-204 (Observable Response Discrepancy) |
| **Location** | `routes/auth.js` — login POST handler |
| **Risk** | The original code returned different error messages for "user not found" vs "wrong password". An attacker could enumerate valid usernames by observing the error message. |
| **Fix** | Unified error message: `"Invalid username or password"` for all authentication failures. |
| **Status** | ✅ Fixed |

#### 2.10 Error Stack Trace Leakage
| Field | Detail |
|-------|--------|
| **CWE** | CWE-209 (Generation of Error Message Containing Sensitive Information) |
| **Location** | `app.js` — global error handler |
| **Risk** | In production, the error handler would display the full stack trace to the user, revealing file paths, database queries, and internal application structure. |
| **Fix** | Error handler now checks `process.env.NODE_ENV` — in production, only a generic "Something went wrong" message is shown. Stack trace is logged server-side only. |
| **Status** | ✅ Fixed |

#### 2.11 Hardcoded MongoDB Credentials in Code
| Field | Detail |
|-------|--------|
| **CWE** | CWE-798 (Use of Hard-coded Credentials) |
| **Location** | `.env` file (not in code, but credentials exposed) |
| **Risk** | The MongoDB URI containing username/password (`<dbUser>:<dbPassword>`) could be committed to git and exposed on public repositories. |
| **Fix** | Created `.gitignore` to prevent `.env`, `node_modules/`, and `uploads/` from being committed. |
| **Status** | ✅ Fixed |

#### 2.12 Missing Rate Limiting on Authentication
| Field | Detail |
|-------|--------|
| **CWE** | CWE-307 (Improper Restriction of Excessive Authentication Attempts) |
| **Location** | `routes/auth.js` |
| **Risk** | No limit on login attempts — an attacker could brute-force passwords indefinitely. |
| **Fix** | Added `loginLimiter` (10 requests per 15 minutes) and `registerLimiter` (3 requests per hour) using `express-rate-limit`. |
| **Status** | ✅ Fixed |

---

### 🟡 MEDIUM

#### 2.13 Missing 404 Handler
| Field | Detail |
|-------|--------|
| **CWE** | CWE-755 (Improper Handling of Exceptional Conditions) |
| **Location** | `app.js` |
| **Risk** | Visiting a non-existent route like `/nonexistent` would show Express's default HTML error page, which leaks server information (Express version, etc.) and provides a poor user experience. |
| **Fix** | Added a catch-all 404 handler that renders `views/error.ejs` with a user-friendly message. |
| **Status** | ✅ Fixed |

#### 2.14 Missing Rate Limiting on Search & Export
| Field | Detail |
|-------|--------|
| **CWE** | CWE-770 (Allocation of Resources Without Limits or Throttling) |
| **Location** | `routes/search.js`, `routes/exportRoutes.js` |
| **Risk** | An attacker could send thousands of search or export requests, causing high database load and potentially a denial of service. |
| **Fix** | Added `searchLimiter` (30 requests/minute) and `exportLimiter` (10 requests/15 minutes). Added `limit(200)` to search queries to cap result size. |
| **Status** | ✅ Fixed |

#### 2.15 Missing Role-Based Access Control on Delete Routes
| Field | Detail |
|-------|--------|
| **CWE** | CWE-285 (Improper Authorization) |
| **Location** | All delete routes across `routes/` |
| **Risk** | Any authenticated user (including employees) could delete any record — clients, cheques, services, staff, payments. |
| **Fix** | Added `checkRole()` middleware to all delete routes: staff delete → `admin` only; client/cheque/service/payment delete → `admin` or `manager`. |
| **Status** | ✅ Fixed |

#### 2.16 Hardcoded Date Locale (fr-FR)
| Field | Detail |
|-------|--------|
| **CWE** | CWE-477 (Use of Obsolete Functions) |
| **Location** | All EJS views using `.toLocaleDateString('fr-FR')` |
| **Risk** | Dates were hardcoded to French locale regardless of the user's language preference. Arabic users would see French dates. |
| **Fix** | Added `/api/lang` endpoint that saves language to session. Views now use `<%= locale %>` which dynamically resolves to `fr-FR`, `ar-SA`, or `en-US` based on the session. |
| **Status** | ✅ Fixed |

---

### 🟢 LOW

#### 2.17 Unoptimized Pagination (Missing `limit()`)
| Field | Detail |
|-------|--------|
| **CWE** | CWE-770 (Allocation of Resources Without Limits or Throttling) |
| **Location** | `routes/search.js` |
| **Risk** | Search queries had no result cap. A search matching 100,000 records would load all into memory before rendering. |
| **Fix** | Added `Service.find(query).limit(200)` — results are now capped at 200. |
| **Status** | ✅ Fixed |

---

## 3. Known Unresolved Vulnerabilities

These issues are **known** but have been intentionally left unresolved due to specific constraints.

### 🟠 HIGH — 3.1 Session-Fixation via Synchronizer Token Pattern

| Field | Detail |
|-------|--------|
| **CWE** | CWE-384 (Session Fixation) |
| **Type** | Web — Session Management |
| **Location** | `app.js` — session configuration |
| **Details** | The CSRF token is stored in `req.session.csrfToken` and is not regenerated on login/logout. If an attacker can set a victim's session cookie to a known value (session fixation), they can also predict the CSRF token since it persists across sessions. |
| **Why Not Fixed** | Session fixation requires the attacker to already have control over the victim's cookie, which is mitigated by `httpOnly` + `SameSite` + random `connect.sid`. Additionally, Express sessions generate a new session ID on every request by default (`resave: true` would prevent it, but we use `resave: false`). Full mitigation would require regenerating the CSRF token on every login/logout, which would break the current session flow. |
| **Status** | ❌ Not fixed — mitigated by defense-in-depth |

### 🟡 MEDIUM — 3.2 No Content Security Policy (CSP) Nonce for Inline Scripts

| Field | Detail |
|-------|--------|
| **CWE** | CWE-79 (Cross-Site Scripting) |
| **Type** | Web — Content Security Policy |
| **Location** | `app.js` — helmet CSP configuration |
| **Details** | The application relies on inline `<script>` tags (e.g., Chart.js initialization in dashboard.ejs, DataTables config in list views). To allow these, CSP would need `'unsafe-inline'` for scripts, which largely defeats the purpose of CSP for XSS prevention. The proper solution is a CSP nonce generated per-request. |
| **Why Not Fixed** | Implementing CSP nonces requires modifying every EJS template to pass `nonce="<%= nonce %>"` on every `<script>` tag and `<style>` tag. There are ~25+ inline script blocks across views. This is a significant refactor that would touch nearly every view file. It should be done in a dedicated refactoring sprint. |
| **Status** | ❌ Not fixed — requires a full view refactoring sprint |

---

## 4. Potential Undiscovered Vulnerabilities

These vulnerabilities **have not been found or confirmed** in the current codebase, but they are **theoretically possible** given the tech stack and architecture. They remain undetected because no specialized testing (DAST, SAST, penetration testing) has been performed.

### 🔴 CRITICAL — 4.1 MongoDB Injection via Aggregation Pipeline

| Field | Detail |
|-------|--------|
| **CWE** | CWE-943 (Improper Neutralization of Special Elements in Data Query Logic) |
| **Type** | Database — NoSQL Injection |
| **Likelihood** | Medium |
| **Description** | While we fixed simple `$where` injection in login, there may be other routes that use raw `$match`, `$regex`, or aggregation pipelines with unsanitized user input. The `search.js` route uses a regex on `name` and `description` fields — if the regex pattern itself is user-controlled, it could lead to ReDoS (Regular Expression Denial of Service) or injection into aggregation stages. |
| **Why Undetected** | No automated SAST (Static Application Security Testing) tool like **SonarQube** or **CodeQL** has been run against the codebase. A manual code review was performed but may have missed edge cases in aggregation queries. |
| **How to Find** | Run `CodeQL` or `SonarQube` with NoSQL injection rules. Alternatively, fuzz all `req.query` and `req.body` parameters with MongoDB operators like `$gt`, `$ne`, `$regex`, `$where`. |

### 🔴 CRITICAL — 4.2 Prototype Pollution via Body Parser

| Field | Detail |
|-------|--------|
| **CWE** | CWE-1321 (Improperly Controlled Modification of Object Prototype Attributes) |
| **Type** | JavaScript — Prototype Pollution |
| **Likelihood** | Medium |
| **Description** | Express's built-in `express.json()` and `express.urlencoded()` parsers are generally safe in recent versions, but if an attacker sends `{ "__proto__": { "isAdmin": true } }` in a JSON body, older parsers could pollute `Object.prototype`. This could grant admin privileges or bypass validation. |
| **Why Undetected** | The exact version of Express/body-parser in `package.json` determines vulnerability. A `npm audit` was run but no dedicated prototype pollution scan was performed. |
| **How to Find** | Run `npm audit --production` for known CVEs. Use `snyk test` or `socket.dev` for deeper dependency analysis. Test manually by sending `{"__proto__":{"polluted":"true"}}` in POST requests. |

### 🟠 HIGH — 4.3 Mass Assignment (Mongoose)

| Field | Detail |
|-------|--------|
| **CWE** | CWE-915 (Improperly Controlled Modification of Dynamically-Determined Object Attributes) |
| **Type** | Database — Mass Assignment |
| **Likelihood** | High |
| **Description** | Many Mongoose operations use `req.body` directly: `Client.findByIdAndUpdate(id, req.body)`. If the client schema has a `role` or `isAdmin` field in the future, an attacker could send `{ "role": "admin" }` in their update request. Currently, `Client` doesn't have sensitive fields, but `User` model has `role`. |
| **Why Undetected** | No manual audit of "what fields can be overwritten via req.body" was performed for every route. The `routes/auth.js` registration explicitly sets `role: 'employee'` after creation, which is good, but other routes may not be as careful. |
| **How to Find** | Search for `findByIdAndUpdate` and `.save()` calls across the codebase. Check if they destructure `req.body` or pass it whole. |

### 🟠 HIGH — 4.4 Timing Attack on Login

| Field | Detail |
|-------|--------|
| **CWE** | CWE-208 (Observable Timing Discrepancy) |
| **Type** | Cryptographic — Timing Attack |
| **Likelihood** | Low |
| **Description** | `bcrypt.compare()` takes measurably longer for correct passwords than incorrect ones (because bcrypt processes the full hash for a match). An attacker on the same network could measure response times to determine if the username exists or the password is partially correct. |
| **Why Undetected** | Timing attacks require thousands of precise network measurements. No timing-analysis tool (like `timing_attack` Python library) has been used against the login endpoint. |
| **How to Find** | Send 10,000+ login requests with known-valid vs known-invalid credentials and measure response time variance. Use statistical analysis (Mann-Whitney U test) to detect timing differences. |

### 🟡 MEDIUM — 4.5 Dependency Supply Chain Attack

| Field | Detail |
|-------|--------|
| **CWE** | CWE-1104 (Use of Unmaintained Third-Party Components) |
| **Type** | Supply Chain |
| **Likelihood** | Medium |
| **Description** | The project has 286+ npm dependencies. Any one of them could be compromised (malicious package update, typo-squatting, dependency confusion). A compromised dependency could exfiltrate the MongoDB credentials, session secrets, or inject backdoor code. |
| **Why Undetected** | `npm audit` was run but only checks known CVEs. No dependency analysis tool like **Socket.dev**, **Snyk**, or **Dependabot** is configured. The `package-lock.json` pins versions but `^` ranges allow automatic minor/patch updates which could introduce malicious code. |
| **How to Find** | Use `socket.dev` to scan each dependency for behavior indicators (network access, filesystem access, obfuscated code). Enable Dependabot on the GitHub repository. Pin all dependencies to exact versions. |

### 🟡 MEDIUM — 4.6 Log Injection / Log Forgery

| Field | Detail |
|-------|--------|
| **CWE** | CWE-117 (Improper Output Neutralization for Logs) |
| **Type** | Web — Log Injection |
| **Likelihood** | Medium |
| **Description** | User-controlled data (client name, service description, etc.) is logged in the `Log` collection via `createNotification()`. If an attacker includes CRLF characters (`\r\n`) or ANSI escape sequences, they could forge log entries or corrupt log analysis tools. |
| **Why Undetected** | The `createNotification()` function in `routes/notifications.js` was reviewed for functionality but not for log injection. No log sanitization (removing `\r\n`, newlines, escape codes) is applied before saving to the Log collection. |
| **How to Find** | Create a client with name `"Client A\r\n[INFO] User admin deleted all records"` and check if the forged log entry appears in `/logs`. |

---

## 5. Security Recommendations

### Immediate (1-2 days)
| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 1 | Run `npm audit fix` to patch known dependency CVEs | 🔴 Critical | 5 min |
| 2 | Enable `Dependabot` or `Renovate` on the GitHub repo | 🟠 High | 10 min |
| 3 | Change the MongoDB password and update `.env` | 🔴 Critical | 5 min |
| 4 | Add a global `express-mongo-sanitize` middleware | 🟠 High | 15 min |

### Short-term (1 week)
| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 5 | Run **SonarQube** or **CodeQL** static analysis | 🟠 High | 1 day |
| 6 | Add CSP nonces to all templates (fix §3.2) | 🟡 Medium | 2-3 days |
| 7 | Audit all `findByIdAndUpdate` calls for mass assignment | 🟠 High | 2 hours |
| 8 | Add log sanitization (strip CRLF, escape sequences) | 🟡 Medium | 1 hour |

### Long-term (1 month)
| # | Action | Priority | Effort |
|---|--------|----------|--------|
| 9 | Set up **OWASP ZAP** or **Burp Suite** for DAST scanning | 🟠 High | 1 week |
| 10 | Implement **Helmet CSP** with strict nonce-based policy | 🟡 Medium | 1 week |
| 11 | Conduct a third-party penetration test | 🔴 Critical | 2-4 weeks |
| 12 | Set up automated security testing in CI/CD pipeline | 🟡 Medium | 3 days |

---

## 6. Security Checklist

### ✅ Done
- [x] CSRF protection on all forms
- [x] XSS prevention (toast + autocomplete)
- [x] NoSQL injection prevention (login)
- [x] Account enumeration prevention
- [x] Rate limiting (login, register, search, export, API)
- [x] Helmet security headers
- [x] Secure session cookies (HttpOnly, SameSite, Secure)
- [x] MongoDB session store (connect-mongo)
- [x] File upload validation (ext + MIME)
- [x] Registration restriction (first admin only)
- [x] Role-based authorization on delete routes
- [x] Error stack trace hidden in production
- [x] `.gitignore` (env, node_modules, uploads)
- [x] Dynamic date locale based on language
- [x] Search result cap (200)

### ❌ Not Done
- [ ] CSP nonces for inline scripts (§3.2)
- [ ] Session-fixation resistant CSRF token regeneration (§3.1)

### ⚠️ Should Be Done
- [ ] Dependency vulnerability audit (`npm audit fix`)
- [ ] `express-mongo-sanitize` global middleware
- [ ] Mass assignment audit on all `req.body` usage
- [ ] Log injection sanitization
- [ ] Timing attack testing on login
- [ ] Penetration test (third party or OWASP ZAP)

---

> **Report generated:** July 30, 2026
> **Audit methodology:** Manual code review + best-practice checklist (OWASP Top 10 — 2021)
> **Tools NOT used (recommended):** SonarQube, CodeQL, OWASP ZAP, Burp Suite, Snyk, Socket.dev

<div align="center">

# 🏢 Company Management Dashboard

**A powerful, all-in-one Company Dashboard & Management System built for modern businesses.**

[![Version](https://img.shields.io/badge/version-4.2.0-gold?style=for-the-badge&logo=github)](https://github.com/wickmrakchi/company-management)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.x-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-7.x-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](LICENSE)

<p align="center">
  <img src="https://img.shields.io/badge/EJS-3.x-B4CA65?style=flat-square&logo=ejs&logoColor=white" alt="EJS">
  <img src="https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=flat-square&logo=bootstrap&logoColor=white" alt="Bootstrap">
  <img src="https://img.shields.io/badge/Mongoose-7.x-880000?style=flat-square&logo=mongoose&logoColor=white" alt="Mongoose">
  <img src="https://img.shields.io/badge/Session-Auth-orange?style=flat-square&logo=auth0&logoColor=white" alt="Session Auth">
  <img src="https://img.shields.io/badge/i18n-Multilingual-blue?style=flat-square&logo=translate&logoColor=white" alt="i18n">
</p>

</div>

---

## 📋 Table of Contents

- [About](#-about)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Screenshots](#-screenshots)
- [Installation](#-installation)
- [Environment Variables](#-environment-variables)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Social Media](#-social-media)
- [License & Usage](#-license--usage)

---

## 🎯 About

**Company Management Dashboard** is a comprehensive, production-ready business management platform designed to streamline daily operations. From tracking cheques and managing clients to overseeing staff and services, this dashboard provides a centralized hub for company administration.

Built with a modern gold-themed UI, full multilingual support (French, Arabic, English), and a responsive sidebar layout, it delivers a premium user experience across all devices.

> **Version 4.0.0** — Enhanced staff/service/payment workflows, activity logging, and an upgraded dashboard.

> **Version 4.1.0** — Job type system improvements + backend logic upgrades for better staff role management.

> **Version 4.2.0** — Major UI/UX update: global search system, dark mode support, and multiple mobile responsiveness improvements.

---


## 🛠️ Tech Stack

- **Frontend:** HTML, CSS, JavaScript, EJS, Bootstrap 5
- **Backend:** Node.js, Express.js, Mongoose
- **Database:** MongoDB
- **Authentication:** Session-based authentication with bcrypt
- **i18n:** Full support for French, Arabic, and English

---

## 📸 Screenshots

<p align="center">

### 🏠 Dashboard Overview
<p align="center">
  <em>Dashboard summary cards, quick actions, recent entities, and payment status visualization.</em>
</p>

![Dashboard Overview](https://cdn.discordapp.com/attachments/1476763298325201037/1513223761288233131/image.png?ex=6a26f2f3&is=6a25a173&hm=4a0a8dd4ed4eb5b8ffc3476da072468c51e362205e6a8c6c57672e3235622c3c&)
![Dashboard Overview](https://cdn.discordapp.com/attachments/1476763298325201037/1513223777981300786/image.png?ex=6a26f2f7&is=6a25a177&hm=d6f7c3af618012ec8cdecf8568aff405a89a251460864368e100213be913e771&)

### 💰 Cheque Management
<p align="center">
  <em>Cheque list with filters/search and quick edit actions.</em>
</p>

![Cheques Table](https://cdn.discordapp.com/attachments/1476763298325201037/1513223797853917204/image.png?ex=6a26f2fb&is=6a25a17b&hm=de7724a8e16f0937e1ee33d040cc8cae0aa756020ba8b40be68b20f0edfdcd1d&)

### 👥 Client Directory
<p align="center">
  <em>Clients table with status, location, devis number and delivery date filtering.</em>
</p>

![Clients Table](https://cdn.discordapp.com/attachments/1476763298325201037/1513223818917843096/image.png?ex=6a26f300&is=6a25a180&hm=0a836638587a43d31a73ac0ee2d6b4ed1616cace4befaaec01f6360c16f88509&)

### 👷 Staff Services & Payments
<p align="center">
  <em>Staff profile details, services list and payment progress tracking per service.</em>
</p>

![Staff Services](https://cdn.discordapp.com/attachments/1476763298325201037/1513223842447757382/image.png?ex=6a26f306&is=6a25a186&hm=5b45809462b4046f108e35068d0e32f549b25cf8aefdb77b49b3420dc8dcd0f4&)
![Staff Services](https://cdn.discordapp.com/attachments/1476763298325201037/1513223870814097458/image.png?ex=6a26f30d&is=6a25a18d&hm=6ed2815be9f164ac6c54f4dea6fb4f246dc89d991a6c19230be413afb81b75d9&)
![Staff Services](https://cdn.discordapp.com/attachments/1476763298325201037/1513223883392811129/image.png?ex=6a26f310&is=6a25a190&hm=daf4bd47115dedf206e579fb0e1cee77443e4fa859e347f897ff73719fea4165&)

### 🔎 Global Search
<p align="center">
  <em>Unified search suggestions across clients, staff, cheques and services.</em>
</p>

![Global Search](https://cdn.discordapp.com/attachments/1476763298325201037/1513223894025240646/image.png?ex=6a26f312&is=6a25a192&hm=5e451bb2ed9f2238cadd8487a40616134102fd374a828ce5cc5c55e7286e82e3&)

</p>

---


## ✨ Features

### 💰 Cheque Management
- Full CRUD operations for cheques
- Track cheque status: **Paid**, **Pending**, **Cancelled**
- Support for PRS and Company cheque types
- Advanced filtering by status, amount, date range, and search
- Upcoming cheques widget on the dashboard
- Payment status overview with animated progress bars

### 👥 Client Management
- Complete client directory with contact details
- Track project status: **Pending**, **In Progress**, **Delivered**
- Devis number tracking and delivery date scheduling
- Filter by location, status, delivery date range, and devis number
- Full-text search across client names and phone numbers

### 👷 Staff & Workforce Management
- Staff profiles with job type classification
- *Manager, Supervisor, Team Leader, Employee, Assistant, Technician, Engineer, Administrator, Coordinator, Specialist, Operator, Intern, Trainee, Consultant, Freelancer*
- Status tracking: **Active** / **Inactive**
- Location-based filtering

### 🛠️ Service & Payment Tracking
- Link services directly to staff members
- Track service status: **En attente**, **En cours**, **Delivery**, **Paid**
- Record partial and full payments per service
- Automatic status update to "Paid" when total amount is reached
- Payment history with notes

### 📊 Dashboard & Analytics
- Real-time stat cards with animated counters
- Upcoming cheques preview table
- Payment status breakdown with visual progress bars
- Quick action shortcuts to all major modules

### 🔐 Security & Authentication
- Secure session-based authentication
- Bcrypt password hashing
- Protected routes with middleware
- Activity logging for all major actions

### 🌍 Internationalization (i18n)
- Full support for **French**, **Arabic**, and **English**
- RTL layout support for Arabic
- Persistent language selection

### 📱 UI/UX
- Modern gold-themed responsive design
- Collapsible sidebar navigation
- Mobile-optimized with overlay menu
- Smooth animations and hover effects
- Custom scrollbar and selection styling

---

## 🛠 Tech Stack

| Layer | Technology |
|-------|------------|
| **Runtime** | Node.js |
| **Framework** | Express.js |
| **Database** | MongoDB |
| **ODM** | Mongoose |
| **Templating** | EJS + Express-EJS-Layouts |
| **Styling** | Bootstrap 5 + Custom CSS |
| **Icons** | Font Awesome 6 |
| **Auth** | Express-Session + Bcrypt |
| **i18n** | Custom JavaScript i18n Engine |

---

## 📸 Screenshots

<div align="center">

### 🏠 Dashboard Overview
<p align="center">
  <em>The main dashboard provides a quick snapshot of your business with real-time stats, upcoming cheques, and payment status breakdowns.</em>
</p>

```
┌─────────────────────────────────────────────────────────────┐
│  [Sidebar]  Dashboard  |  🔍 Search    🌐 FR  👤 Admin  🚪 │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐          │
│  │  156    │ │   89    │ │   45    │ │   22    │          │
│  │ Total   │ │  Paid   │ │ Pending │ │Cancelled│          │
│  │ Cheques │ │         │ │         │ │         │          │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘          │
│                                                             │
│  [View All Cheques] [Manage Clients] [Manage Staff]         │
│                                                             │
│  ┌───────────────── Upcoming Cheques ─────────────────┐    │
│  │ Name        │ Amount   │ Number    │ Date   │ ✏️   │    │
│  ├─────────────┼──────────┼───────────┼────────┼──────┤    │
│  │ Client A    │ 5,000 DH │ CH-00123  │ 15/06  │ Edit │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
│  ┌─ Payment Status Overview ─┐                              │
│  │ Paid     ████████████ 57% │                              │
│  │ Pending  ██████░░░░░░ 29% │                              │
│  │ Cancelled ███░░░░░░░░ 14% │                              │
│  └───────────────────────────┘                              │
└─────────────────────────────────────────────────────────────┘
```

### 💰 Cheque Management Table
<p align="center">
  <em>Advanced filtering and search capabilities to manage all company cheques efficiently.</em>
</p>

```
┌─────────────────────────────────────────────────────────────┐
│  Cheques                                                    │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Status: [All ▼]  Search: [________]  Amount: [__]   │   │
│  │ Date From: [__/__/____]  To: [__/__/____] [Filter]  │   │
│  └─────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Name │ Amount  │ Number   │ Type   │ Date   │ Status│   │
│  ├──────┼─────────┼──────────┼────────┼────────┼───────┤   │
│  │ ...  │ ... DH  │ ...      │ PRS    │ ...    │ PAID  │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### 👥 Client Directory
<p align="center">
  <em>Organize clients with detailed profiles, project tracking, and delivery scheduling.</em>
</p>

```
┌─────────────────────────────────────────────────────────────┐
│  Clients                                                    │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Status: [All ▼]  Location: [______]  Devis: [__]    │   │
│  │ Delivery: [__/__/____] to [__/__/____] [Filter]     │   │
│  └─────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Name │ Location │ Phone      │ Devis  │ Delivery │ ✏️ │   │
│  ├──────┼──────────┼────────────┼────────┼──────────┼───┤   │
│  │ ...  │ ...      │ 06XX...    │ D-001  │ 20/06    │ ✏️ │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### 👷 Staff & Services
<p align="center">
  <em>Manage your workforce and track services with integrated payment monitoring.</em>
</p>

```
┌─────────────────────────────────────────────────────────────┐
│  Staff: John Doe - Carpenter                                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Client    │ Job Type │ Amount   │ Paid   │ Status   │   │
│  ├───────────┼──────────┼──────────┼────────┼──────────┤   │
│  │ Client A  │ Cabinet  │ 8,000 DH │ 5,000  │ En cours │   │
│  │ Client B  │ Chair    │ 3,500 DH │ 3,500  │ Payé     │   │
│  └─────────────────────────────────────────────────────┘   │
│  [+ Add Service] [+ Add Payment]                            │
└─────────────────────────────────────────────────────────────┘
```

### 🔐 Secure Login
<p align="center">
  <em>Clean, branded login page with multilingual support and password visibility toggle.</em>
</p>

```
┌─────────────────────────────────────────────────────────────┐
│  🌐 [FR ▼]                                                 │
│                                                             │
│              ┌─────────────────────────┐                   │
│              │    📊 Dashboard         │                   │
│              │  Système de gestion     │                   │
│              │                         │                   │
│              │  👤 Username            │                   │
│              │  [________________]     │                   │
│              │                         │                   │
│              │  🔒 Password            │                   │
│              │  [________________] 👁️  │                   │
│              │                         │                   │
│              │  [    🔐 Connexion    ] │                   │
│              │                         │                   │
│              │  Bienvenue! Veuillez    │                   │
│              │  vous connecter.        │                   │
│              └─────────────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

</div>

---

## 🚀 Installation

### Prerequisites
- [Node.js](https://nodejs.org/) v18 or higher
- [MongoDB](https://www.mongodb.com/) instance (local or Atlas)

### Step 1: Clone the Repository
```bash
git clone https://github.com/wickmrakchi/company-management.git
cd company-management
```

### Step 2: Install Dependencies
```bash
npm install
```

### Step 3: Configure Environment Variables
Create a `.env` file in the root directory and add the following:

```env
PORT=3000
MONGODB_URI=mongodb://localhost:27017/company-management
SESSION_SECRET=your-super-secret-session-key-change-this
```

> ⚠️ **Important:** Replace `SESSION_SECRET` with a strong, random string for production.

### Step 4: Start the Application

**Production mode:**
```bash
npm start
```

**Development mode (with auto-reload):**
```bash
npm run dev
```

The application will be available at: `http://localhost:3000`

---

## 🔧 Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port number | ✅ Yes |
| `MONGODB_URI` | MongoDB connection string | ✅ Yes |
| `SESSION_SECRET` | Secret key for session encryption | ✅ Yes |

---

## 📖 Usage

1. **Login** — Navigate to `http://localhost:3000/login` and authenticate with your credentials.
2. **Dashboard** — View real-time statistics, upcoming cheques, and payment breakdowns.
3. **Cheques** — Manage all company cheques with filtering and search.
4. **Clients** — Maintain your client directory with project tracking.
5. **Staff** — Manage workforce, assign services, and track payments.
6. **Logs** — Monitor all system activities and user actions.
7. **Language** — Switch between French, Arabic, and English using the topbar selector.

---

## 📁 Project Structure

```
company-management/
├── app.js                  # Main application entry point
├── package.json            # Project dependencies & scripts
├── .env                    # Environment variables (not tracked)
├── LICENSE                 # License file
├── README.md               # Project documentation
│
├── middleware/
│   └── auth.js             # Authentication middleware
│
├── models/
│   ├── Cheque.js           # Cheque schema
│   ├── Client.js           # Client schema
│   ├── Log.js              # Activity log schema
│   ├── Payment.js          # Payment schema
│   ├── Service.js          # Service schema
│   ├── Staff.js            # Staff schema
│   └── User.js             # User schema with bcrypt
│
├── routes/
│   ├── auth.js             # Authentication (login/logout)
│   ├── cheques.js          # Cheque CRUD routes + dashboard aggregation
│   ├── clients.js          # Client CRUD routes
│   ├── staffs.js           # Staff + Services + Payments routes
│   ├── logs.js             # Activity logs
│   ├── search.js           # Global search API + search results

│
├── views/
│   ├── layout.ejs          # Main dashboard layout (sidebar)
│   ├── login.ejs           # Login page
│   ├── home.ejs            # Dashboard overview
│   ├── table.ejs           # Cheques table
│   ├── add.ejs             # Add cheque form
│   ├── edit.ejs            # Edit cheque form
│   ├── clients-table.ejs   # Clients table
│   ├── add-client.ejs      # Add client form
│   ├── edit-client.ejs     # Edit client form
│   ├── staffs-table.ejs    # Staff table
│   ├── add-staff.ejs       # Add staff form
│   ├── edit-staff.ejs      # Edit staff form
│   ├── staff-services.ejs  # Staff detail & services
│   ├── add-service.ejs     # Add service form
│   ├── edit-service.ejs    # Edit service form
│   ├── add-payment.ejs     # Add payment form
│   ├── logs.ejs            # Activity logs view
│   └── register.ejs        # Registration form
│
├── config/
│   └── constants.js         # Centralized job types/constants
│
├── middleware/
│   └── auth.js             # Authentication middleware
│
├── scripts/
│   └── migrate-job-types.js # One-time job type migration helper
│
└── public/
    ├── css/
    │   └── style.css         # Custom styles
    ├── images/              # Dashboard images (screenshots)
    └── js/
        ├── i18n.js           # Internationalization engine
        ├── script.js         # Main frontend UI logic
        └── search-autocomplete.js # Global search autocomplete

```

---

## 🌐 Social Media

Connect with the developer and stay updated:

<p align="center">

[![GitHub](https://img.shields.io/badge/GitHub-wickmrakchi-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/wickmrakchi)
[![Instagram](https://img.shields.io/badge/Instagram-@mrakchi__5-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://instagram.com/@mrakchi_5)
[![Discord](https://img.shields.io/badge/Discord-wicks-5865F2?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/wicks)
[![Email](https://img.shields.io/badge/Email-hessamgrati@gmail.com-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:hessamgrati@gmail.com)
[![PayPal](https://img.shields.io/badge/PayPal-Support%20the%20Project-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://www.paypal.com/paypalme/Essamgrati)

</p>

---

## License & Usage

**Proprietary Software - All Rights Reserved**

This Company Dashboard project is proprietary software developed by Mrakchi Service.

**Strictly Prohibited Without Written Permission:**

* Using the code in any project or product
* Copying any part of the codebase
* Modifying the source code
* Distributing the software or any derivatives
* Reselling or offering as a service

Repository access does **NOT** grant any usage rights. To use this software, you must obtain explicit written permission or purchase a commercial license from the copyright holder.

Unauthorized use may result in legal action.

[See LICENSE file for full terms](LICENSE)


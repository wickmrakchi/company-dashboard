# Company Dashboard

A comprehensive web dashboard for managing company operations, including clients, services, staff, payments, cheques, and logs. Built with Node.js, Express, EJS templates, and vanilla JavaScript.

## Features
- **User Authentication**: Login/Register system
- **Client Management**: Add, edit, view clients (table view)
- **Service Management**: Add, edit, assign services to staff
- **Staff Management**: Add, edit staff and view assignments
- **Payment Tracking**: Record payments
- **Cheque Management**: Handle cheques
- **Logs**: Activity logging
- **Internationalization**: Multi-language support (i18n)
- **Responsive UI**: Styled with CSS and interactive JS

## Tech Stack
- Backend: Node.js, Express.js
- Database: MongoDB (via Mongoose models)
- Frontend: EJS, Vanilla JS, CSS
- Authentication: Custom middleware

## Quick Start

### Prerequisites
- Node.js (v18+)
- MongoDB (local or Atlas)

### Installation
1. Clone the repository:
   ```
   git clone https://github.com/wickmrakchi/company-dashboard.git
   cd company-dashboard
   ```
2. Install dependencies:
   ```
   npm install
   ```
3. Set up environment variables (create `.env`):
   ```
   PORT=3000
   MONGODB_URI=your_mongodb_connection_string
   JWT_SECRET=your_jwt_secret
   ```
4. Start the server:
   ```
   npm start
   ```
5. Open http://localhost:3000 in your browser

## Usage
- Register a new account or login
- Manage clients, staff, services via the dashboard
- View logs and payments

## Project Structure
```
.
├── middleware/         # Auth middleware
├── models/             # Mongoose schemas
├── routes/             # API routes
├── views/              # EJS templates
├── public/             # Static assets (CSS/JS)
├── app.js              # Main Express app
└── package.json
```

## Contact & Social Media
- **GitHub**: [github.com](https://github.com/wickmrakchi)
- **Instagram**: [instagram](https://instagram.com/@mrakchi_5)
- **Discord**: [discord](https://discord.gg/wicks)
- **Email**: [hessamgrati@gmail.com](mailto:hessamgrati@gmail.com)

## License
MIT License - feel free to use and modify.

---

⭐ Star this repo if you found it useful!


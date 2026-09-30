# 🚀 Last Minute

A full-stack last-minute booking platform built with a **microservices architecture**: separate backend services for authentication, listings, bookings and payments, plus a React frontend.

---

# 📌 Project Overview

Last Minute allows users to:

* Register and log in securely
* Create and manage listings (sellers)
* Search listings and check availability
* Book available listings and cancel bookings
* Verify guest identity by uploading ID documents
* Make payments
* Save listings to a wishlist
* View buyer and seller dashboards

The project simulates a real-world booking platform similar to Airbnb or last-minute rental systems.

---

# 🏗️ Architecture

| Service         | Port | Description                                              |
| --------------- | ---- | -------------------------------------------------------- |
| Auth Service    | 4001 | Registration, login, JWT auth, user profile              |
| Listing Service | 4002 | Create, search and manage listings, availability, stats  |
| Booking Service | 4003 | Bookings, pricing, eligibility, document verification    |
| Payment Service | 4004 | Payment creation and status (Stripe, with a mock mode)   |
| PostgreSQL      | 5432 | Main relational database                                 |

The services talk to each other over HTTP (for example, the Listing and Payment services call the Booking service). Each service exposes a `/health` endpoint.

---

# 🛠️ Tech Stack

**Frontend:** React 19, Vite, Tailwind CSS, React Router, Zustand, Axios, Recharts, Framer Motion

**Backend:** Node.js, Express.js, PostgreSQL, JWT authentication, bcrypt

**Verification:** Sharp (image processing), Tesseract OCR

**Payments:** Stripe (runs in mock mode when no key is set)

**DevOps:** Docker, Docker Compose, Nginx

---

# 📁 Folder Structure

```bash
TheLastMinuteApp/
│── frontend/
│   └── web/
│── services/
│   ├── auth-service/
│   ├── listing-service/
│   ├── booking-service/
│   └── payment-service/
│── db/
│   └── init.sql
│── docker-compose.yml
│── .env.example
│── README.md
```

---

# 🔐 Features

## Authentication
* Register and log in
* JWT token generation and protected routes
* Password hashing with bcrypt
* Buyer and seller roles

## Listings
* Create and manage listings
* Search and categories
* Availability management
* Seller dashboard stats

## Bookings
* Availability checking and price calculation
* Eligibility checks
* Book and cancel
* Transaction-based booking logic
* Wishlist
* Buyer dashboard stats

## Guest Verification
* Document upload
* Image processing with Sharp
* OCR text extraction with Tesseract

## Payments
* Payment creation and status tracking tied to bookings
* Stripe integration with mock mode for development

## Frontend
* Responsive UI
* Authentication flow
* Listings, booking and payment pages
* Buyer and seller dashboards

---

# ⚙️ Local Setup

## 1. Clone the repository

```bash
git clone https://github.com/YashiUpmanyu25/TheLastMinuteApp.git
cd TheLastMinuteApp
```

## 2. Run the backend and database

`docker-compose.yml` already contains the local environment variables, so you can start everything with:

```bash
docker compose up --build
```

This starts PostgreSQL (and loads `db/init.sql`) and the four backend services on ports 4001-4004.

## 3. Run the frontend

```bash
cd frontend/web
npm install
npm run dev
```

The frontend reads service URLs from these optional variables (defaults point to localhost):

```env
VITE_AUTH_SERVICE_URL=http://localhost:4001
VITE_LISTING_SERVICE_URL=http://localhost:4002
VITE_BOOKING_SERVICE_URL=http://localhost:4003
VITE_PAYMENT_SERVICE_URL=http://localhost:4004
```

## Environment variables (backend)

| Variable | Used by | Purpose |
| -------- | ------- | ------- |
| `PORT` | all services | Port to listen on |
| `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME` | all services | PostgreSQL connection |
| `DB_SSL` | all services | Set to `true` for managed databases that require SSL |
| `JWT_SECRET` | all services | Must be the same on every service. **Use a long random value in production.** |
| `JWT_REFRESH_SECRET` | auth | Refresh token signing |
| `FRONTEND_URL` | backends | Allowed CORS origin |
| `BOOKING_SERVICE_URL` | listing, payment | Booking service address |
| `AUTH_SERVICE_URL` | payment | Auth service address |
| `STRIPE_SECRET_KEY` | payment | Optional; mock mode without it |

See `.env.example` for a template.

---

# 🌐 API Endpoints

## Auth Service

```http
POST /auth/register
POST /auth/login
GET  /auth/me
```

Also: `/auth/profile`, `/auth/verification-status`, `/auth/check-verification`

## Listings

```http
POST   /api/listings
GET    /api/listings/search
GET    /api/listings/:id
GET    /api/listings/my/listings
```

Also: `/api/listings/categories`, `/api/listings/:id/availability`, `/api/listings/seller/listings`, `/api/listings/seller/stats`

## Bookings

```http
POST  /bookings
GET   /bookings/my-bookings
PATCH /bookings/:id/cancel
```

Also: `/bookings/check-availability`, `/bookings/calculate-price`, `/bookings/check-eligibility`, `/bookings/verify-guests`, `/bookings/upload-document`, `/bookings/documents`, `/bookings/wishlist`, `/bookings/buyer/stats`, `/bookings/:id/payment-status`

## Payments

```http
POST /api/payments/create
```

Also: `/api/payments/:bookingId/status`

---

# 🚀 Deployment

Each service and the frontend has its own `Dockerfile`, so the project can be deployed on Render, Railway, AWS or any Docker host. Set the environment variables above, run `db/init.sql` once against your database, and point the frontend's `VITE_*` variables at the deployed service URLs.

---

# 📈 Future Improvements

* Redis caching and idempotency keys for booking requests
* Row-level locking for stronger double-booking protection
* Notifications (email / SMS)
* Reviews and ratings
* Admin dashboard
* API gateway
* CI/CD pipeline
* Automated tests

---

# 📬 Contact

**Yashi Upmanyu** · [LinkedIn](https://www.linkedin.com/in/yashi-upmanyu/) · [GitHub](https://github.com/YashiUpmanyu25)

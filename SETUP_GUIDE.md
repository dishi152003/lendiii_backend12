# Lendii FWP Backend - Quick Start Guide

## ✅ What's Been Created

A complete production-ready Express.js backend with TypeScript implementing all 30 API endpoints from your Postman collection.

### 📁 Project Structure Created:

```
landi_backend/
├── src/
│   ├── config/               # Supabase, Stripe, Email configuration
│   ├── controllers/          # 4 controllers (Auth, Listing, Order, Payment)
│   ├── middleware/           # Auth, Error handling, Rate limiting, Validation
│   ├── routes/              # API route definitions  
│   ├── services/            # Business logic layer
│   ├── types/               # TypeScript definitions
│   ├── utils/               # Validation schemas
│   ├── database/            # SQL migrations & seed data
│   └── server.ts            # Main application
├── package.json             # Dependencies installed ✅
├── tsconfig.json           # TypeScript configuration
├── .env                     # Environment variables
├── .env.example            # Environment template
└── README.md               # Complete documentation
```

## 🚀 Next Steps

### 1. Setup Supabase Database

You need to create a Supabase project and run the database migrations:

#### Option A: Using Supabase Dashboard (Recommended)

1. Go to https://supabase.com and create a free project
2. Wait for project to be ready (~2 minutes)
3. Go to **SQL Editor** in the left menu
4. Create a new query and paste contents of `landi_backend/src/database/schema.sql`
5. Click **Run** to create all tables
6. Create another new query and paste contents of `landi_backend/src/database/seed.sql`
7. Click **Run** to insert sample data

#### Option B: Using Supabase CLI

```bash
# Install Supabase CLI
npm install -g supabase

# Login to Supabase
supabase login

# Link to your project
supabase link --project-ref your-project-ref

# Run migrations
supabase db push
```

### 2. Get Your Supabase Credentials

1. In Supabase Dashboard, go to **Project Settings** → **API**
2. Copy these values to your `.env` file:

```env
NEXT_PUBLIC_SUPABASE_URL=https://xxxxx.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

3. Also copy the **Database URL** from **Project Settings** → **Database**:

```env
DATABASE_URL=postgresql://postgres:[YOUR-PASSWORD]@db.xxxxx.supabase.co:5432/postgres
```

### 3. Get Stripe API Keys

1. Go to https://stripe.com and sign up/login
2. Go to **Developers** → **API keys**
3. Copy your keys to `.env`:

```env
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...
```

4. For webhook secret, set up webhook in Stripe Dashboard:
   - Go to **Developers** → **Webhooks**
   - Click **Add endpoint**
   - URL: `http://localhost:3001/api/v1/webhooks/stripe` (for local testing)
   - Events: `checkout.session.completed`, `payment_intent.succeeded`, `payment_intent.payment_failed`
   - Copy the **Signing secret** to `.env` as `STRIPE_WEBHOOK_SECRET`

### 4. Get Resend API Key (Optional for emails)

1. Go to https://resend.com and sign up
2. Go to **API Keys** and create a new key
3. Add to `.env`:

```env
RESEND_API_KEY=re_...
FROM_EMAIL=noreply@yourdomain.com
```

### 5. Start the Server

```bash
cd landi_backend

# Development mode (with auto-restart)
npm run dev

# Production mode
npm run build
npm start
```

Server will start on **http://localhost:3001**

### 6. Test the API

```bash
# Health check
curl http://localhost:3001/api/v1/health

# Get system enums
curl http://localhost:3001/api/v1/meta/enums

# Request magic link
curl -X POST http://localhost:3001/api/v1/auth/magic-link \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@lendii.test"}'
```

## 📊 Database Schema Created

The following tables will be created in Supabase:

- ✅ **users** - User accounts (admin, borrower roles)
- ✅ **listings** - Fashion items for rent  
- ✅ **orders** - Rental orders
- ✅ **sessions** - Authentication sessions
- ✅ **payments** - Stripe payment records
- ✅ **magic_link_tokens** - Temporary auth tokens

**Sample Data Included:**
- 2 test users (admin@lendii.test, laura@example.com)
- 5 sample listings (dresses, jackets, etc.)
- 1 sample order

## 🎯 All 30 API Endpoints Implemented

### ✅ Authentication (4)
- POST `/api/v1/auth/magic-link`
- GET `/api/v1/auth/callback`
- GET `/api/v1/auth/session`
- POST `/api/v1/auth/logout`

### ✅ Catalog (3)
- GET `/api/v1/listings`
- GET `/api/v1/listings/:id`
- GET `/api/v1/listings/:id/availability`

### ✅ Admin Listings (5)
- POST `/api/v1/admin/listings`
- PATCH `/api/v1/admin/listings/:id`
- POST `/api/v1/admin/listings/:id/publish`
- POST `/api/v1/admin/listings/:id/unpublish`
- DELETE `/api/v1/admin/listings/:id`

### ✅ Booking & Checkout (4)
- POST `/api/v1/bookings/validate`
- POST `/api/v1/checkout/create-session`
- GET `/api/v1/checkout/success`
- GET `/api/v1/checkout/cancel`

### ✅ Payments (2)
- POST `/api/v1/webhooks/stripe`
- GET `/api/v1/payments/transactions`

### ✅ Orders (8)
- GET `/api/v1/orders`
- GET `/api/v1/orders/:id`
- GET `/api/v1/admin/orders`
- GET `/api/v1/admin/orders/:id`
- PATCH `/api/v1/admin/orders/:id/ship`
- PATCH `/api/v1/admin/orders/:id/complete`
- POST `/api/v1/admin/orders/:id/refund`
- PATCH `/api/v1/admin/orders/:id/notes`

### ✅ Admin Dashboard (2)
- GET `/api/v1/admin/dashboard/stats`
- GET `/api/v1/admin/dashboard/recent-orders`

### ✅ Utilities (2)
- GET `/api/v1/health`
- GET `/api/v1/meta/enums`

## 🔐 Security Features

- ✅ Helmet.js for HTTP headers
- ✅ CORS protection
- ✅ Rate limiting (100 req/15min, 5 req/15min for auth)
- ✅ Input validation with Zod
- ✅ Role-based access control
- ✅ JWT session management
- ✅ SQL injection protection

## 📱 Testing with Postman

Update your existing Postman environment:

```json
{
  "base_url": "http://localhost:3001/api/v1"
}
```

All your existing Postman tests should work once you:
1. Set up Supabase and run migrations
2. Update `.env` with real credentials
3. Start the server

## 🆚 Mock Server vs Production Backend

| Feature | Mock Server | Production Backend |
|---------|-------------|-------------------|
| Data Storage | In-memory | PostgreSQL (Supabase) |
| Authentication | Fake tokens | Real magic links + sessions |
| Payments | Simulated | Real Stripe integration |
| Emails | Console logs | Real emails via Resend |
| Validation | Basic | Comprehensive with Zod |
| Security | Minimal | Production-ready |
| Database | None | Full schema with RLS |

## 📚 Documentation

See `landi_backend/README.md` for complete documentation including:
- Full API reference
- Authentication guide
- Deployment instructions
- Troubleshooting
- Database schema details

## ❓ Common Issues

### "Module not found" errors
```bash
npm install
```

### "Cannot connect to database"
- Verify Supabase credentials in `.env`
- Check Supabase project is running
- Verify IP is allowed in Supabase dashboard

### "Stripe webhook failed"
- Check webhook secret matches Stripe dashboard
- Use Stripe CLI for local testing: `stripe listen --forward-to localhost:3001/api/v1/webhooks/stripe`

## 🎉 You're Ready!

Once you complete steps 1-5 above, your production backend will be fully functional and ready to connect to a frontend application!

**Current mock server:** `http://localhost:3000` (if still running)  
**New production backend:** `http://localhost:3001`

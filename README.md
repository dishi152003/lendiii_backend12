# Lendii FWP - Fashion Rental Platform Backend

Production-ready Express.js backend API for the Lendii Fashion Rental Platform.

## 🚀 Tech Stack

- **Runtime:** Node.js v18+
- **Framework:** Express.js v4
- **Language:** TypeScript
- **Database:** PostgreSQL (Supabase)
- **Authentication:** Magic Link (Passwordless)
- **Payments:** Stripe
- **Email:** Resend
- **ORM:** Supabase Client

## 📋 Features

✅ **30 RESTful API Endpoints**
- Authentication with magic links
- Listing management (CRUD)
- Order processing and tracking
- Stripe payment integration
- Admin dashboard with analytics
- Role-based access control

✅ **Security**
- Helmet.js security headers
- CORS protection
- Rate limiting
- JWT session management
- Input validation with Zod

✅ **Development**
- TypeScript strict mode
- Error handling middleware
- Request logging
- Auto-restart with Nodemon
- Environment-based configuration

## 🏗️ Project Structure

```
landi_backend/
├── src/
│   ├── config/           # Configuration files
│   │   ├── supabase.ts
│   │   ├── stripe.ts
│   │   └── email.ts
│   ├── controllers/      # Request handlers
│   │   ├── authController.ts
│   │   ├── listingController.ts
│   │   ├── orderController.ts
│   │   └── paymentController.ts
│   ├── middleware/       # Express middleware
│   │   ├── auth.ts
│   │   ├── errorHandler.ts
│   │   ├── rateLimiter.ts
│   │   └── validator.ts
│   ├── routes/          # API routes
│   │   ├── authRoutes.ts
│   │   ├── listingRoutes.ts
│   │   ├── orderRoutes.ts
│   │   ├── paymentRoutes.ts
│   │   └── index.ts
│   ├── services/        # Business logic
│   │   ├── authService.ts
│   │   ├── listingService.ts
│   │   ├── orderService.ts
│   │   └── stripeService.ts
│   ├── types/           # TypeScript definitions
│   │   └── index.ts
│   ├── utils/           # Helpers and validators
│   │   └── validators.ts
│   ├── database/        # SQL migrations
│   │   ├── schema.sql
│   │   └── seed.sql
│   └── server.ts        # Application entry point
├── dist/                # Compiled JavaScript
├── .env.example         # Environment template
├── .gitignore
├── package.json
├── tsconfig.json
├── nodemon.json
└── README.md
```

## 🛠️ Setup Instructions

### 1. Prerequisites

- Node.js v18 or higher
- npm v9 or higher
- Supabase account (https://supabase.com)
- Stripe account (https://stripe.com)
- Resend account (https://resend.com)

### 2. Install Dependencies

```bash
cd landi_backend
npm install
```

### 3. Environment Configuration

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Update the following variables in `.env`:

```env
# Server
PORT=3001
NODE_ENV=development

# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
DATABASE_URL=postgresql://...

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Resend
RESEND_API_KEY=re_...

# JWT
JWT_SECRET=your-super-secret-key-change-in-production
```

### 4. Database Setup

#### Using Supabase Dashboard:

1. Go to your Supabase project
2. Navigate to SQL Editor
3. Run the migration script:

```bash
# Copy contents of src/database/schema.sql and run in SQL Editor
```

4. Run the seed script:

```bash
# Copy contents of src/database/seed.sql and run in SQL Editor
```

### 5. Start Development Server

```bash
npm run dev
```

Server will start on http://localhost:3001

### 6. Build for Production

```bash
npm run build
npm start
```

## 📡 API Endpoints

Base URL: `http://localhost:3001/api/v1`

### Authentication

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/auth/magic-link` | Request magic link | No |
| GET | `/auth/callback` | Verify magic link | No |
| GET | `/auth/session` | Get current session | Yes |
| POST | `/auth/logout` | Logout | Yes |

### Listings

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/listings` | Get all published listings | No |
| GET | `/listings/:id` | Get listing details | No |
| GET | `/listings/:id/availability` | Check availability | No |

### Admin - Listings

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/admin/listings` | Create listing | Admin |
| PATCH | `/admin/listings/:id` | Update listing | Admin |
| POST | `/admin/listings/:id/publish` | Publish listing | Admin |
| POST | `/admin/listings/:id/unpublish` | Unpublish listing | Admin |
| DELETE | `/admin/listings/:id` | Delete listing | Admin |

### Booking & Checkout

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/bookings/validate` | Validate booking dates | No |
| POST | `/checkout/create-session` | Create Stripe checkout | Yes |
| GET | `/checkout/success` | Checkout success callback | No |
| GET | `/checkout/cancel` | Checkout cancel callback | No |

### Orders

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/orders` | Get user orders | Yes |
| GET | `/orders/:id` | Get order details | Yes |

### Admin - Orders

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/admin/orders` | Get all orders | Admin |
| GET | `/admin/orders/:id` | Get order details | Admin |
| PATCH | `/admin/orders/:id/ship` | Mark as shipped | Admin |
| PATCH | `/admin/orders/:id/complete` | Complete order | Admin |
| POST | `/admin/orders/:id/refund` | Refund order | Admin |
| PATCH | `/admin/orders/:id/notes` | Update notes | Admin |

### Admin - Dashboard

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/admin/dashboard/stats` | Get KPIs | Admin |
| GET | `/admin/dashboard/recent-orders` | Recent orders | Admin |

### Payments

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| POST | `/webhooks/stripe` | Stripe webhook | No |
| GET | `/payments/transactions` | Get transactions | Yes |

### Utilities

| Method | Endpoint | Description | Auth |
|--------|----------|-------------|------|
| GET | `/health` | Health check | No |
| GET | `/meta/enums` | System enums | No |

## 🔐 Authentication

The API uses magic link authentication with Bearer tokens.

### Getting a Token:

1. **Request magic link:**
```bash
POST /api/v1/auth/magic-link
{
  "email": "admin@lendii.test"
}
```

2. **Verify magic link (response contains token):**
```bash
GET /api/v1/auth/callback?token=YOUR_MAGIC_TOKEN
```

3. **Use the access_token in headers:**
```bash
Authorization: Bearer YOUR_ACCESS_TOKEN
```

## 👥 Test Users

| Email | Role | Use Case |
|-------|------|----------|
| `admin@lendii.test` | Admin | Full access |
| `laura@example.com` | Borrower | Regular user |

## 🧪 Testing

### Using cURL:

```bash
# Health check
curl http://localhost:3001/api/v1/health

# Get listings
curl http://localhost:3001/api/v1/listings

# Request magic link
curl -X POST http://localhost:3001/api/v1/auth/magic-link \
  -H "Content-Type: application/json" \
  -d '{"email": "admin@lendii.test"}'
```

### Using Postman:

Import the Postman collection from the parent directory:
- `Lendii_FWP_API_Collection.postman_collection 1.json`
- `Lendii_Test_Environment.postman_environment.json`

Update environment `base_url` to `http://localhost:3001/api/v1`

## 🚀 Deployment

### Environment Variables for Production:

```env
NODE_ENV=production
PORT=3001
CORS_ORIGIN=https://yourdomain.com
FRONTEND_URL=https://yourdomain.com
```

### Build:

```bash
npm run build
```

### Deploy to:
- **Vercel**: Use serverless functions
- **Railway**: Direct deployment
- **Render**: Web service
- **AWS/Azure**: Container or VM

## 📊 Database Schema

### Core Tables:
- `users` - User accounts
- `listings` - Fashion items for rent
- `orders` - Rental orders
- `sessions` - Auth sessions
- `payments` - Payment records
- `magic_link_tokens` - Temporary auth tokens

See `src/database/schema.sql` for full schema.

## 🛡️ Security

- Helmet.js for HTTP headers
- CORS middleware
- Rate limiting (100 req/15min)
- Input validation with Zod
- SQL injection protection
- XSS protection

## 📝 Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Compile TypeScript |
| `npm start` | Start production server |
| `npm run lint` | Run ESLint |
| `npm test` | Run tests (Jest) |

## 🐛 Troubleshooting

### Server won't start:
- Check `.env` file exists and has all required variables
- Verify Supabase connection
- Check port 3001 is available

### Database errors:
- Verify Supabase credentials
- Run schema.sql migration
- Check RLS policies

### Stripe errors:
- Verify webhook secret matches Stripe dashboard
- Check webhook endpoint is accessible
- Validate API keys

## 📚 Additional Resources

- [Express Documentation](https://expressjs.com/)
- [TypeScript Documentation](https://www.typescriptlang.org/)
- [Supabase Documentation](https://supabase.com/docs)
- [Stripe API Documentation](https://stripe.com/docs/api)
- [Resend Documentation](https://resend.com/docs)

## 📧 Support

For issues or questions, check the project documentation or create an issue in the repository.

---

**Built with ❤️ for Lendii Fashion Rental Platform**

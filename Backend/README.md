# Ethara AI Task Management Backend

Production-grade REST API for an AI operations task management platform. Built with Express.js, Prisma ORM, PostgreSQL, JWT authentication, and role-based access control.

## Tech Stack

| Layer          | Technology       |
|----------------|------------------|
| Runtime        | Node.js          |
| Framework      | Express.js       |
| Database       | PostgreSQL       |
| ORM            | Prisma           |
| Auth           | JWT + bcrypt     |
| Validation     | Zod              |
| Security       | Helmet + CORS    |

## Project Structure

```
src/
├── config/          # Environment validation
├── controllers/     # Request handlers
├── middleware/       # Auth, validation, error handling
├── prisma/          # Prisma client singleton
├── routes/          # Route definitions
├── services/        # Business logic layer
├── utils/           # Response helpers, JWT, password
└── validators/      # Zod schemas
prisma/
├── schema.prisma    # Database schema
└── seed.js          # Seed data
```

## Setup

### Prerequisites

- Node.js 18+
- PostgreSQL 14+

### Installation

```bash
# Install dependencies
npm install

# Copy environment file and configure
cp .env.example .env
# Edit .env with your database URL and JWT secret

# Generate Prisma client
npx prisma generate

# Run database migrations
npx prisma migrate dev --name init

# Seed the database (optional)
npm run prisma:seed

# Start development server
npm run dev
```

### Environment Variables

| Variable       | Description                    | Default               |
|----------------|--------------------------------|-----------------------|
| `DATABASE_URL` | PostgreSQL connection string   | —                     |
| `JWT_SECRET`   | Secret key for JWT signing     | —                     |
| `JWT_EXPIRES_IN` | Token expiration duration    | `7d`                  |
| `PORT`         | Server port                    | `5000`                |
| `NODE_ENV`     | Environment mode               | `development`         |
| `CORS_ORIGIN`  | Allowed CORS origin            | `http://localhost:3000` |

## API Endpoints

### Authentication

| Method | Endpoint             | Auth | Description       |
|--------|----------------------|------|-------------------|
| POST   | `/api/auth/register` | No   | Register new user |
| POST   | `/api/auth/login`    | No   | Login             |
| GET    | `/api/auth/me`       | Yes  | Get profile       |

### Projects

| Method | Endpoint             | Auth | Role  | Description        |
|--------|----------------------|------|-------|--------------------|
| POST   | `/api/projects`      | Yes  | ADMIN | Create project     |
| GET    | `/api/projects`      | Yes  | Any   | List projects      |
| GET    | `/api/projects/:id`  | Yes  | Any   | Get project        |
| PUT    | `/api/projects/:id`  | Yes  | ADMIN | Update project     |
| DELETE | `/api/projects/:id`  | Yes  | ADMIN | Delete project     |

### Tasks

| Method | Endpoint          | Auth | Role  | Description    |
|--------|-------------------|------|-------|----------------|
| POST   | `/api/tasks`      | Yes  | ADMIN | Create task    |
| GET    | `/api/tasks`      | Yes  | Any   | List tasks     |
| GET    | `/api/tasks/:id`  | Yes  | Any   | Get task       |
| PUT    | `/api/tasks/:id`  | Yes  | Any   | Update task    |
| DELETE | `/api/tasks/:id`  | Yes  | ADMIN | Delete task    |

**Query filters for GET /api/tasks:** `?status=TODO&priority=HIGH&projectId=<uuid>`

### Dashboard (Admin Only)

| Method | Endpoint                  | Description               |
|--------|---------------------------|---------------------------|
| GET    | `/api/dashboard/stats`    | Aggregate statistics      |
| GET    | `/api/dashboard/overdue`  | Overdue tasks list        |
| GET    | `/api/dashboard/activity` | Recent 20 tasks           |

## Roles & Permissions

| Action               | ADMIN | MEMBER |
|----------------------|-------|--------|
| Create projects      | ✅    | ❌     |
| View all projects    | ✅    | Own    |
| Create/assign tasks  | ✅    | ❌     |
| Update any task      | ✅    | ❌     |
| Update own task status | ✅  | ✅     |
| View dashboard       | ✅    | ❌     |

## Seed Credentials

| Role   | Email              | Password       |
|--------|--------------------|----------------|
| ADMIN  | admin@ethara.com   | admin123456    |
| MEMBER | member@ethara.com  | member123456   |

## Railway Deployment

1. Push code to a GitHub repository
2. Connect the repo to [Railway](https://railway.app)
3. Add a PostgreSQL service in Railway
4. Set environment variables in the Railway dashboard
5. Railway will auto-detect Node.js and run `npm start`

The `start` script runs `node src/server.js` for production.

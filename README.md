# MiniDesk — Helpdesk & Ticket Management System

MiniDesk is a full-stack web application for managing support tickets and organizing work in shared spaces. It provides a React interface backed by a Node.js/Express API, with MySQL for persistent data and Redis available for supporting services.

> **Project status:** This README describes the structure and configuration visible in the repository. Check the current code and environment configuration before deploying; local setup and optional integrations may require additional configuration.

## Features

- **Authentication:** Sign up, log in, refresh-token and logout endpoints.
- **Google sign-in:** Google OAuth integration is included in the codebase and requires a configured Google client ID.
- **Ticket management:** Create, view, update and delete tickets.
- **Ticket collaboration:** Comments, tags and ticket status-history components are present in the backend.
- **Spaces:** The frontend includes pages for listing, creating, viewing and editing spaces, plus invitation-acceptance routes.
- **Dashboard:** A dashboard page and dashboard API routes are included.
- **Protected pages:** Authenticated routes are protected; the users page is restricted to the `admin` role in the frontend.
- **Input and API protections:** The backend uses Helmet, CORS, request-body parsing and a centralized error-handling layer. Authentication routes are configured to use a rate limiter.

## Tech Stack

| Layer | Technologies |
|---|---|
| Frontend | React, Vite, React Router |
| Backend | Node.js, Express.js (ES modules) |
| Database | MySQL, Sequelize ORM |
| Authentication | JWT access/refresh tokens, bcrypt, Google OAuth |
| Supporting services | Redis; BullMQ is included as a dependency |
| Validation / security | Zod, Helmet, CORS, Express rate limiting |

## Repository Structure

```text
MiniDesk/
├── Minidesk-ticketing_system-Backend/
│   ├── src/
│   │   ├── config/          # Database, Redis and Google configuration
│   │   ├── controllers/     # Request handlers
│   │   ├── middleware/      # Authentication and role checks
│   │   ├── models/          # Sequelize models
│   │   ├── repositories/    # Data-access layer
│   │   ├── routes/          # API routes
│   │   ├── services/        # Business logic
│   │   ├── utils/           # Shared utilities
│   │   └── validators/      # Request validation
│   └── package.json
└── Minidesk-ticketing_system-Frontend/
    ├── src/
    │   ├── api/              # Frontend API helpers
    │   ├── components/       # Reusable UI components
    │   ├── pages/            # Application pages
    │   └── style/            # CSS styles
    └── package.json
```

## Prerequisites

Install the following before running MiniDesk locally:

- Node.js and npm
- MySQL server
- Redis server (the backend initializes a Redis connection during startup)
- A Google OAuth client ID if you want to use Google sign-in

## Getting Started

### 1. Configure the backend

Open a terminal in the backend folder:

```bash
cd Minidesk-ticketing_system-Backend
npm install
```

Create a `.env` file in this folder. Add your own local values; do not commit this file.

```dotenv
PORT=5000
NODE_ENV=development

DB_HOST=localhost
DB_PORT=3306
DB_NAME=minidesk
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password

REDIS_HOST=localhost
REDIS_PORT=6379

JWT_ACCESS_SECRET=replace_with_a_long_random_secret
JWT_REFRESH_SECRET=replace_with_another_long_random_secret
JWT_ACCESS_EXPIRES=15m
JWT_REFRESH_EXPIRES=7d
JWT_SECRET=replace_with_a_long_random_secret

GOOGLE_CLIENT_ID=your_google_oauth_client_id
```

Some code paths may also use `EMAIL_IV` and `EMAIL_SECRET_KEY` for email-related encryption. Configure these only with values appropriate to the implementation and never publish real secrets. Optional variables and exact formats should be verified against the code you are using.

Create the MySQL database named in `DB_NAME` and make sure MySQL and Redis are running.

Start the backend in development mode:

```bash
npm run start:dev
```

Or start it normally:

```bash
npm start
```

The backend defaults to port `5000`. Its root endpoint is `http://localhost:5000/`.

### 2. Configure the frontend

Open a second terminal from the project root:

```bash
cd Minidesk-ticketing_system-Frontend
npm install
npm run dev
```

Vite will print the local development URL in the terminal (commonly `http://localhost:5173`). The current authentication API helper points to `http://localhost:5000`, so run the backend on that port unless you update the frontend API configuration.

Build the frontend for production with:

```bash
npm run build
```

Run the configured lint command with:

```bash
npm run lint
```

## API Overview

The Express app mounts API routes under `/api`, including these route groups:

| Route prefix | Purpose |
|---|---|
| `/api/auth` | Sign up, log in, refresh token and logout |
| `/api/users` | User-related operations |
| `/api/tickets` | Ticket operations and related ticket routes |
| `/api/dashboard` | Dashboard data |
| `/api/oauth` | Google OAuth endpoint |
| `/api/tags` | Tag operations |
| `/api/spaces` | Space-related operations |

Refer to the route files in `Minidesk-ticketing_system-Backend/src/routes/` for the current endpoints, HTTP methods and authentication requirements.

## Security Notes

- Never commit `.env` files, access tokens, refresh tokens, passwords, OAuth credentials or production connection strings.
- Use strong, unique secrets for JWT configuration.
- Restrict CORS to trusted frontend origins before production deployment.
- Use HTTPS and production-grade configuration when deploying.
- Review authorization on the backend for every protected operation; frontend route protection alone is not sufficient.

## Contributing

1. Create a branch for your change.
2. Keep frontend and backend changes focused and test them locally.
3. Do not include secrets or generated dependency folders in commits.
4. Open a pull request with a clear summary and testing notes.

## License

No project-level license is specified here. Add a license file if you intend to grant others permission to use, modify or distribute this project.

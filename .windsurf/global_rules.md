# Global Rules for Better-T-Stack Project

## Project Overview

This project was created with [Better-T-Stack](https://github.com/AmanVarshney01/create-better-t-stack), a modern TypeScript stack that combines Next.js, Hono, TRPC, and more.

## Core Technologies

- **TypeScript** - For type safety and improved developer experience
- **Next.js** - Full-stack React framework
- **TailwindCSS** - Utility-first CSS for rapid UI development
- **shadcn/ui** - Reusable UI components
- **Hono** - Lightweight, performant server framework
- **tRPC** - End-to-end type-safe APIs
- **Bun** - Runtime environment
- **Drizzle** - TypeScript-first ORM
- **PostgreSQL** - Database engine
- **Authentication** - Email & password authentication with Better Auth
- **Turborepo** - Optimized monorepo build system
- **Biome** - Linting and formatting

## Project Structure

```
better-t-stack-starter/
├── apps/
│   ├── web/         # Frontend application (Next.js)
│   │   ├── src/     # Source code
│   │   │   ├── app/       # Next.js app router
│   │   │   ├── components/# UI components
│   │   │   └── lib/       # Utilities and helpers
│   └── server/      # Backend API (Hono, TRPC)
│       ├── src/
│       │   ├── db/        # Database schemas and configuration
│       │   ├── lib/       # Core utilities and configuration
│       │   └── routers/   # tRPC API routes and handlers
```

## Development Workflow

### Setup

1. Install dependencies:
   ```bash
   bun install
   ```

2. Database Setup:
   - Ensure PostgreSQL is set up
   - Update `apps/server/.env` with connection details
   - Apply schema: `bun db:push`

3. Start development servers:
   ```bash
   bun dev
   ```
   - Web: [http://localhost:3001](http://localhost:3001)
   - API: [http://localhost:3000](http://localhost:3000)

### Available Scripts

- `bun dev`: Start all applications in development mode
- `bun build`: Build all applications
- `bun dev:web`: Start only the web application
- `bun dev:server`: Start only the server
- `bun check-types`: Check TypeScript types across all apps
- `bun db:push`: Push schema changes to database
- `bun db:studio`: Open database studio UI
- `bun check`: Run Biome formatting and linting

## Coding Standards

### General

- Use TypeScript for all code
- Follow the Biome linting and formatting rules
- Ensure type safety across the entire application

### Backend (Server)

- Group related functionality in dedicated router files
- Use `publicProcedure` for unauthenticated endpoints
- Use `protectedProcedure` for authenticated endpoints
- Keep database schema definitions in the `db` directory
- Place shared utilities and configurations in the `lib` directory

### Frontend (Web)

- Use Next.js App Router for routing
- Implement UI components with TailwindCSS and shadcn/ui
- Keep client-side tRPC setup in the `lib` directory
- Follow component-based architecture

## Authentication

- Implementation: Using `better-auth` with Drizzle adapter
- Routes: Mounted at `/api/auth/**`
- Session handling: Sessions are retrieved and made available in the tRPC context

## Database

- Use Drizzle ORM for database operations
- Keep schema definitions type-safe and well-documented
- Use migrations for schema changes when appropriate

---

For more detailed information, refer to the project's [README.md](../README.md).

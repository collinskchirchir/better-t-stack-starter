# Global Rules for Better-T-Stack Project

## Project Overview

This project was created with [Better-T-Stack](https://github.com/AmanVarshney01/create-better-t-stack), a modern TypeScript stack that combines Next.js, Hono, TRPC, and more.

## General Code Style & Formatting
- Follow the Biome formatting and linting rules.
- Use PascalCase for React component file names (e.g., TodoItem.tsx).
- Prefer named exports for components.
- Use descriptive variable names with auxiliary verbs (e.g., isLoading, hasError).

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

## Key Directories

### Backend (`apps/server`)
- `src/db`: Store all database schema definitions and migrations
- `src/lib`: Place core utilities, auth setup, and tRPC configuration
- `src/routers`: Organize tRPC routes by feature/domain

### Frontend (`apps/web`)
- `src/app`: Follow Next.js App Router conventions for page routing
- `src/components`: Group components by feature or UI category
- `src/lib`: Store API clients, hooks, and utility functions

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

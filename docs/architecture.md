# Smart Calendar Repository Overview

This repository contains the full codebase for **Smart Calendar**, a mobility-aware calendar system designed to turn a user’s Google Calendar into a realistic, trustworthy schedule. The repo is structured to clearly separate frontend application code, backend infrastructure, documentation, and automation.

---

## High-Level Structure

The repository is organized into four main areas:

- Web application (Next.js)
- Infrastructure (AWS)
- Documentation
- Automation and configuration

This structure is designed to scale from Phase 0 (foundation) through later feature-heavy phases without major refactors.

---

## `.github/`

### `.github/workflows/ci.yml`
Defines continuous integration workflows.

Responsibilities:
- Run type checking and linting on pull requests
- Catch broken builds before merging
- Enforce basic code quality standards

This ensures the main branch always stays deployable.

---

## `app/` (Next.js Application)

This directory contains the entire web application and is deployed to Vercel.

### `app/public/`
Static assets served directly by Next.js.

Examples:
- Favicons
- Logos
- Static images
- robots.txt or sitemap.xml in later phases

---

### `app/src/`

All runtime source code lives here. This keeps the project root clean and separates application logic from configuration files.

---

### `app/src/app/` (Next.js App Router)

This folder defines all routes, layouts, and API endpoints.

#### Route Groups

Route groups are used for organization and shared behavior without affecting URLs.

##### `(auth)/`
Authentication-related routes.

- `login/page.tsx`  
  URL: `/login`  
  Handles user sign-in and OAuth initiation.

- `logout/page.tsx`  
  URL: `/logout`  
  Handles session termination and redirects.

##### `(protected)/`
Authenticated user routes.

- `dashboard/page.tsx`  
  URL: `/dashboard`  
  Main application view showing calendar data and mobility insights.

- `settings/page.tsx`  
  URL: `/settings`  
  User preferences, integrations, and account controls.

---

#### API Routes

##### `api/auth/`
Server-side authentication logic.

Responsibilities:
- OAuth callbacks
- Token refresh
- Secure session handling

##### `api/health/route.ts`
URL: `/api/health`  
Simple health check endpoint used to verify deployments and uptime.

---

#### Core App Files

- `layout.tsx`  
  Root layout for the entire app. Defines shared HTML structure, providers, and global UI.

- `page.tsx`  
  Landing page at `/`. Often redirects authenticated users to the dashboard.

- `globals.css`  
  Global styles and Tailwind base configuration.

---

## `app/src/components/`

Reusable UI components used across pages.

### `components/ui/`
shadcn/ui components and design system primitives.

### `components/auth/`
Authentication-related UI elements.
Examples:
- Login buttons
- User avatar and account menu

### `components/calendar/`
Calendar-specific UI.
Examples:
- Event lists
- Timeline views
- Conflict and travel buffer indicators

### `components/shared/`
General shared UI components.
Examples:
- Navbar
- Page containers
- Loading states

---

## `app/src/lib/`

Core application logic and integrations. This folder contains the non-UI “brains” of the app.

- `auth.ts`  
  Authentication helpers and session utilities.

- `db.ts`  
  Prisma client initialization and database helpers.

- `google.ts`  
  Google Calendar API client setup and helper functions.

- `env.ts`  
  Environment variable validation to fail fast on misconfiguration.

---

## `app/src/types/`

Shared TypeScript types used across frontend and backend code.

Examples:
- Normalized calendar event types
- API response shapes
- Mobility calculation results

---

## `app/src/utils/`

Small pure helper functions.

Examples:
- Date and time formatting
- String parsing
- Lightweight data transformations

---

## `app/prisma/`

Database schema and migration history managed by Prisma.

- `schema.prisma`  
  Defines database models and relationships.

- `migrations/`  
  Versioned migration files representing schema evolution.

---

## `app/.env.local`

Local-only environment variables.

Contains secrets such as:
- Database connection URL
- Google OAuth credentials

This file is never committed to version control.

---

## `app/next.config.ts`

Next.js configuration file.

Used for:
- Build settings
- Image domain configuration
- Redirects and rewrites

---

## `app/tsconfig.json`

TypeScript compiler configuration for the web application.

---

## `infra/` (Infrastructure)

AWS infrastructure code, fully separated from application logic.

### `infra/cdk/` or `infra/terraform/`
Infrastructure as Code definitions.

Responsibilities:
- S3 buckets
- IAM roles
- Lambda functions
- CloudWatch logs

The exact tool depends on whether CDK or Terraform is chosen.

### `infra/lambda/`
Source code for AWS Lambda functions.

Used for:
- Background jobs
- Event syncing
- File processing tasks in later phases

### `infra/README.md`
Documentation for deploying and managing AWS infrastructure.

---

## `docs/`

Project documentation for maintainability and onboarding.

- `architecture.md`  
  High-level system design and data flow.

- `environments.md`  
  Environment variable definitions and differences between local, preview, and production.

- `decisions.md`  
  Architectural and tooling decisions with reasoning.

---

## Root Files

- `.gitignore`  
  Prevents committing build output, secrets, and dependencies.

- `README.md`  
  Project introduction, setup instructions, and feature overview.

- `package.json`  
  Dependency list and npm scripts for development, build, linting, and tooling.

---

## Design Philosophy

- Clear separation of concerns
- Backend logic never leaks into UI components
- Infrastructure managed independently from application code
- Early optimization for maintainability and scaling

This structure is intentionally production-oriented, even at Phase 0, to avoid costly refactors later.

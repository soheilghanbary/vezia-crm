# Vezia-CRM

A lightweight company management platform for handling invoices, customers and counterparties, letters, contracts, company documents, and day-to-day tasks from a single workspace.

Built for fast, repeatable office work: issue an invoice in under a minute, generate a letter from a template in seconds, and find any company document instantly.

> **Status:** Early development (MVP in progress).

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Project Structure](#project-structure)
- [Core Business Rules](#core-business-rules)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Features

**MVP scope**

- **Invoices** – Purchase and sales invoices with auto-numbering, line items, tax, status workflow (draft, issued, paid, cancelled), and printable PDF output with letterhead, stamp, and signature.
- **Seller profiles** – One or more issuer identities (the invoice owner), with a default profile.
- **Parties** – Customers and suppliers in a single directory, with a detail page showing related invoices, letters, contracts, and the unpaid total.
- **Letters** – Issue outgoing letters from templates, register incoming letters, with automatic sequential numbering.
- **Contracts** – Track contracts per party with status, dates, and the signed file.
- **Company documents** – Store and categorize important files (registration papers, tax declarations, licenses, IDs) with optional expiry dates and quick-access pinning.
- **Tasks and notes** – Simple tasks with due dates and statuses, plus free-form notes optionally linked to a party.
- **Dashboard** – Six KPI cards (monthly sales, monthly purchases, open receivables, overdue invoices, open tasks, expiring contracts and documents) plus latest invoices and letters.

**Localization**

- Right-to-left (RTL) interface with Persian typography
- Jalali (Persian) calendar in the UI, standard timestamps in the database
- Persian/Arabic character normalization for reliable search

---

## Tech Stack

| Layer      | Technology                                                                       |
| ---------- | -------------------------------------------------------------------------------- |
| Framework  | [Next.js](https://nextjs.org/) (App Router) with TypeScript                      |
| UI         | [Tailwind CSS](https://tailwindcss.com/) and [shadcn/ui](https://ui.shadcn.com/) |
| API        | [oRPC](https://orpc.unnoq.com/) with [Zod](https://zod.dev/) validation          |
| Database   | [PostgreSQL](https://www.postgresql.org/)                                        |
| ORM        | [Drizzle ORM](https://orm.drizzle.team/)                                         |
| Auth       | Email and password with secure sessions                                          |
| Files      | Local disk or S3-compatible storage (behind an abstraction layer)                |
| PDF        | HTML-to-PDF rendering                                                            |
| Deployment | Docker                                                                           |

---

## Getting Started

### Prerequisites

- Node.js 20 or later
- pnpm (or your preferred package manager)
- Docker and Docker Compose (for local PostgreSQL)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/vezia-crm.git
cd vezia-crm

# 2. Install dependencies
pnpm install

# 3. Configure environment variables
cp .env.example .env

# 4. Start PostgreSQL
docker compose up -d

# 5. Run database migrations and seed the initial admin user
pnpm db:migrate
pnpm db:seed

# 6. Start the development server
pnpm dev
```

The app will be available at [http://localhost:3000](http://localhost:3000).

> Script names above follow common conventions; adjust them to match your `package.json`.

---

## Environment Variables

Create a `.env` file based on `.env.example`:

| Variable         | Description                                                   |
| ---------------- | ------------------------------------------------------------- |
| `DATABASE_URL`   | PostgreSQL connection string                                  |
| `AUTH_SECRET`    | Secret used to sign sessions                                  |
| `APP_URL`        | Public base URL of the application                            |
| `STORAGE_DRIVER` | `local` or `s3`                                               |
| `STORAGE_PATH`   | Directory for uploaded files when using local storage         |
| `ADMIN_EMAIL`    | Email of the initial admin user created by the seed script    |
| `ADMIN_PASSWORD` | Password of the initial admin user (change after first login) |

Never commit real secrets to the repository.

---

## Project Structure

A suggested layout:

```
vezia-crm/
├── src/
│   ├── app/                # Next.js routes and pages
│   ├── components/         # Shared UI components (shadcn/ui based)
│   ├── server/
│   │   ├── db/             # Drizzle schema, migrations, seed
│   │   ├── routers/        # oRPC routers per module
│   │   └── services/       # Business logic (invoice totals, numbering, PDF)
│   └── lib/                # Utilities (Jalali dates, currency, text normalization)
├── drizzle/                # Generated migrations
├── docker-compose.yml
└── .env.example
```

---

## Core Business Rules

- Invoice and letter numbers are generated automatically, are unique, and are allocated inside a database transaction.
- An issued invoice is locked; changes require cancelling and reissuing.
- Monetary amounts are stored as integers (in Rial). All totals are calculated on the server only.
- Parties and sellers that have invoices are never deleted, only deactivated.
- Receivables are the sum of issued, unpaid sales invoices.
- All access checks are enforced on the server for every API procedure.

---

## Roadmap

**Phase 1 – Core**

- [ ] Project setup, authentication, RTL layout
- [ ] Seller profiles and parties
- [ ] Invoices with PDF export

**Phase 2 – Paperwork**

- [ ] Letters and templates
- [ ] Contracts
- [ ] Company documents

**Phase 3 – Daily work**

- [ ] Tasks and notes
- [ ] Dashboard
- [ ] Backups and deployment

**Later**

- [ ] Partial payments and detailed account balances
- [ ] Tax authority system integration
- [ ] Activity log and document versioning
- [ ] Automatic reminders, email and SMS
- [ ] OCR for scanned documents
- [ ] Fine-grained roles and confidentiality levels

---

## Contributing

This is currently a private, internal project. If you have access and want to contribute:

1. Create a feature branch from `main`.
2. Keep changes focused on a single vertical slice (database, API, UI, and a basic test).
3. Open a pull request with a short description and screenshots for UI changes.

---

## License

License to be determined. All rights reserved until a license is added.

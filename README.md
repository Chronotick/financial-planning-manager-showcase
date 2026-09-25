# Financial Planning Manager — Showcase

A **local-first personal finance desktop application** for recording money movement,
managing monthly budgets, and building toward structured financial statements and
future planning.

> **Portfolio showcase only.**  
> The production repository is private. This public repository intentionally contains
> no personal financial records, private spreadsheets, local databases, screenshots,
> credentials, machine-specific paths, or proprietary implementation source.

## What the project does

The application connects day-to-day ledger activity with monthly budget decisions.

```text
Accounts + Transactions + Transfers
              │
              ▼
        Local SQLite ledger
              │
      ┌───────┴────────┐
      ▼                ▼
 Transactions       Budgets
      │                │
      └───────┬────────┘
              ▼
     Budget vs. Actual
              │
              ▼
        Variance Review
              │
              ▼
   Financial Statements
      (in development)
              │
              ▼
      Future Planning
       (later milestone)
```

## Implemented capabilities

### Transactions and accounts

- Create and edit transactions.
- Confirmed void/delete workflows.
- Filter and tag ledger activity.
- Signed income/expense summaries.
- Create local bank accounts.
- Record explicit account-to-account transfers separately from income and expenses.

### Monthly budgeting

- Create and edit monthly budget periods.
- Add category-level planned amounts.
- Compare plan versus actual posted ledger activity.
- Classify overspend, underspend, and on-track results.
- Navigate between months.
- Copy **plan values only** into the next month.
- Recalculate actuals when the underlying transaction data changes.
- Preserve durable budget definitions in local SQLite storage.

### Financial statement foundation

- A tested **profit-and-loss calculation engine** is implemented.
- Category grouping and source provenance are retained.
- Incomplete or failed statement inputs produce explicit diagnostics rather than
  silently returning misleading totals.
- Statement UI/export, balance-sheet and cash-flow engines remain future work.

## Architecture

![High-level architecture](assets/architecture.svg)

The application is intentionally **desktop-first, offline-first, single-user, and
privacy-oriented**.

### Main technology stack

| Area | Technology |
|---|---|
| Desktop shell | Tauri |
| Frontend | React + TypeScript |
| Build tooling | Vite |
| Local database | SQLite |
| Persistence | Drizzle ORM + better-sqlite3 |
| Validation | Zod |
| State | Zustand |
| Unit/integration tests | Vitest |
| Browser workflow tests | Playwright |
| Architecture enforcement | ESLint + dependency-cruiser |

The React UI talks to feature gateways and use-case services. Native desktop requests
cross a private local boundary to a bundled Node ledger runtime backed by SQLite.
Persistence is kept behind repository/query-service boundaries rather than accessed
directly from UI components.

## Data model

The private implementation includes separate domain structures for:

- profiles
- accounts
- hierarchical categories
- tags
- transactions
- transfers
- monthly budget periods
- budget lines
- statement context
- future planning structures

Money calculations use **integer minor units** instead of floating-point aggregation.

## Engineering decisions

### Local-first privacy boundary

Real accounts, transactions, balances, budgets, forecasts, exports, backups, and
local databases stay outside Git. The desktop app stores its ledger in local
application data; browser-development and tests use isolated local databases.

No cloud account or bank synchronization is required for the current baseline.

### Explicit financial semantics

Transfers are modeled separately from income and expenses. Budget actuals are derived
from posted ledger activity instead of being copied into budget definitions. Statement
calculation retains source provenance and diagnostics so errors remain inspectable.

### Durable budget workflow

Budget definitions survive process restart. Actual values are calculated from the
ledger, which means changing a transaction can update budget variance without rewriting
the plan.

### Boundary enforcement

Subsystem boundaries are enforced through import restrictions and dependency-graph
checks. Persistence, financial rules, UI workflows, and statement logic are kept as
separate concerns.

## Example budget behavior

A simplified workflow demonstrated by the private implementation:

1. Record a grocery expense.
2. Create a monthly Grocery budget.
3. Review planned versus actual spending.
4. Edit the plan and observe updated variance.
5. Edit the underlying transaction and return to the budget.
6. Actuals recalculate from the ledger while the plan remains unchanged.
7. Copy the plan into the next month without copying the previous month's actuals.
8. Restart the desktop application and reopen the saved budget.

This workflow has been exercised through unit, integration, browser, and native
desktop verification in the private development repository.

## Current implementation status

The private project currently has a working transaction-management and monthly-budget
baseline. The profit-and-loss calculation engine is implemented and tested.

The following are **not claimed as completed** in this showcase:

- balance-sheet engine
- cash-flow engine
- full financial statement UI/export
- automated forecasting
- recommendation engine
- bank synchronization
- automatic backup/restore
- database encryption
- cloud accounts
- multi-user collaboration
- tax-filing functionality

## Development context

This is an **AI-assisted software development project**. My contribution includes
requirements definition, workflow and product design, financial/domain modeling,
architecture decisions, integration, debugging, verification, and iterative
refinement with AI coding tools.

I do not claim that every line of the private implementation was manually authored.

## What is intentionally not public

The production repository remains private. This showcase excludes:

- production source code
- personal financial records
- local SQLite databases
- private spreadsheets and trackers
- screenshots containing local data
- raw logs and backups
- machine-specific configuration
- internal governance and debugging artifacts

The purpose of this repository is to demonstrate the **product scope, architecture,
financial semantics, implementation depth, and engineering decisions** without
publishing the private application itself.

## Repository note

If you are reviewing this project for a role, I can discuss the architecture,
financial modeling decisions, debugging process, testing strategy, and selected
sanitized implementation examples.

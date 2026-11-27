# SprintBoard

SprintBoard is a Jira-inspired project management platform built as a personal full-stack engineering and portfolio project. It is designed to support workspace-based collaboration, project planning, work item tracking, and sprint management through incremental feature delivery.

## Project status

**Current phase:** R0 — Engineering Bootstrap  
**Current work package:** R0-01 — Repository Bootstrap (in progress)

The approved product scope covers 22 modules and 170 features (F001–F170). This is the planned scope, not a list of implemented features.

| Area | Current status |
| --- | --- |
| Frontend | React/Vite/Tailwind scaffold; development server, build and lint verified |
| Backend | NestJS/ESM scaffold; development server, build, lint, starter unit and E2E tests verified |
| Monorepo | npm Workspaces and root scripts configured; combined development startup pending explicit verification |
| Database | Existing MiniJira physical schema deployed and structurally verified; Document 06 v1.0 approved |
| Prisma | Database First and migration baseline design approved; runtime integration and baseline execution not yet completed |
| Business features | Not implemented in this bootstrap |
| CI/CD | Planned; not yet verified or claimed as operational |

The current UI and API responses are bootstrap smoke checks, not production features.

## Technology stack

| Area | Technologies |
| --- | --- |
| Frontend | React, TypeScript, Vite, Tailwind CSS v4 |
| Backend | NestJS, TypeScript, ES Modules (ESM), Express, REST API |
| Database (approved design) | PostgreSQL, Prisma ORM (7.x stable target; exact version to be verified and pinned) |
| Architecture | Modular Monolith, npm Workspaces |
| Testing | Vitest, Supertest (backend); integration and frontend testing to be added in the relevant phases |
| Code quality | ESLint (frontend), Oxlint (backend), Prettier (backend) |
| Development tooling | Concurrently |
| Planned API/security tooling | Swagger, JWT |
| Planned infrastructure | Docker, Docker Compose, GitHub Actions |

Dependency versions are managed by the workspace `package.json` files and the root `package-lock.json`.

## Repository structure

```text
SprintBoard/
├── .agents/
│   └── skills/                 # Repository-level NestJS agent skills
├── apps/
│   ├── web/                    # React + Vite application
│   │   ├── src/
│   │   │   ├── app/
│   │   │   │   └── App.tsx
│   │   │   ├── index.css
│   │   │   └── main.tsx
│   │   └── package.json
│   └── api/                    # NestJS application
│       ├── src/
│       │   ├── app.controller.spec.ts
│       │   ├── app.controller.ts
│       │   ├── app.module.ts
│       │   ├── app.service.ts
│       │   └── main.ts
│       ├── test/
│       ├── AGENTS.md
│       └── package.json
├── .gitignore
├── AGENTS.md
├── package.json
├── package-lock.json
└── README.md
```

This is a simplified view of the current bootstrap. Configuration files and optional or empty directories are omitted.

### Architecture conventions

The frontend follows a feature-based structure. Add `src/features/`, `src/components/`, and `src/lib/` when they have a concrete purpose.

The backend follows a domain/module-based Modular Monolith. Add `src/config/`, `src/common/`, `src/infrastructure/`, and `src/modules/` incrementally. Prisma schema and migrations will reside in `apps/api/prisma/` when introduced.

The approved backend data access flow is:

```text
Controller -> Service -> Repository -> PrismaService -> PostgreSQL
```

Controllers handle HTTP boundaries, services enforce business rules and authorization, repositories encapsulate data access, and PrismaService provides shared database connectivity. Controllers and services must not query Prisma directly. Do not create empty domain modules or repositories solely to match the long-term architecture.

## Getting started

### Prerequisites

The following versions were used during the local Windows bootstrap:

- Node.js 24.22.3 (Node.js 24 LTS)
- npm 11.19.1
- Git

Docker, Docker Compose and PostgreSQL are part of the approved engineering stack, but are not required to run the current scaffold or its starter tests. No database credentials are needed for this bootstrap.

### Install dependencies

After obtaining a local copy of the repository, open a terminal at the repository root:

```bash
npm ci
```

Use the root lockfile for reproducible installations. Use `npm install` when intentionally adding or updating dependencies. Do not maintain independent lockfiles for the two applications.

To check that both workspaces are recognized:

```bash
npm ls --workspaces --depth=0
```

### Development

Start both applications from the root:

```bash
npm run dev
```

Or start them independently in separate terminals:

```bash
npm run dev:web
npm run dev:api
```

| Application | Default local URL | Expected bootstrap response |
| --- | --- | --- |
| Web | http://localhost:5173 | SprintBoard test UI |
| API | http://localhost:3000 | `Hello World!` |

Use the actual URL printed by each server if its port differs. Press `Ctrl+C` to stop a running development server.

### Build and run compiled backend

```bash
npm run build
npm run start
```

The build command compiles both workspaces. The start command runs the compiled NestJS backend only; it does not host the frontend or constitute a production deployment. The frontend build output is located at `apps/web/dist/`.

## Root scripts

Run these commands from the repository root.

| Command | Purpose |
| --- | --- |
| `npm run dev` | Run the web and API development servers concurrently |
| `npm run dev:web` | Start the Vite development server |
| `npm run dev:api` | Start NestJS in watch mode |
| `npm run build` | Build both workspaces |
| `npm run build:web` | Build the frontend |
| `npm run build:api` | Build the backend |
| `npm run start` | Start the compiled backend after building it |
| `npm run lint` | Lint both workspaces |
| `npm run lint:web` | Run frontend ESLint |
| `npm run lint:api` | Run backend Oxlint |
| `npm run test` | Run the configured backend Vitest test suite |
| `npm run test:e2e` | Run backend E2E tests |
| `npm run test:cov` | Run backend tests with coverage |
| `npm run format` | Format backend source and test files with Prettier (writes files) |

Frontend automated tests and a separate integration test command are not configured in the current bootstrap.

## Testing and validation

For the current scaffold, run the following from the root:

```bash
npm run build
npm run lint
npm run test
npm run test:e2e
npm run test:cov
```

The existing backend tests cover starter behavior only. Passing these checks does not establish that business features, database constraints, concurrency behavior, recovery, or deployment have been tested.

When database-dependent tests are introduced, verify the target environment first and use an isolated test database. Never use the protected MiniJira database for automated tests.

## Database safety

The existing `MiniJira` PostgreSQL database is protected. Its physical schema was deployed using D06 Patch v0.7 and structurally verified (postflight 11/11 PASS). The approved baseline is documented in Document 06 v1.0: seven business schemas, 95 tables, 252 foreign keys, 11 user triggers, and 173 SQL-only rules. These are baseline inventory figures, not permanent limits on approved future evolution.

The approved strategy is Database First. Prisma and DBML must reflect the approved physical schema, including SQL-specific constraints and behavior.

- Do not connect to MiniJira or execute database operations without the required explicit authorization.
- Never run `prisma migrate reset`, `prisma migrate dev`, or `prisma db push` against MiniJira.
- Never recreate MiniJira or rerun historical D06 deployment SQL.
- Validate the migration baseline on an isolated database before any registration on MiniJira.
- Obtain separate approval before writing baseline metadata to `_prisma_migrations` or applying an approved schema migration to MiniJira.
- Do not commit real environment files, credentials, or database backups.

See the root `AGENTS.md`, Document 06 and Document 06D for the full safety and approval rules. No database setup or migration command is required to run the current application scaffold.

## Project documentation

The approved project documents are the source of truth for product scope, roadmap, architecture, domain rules, database design, and migration policy. The [root AGENTS.md](AGENTS.md) contains the document index and access links. Access to private Google Docs may require permission from the project owner.

`Approved` means a design or decision has been accepted. `Implemented` and `Tested` require separate evidence. Do not interpret the approved product scope or database deployment as proof that application features are complete.

## AI-assisted development

Codex is used as a development aid. Repository instructions and official NestJS skills are maintained at:

- [Root AGENTS.md](AGENTS.md) — project-wide rules.
- [Backend AGENTS.md](apps/api/AGENTS.md) — backend-specific rules.
- `.agents/skills/` — official NestJS agent skills.

Framework documentation and skills are technical references. They must not override approved project decisions or database safety rules.

## Development workflow

1. Review the relevant approved documents and feature scope.
2. Make small, focused changes following the approved architecture.
3. Add or update relevant tests and run validation in the affected workspace.
4. Review the diff, report results and obtain any required approvals.
5. Commit and push only authorized changes.

Keep `package-lock.json` under version control. Do not commit dependencies, build outputs, coverage reports, secrets, or local database backups.

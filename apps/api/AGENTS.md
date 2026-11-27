
# SprintBoard - Backend Agent Instructions

Version: 1.0
Status: Approved
Last updated: 2026-11-26

## 1. Instruction Scope and Authority

These instructions apply to `apps/api` and its subdirectories.

Read and follow the root `AGENTS.md` before performing any backend task.
These instructions supplement the root `AGENTS.md` and must never override
or weaken its requirements.

Official NestJS documentation and agent skills are technical references,
not authorization to change approved project decisions.

If a conflict exists, stop the affected work and report it to the project owner.

## 2. Backend Technology Stack

- NestJS
- TypeScript
- ES Modules (ESM)
- Express HTTP adapter
- REST API
- PostgreSQL
- Prisma ORM
- Vitest
- Supertest
- Oxlint
- Prettier

Do not replace or introduce alternative technologies without explicit approval.

Do not switch to TypeORM, Drizzle, Sequelize, Mongoose, Fastify, NodeAdapter,
GraphQL, or microservices without approval.

## 3. NestJS Documentation and Agent Skills

Official documentation:

- <https://docs.nestjs.com/>
- <https://docs.nestjs.com/llms.txt>

Repository-level skills (paths relative to the repository root):

- `.agents/skills/nestjs/SKILL.md`
- `.agents/skills/nestjs-auth/SKILL.md`
- `.agents/skills/nestjs-data/SKILL.md`
- `.agents/skills/nestjs-feature/SKILL.md`
- `.agents/skills/nestjs-production/SKILL.md`
- `.agents/skills/nestjs-testing/SKILL.md`

- Verify that a skill exists before using it.
- Read only the relevant `SKILL.md` files when applicable.
- Verify recommendations against the installed NestJS version and approved
  project documents.
- Do not install or update agent skills automatically without project owner
  approval.
- Report unavailable skills or documentation instead of claiming to have read them.

## 4. ES Modules and TypeScript

This application uses ES Modules.

- Follow the existing TypeScript and Node.js module configuration.
- Use `.js` extensions for relative imports where required by the ESM configuration.
- Prefer `import` and `export` syntax.
- Use `import.meta.dirname` when directory access is needed and supported by
  the configured runtime.
- Follow strict TypeScript rules in the root `AGENTS.md`.
- Never use `any`, `as any`, `@ts-ignore`, or `@ts-nocheck` to bypass type safety.
- Do not weaken compiler or lint settings.

## 5. Backend Architecture

Required data access flow:

`Controller -> Service -> Repository -> PrismaService`

- Controllers handle HTTP requests and responses.
- Services enforce business rules and authorization.
- Repositories encapsulate database queries.
- PrismaService provides shared database connectivity.
- Use NestJS dependency injection for provider management.
- Expose only necessary providers through module exports.
- Respect approved domain ownership and module boundaries.
- Avoid circular dependencies.
- Do not access Prisma directly from controllers or services.
- Do not access another module's repository directly without an approved
  architectural design.
- Do not expose Prisma models directly as API response contracts.
- Create modules and repositories only when needed.
- Do not create generic CRUD repositories unnecessarily.
- Preserve approved transaction boundaries.
- Do not bypass the repository layer to implement transactions.
- Follow approved concurrency and locking strategies.
- Request clarification if a required transaction pattern has not been defined.

Follow Document 03 for source code organization and architectural decisions.

## 6. Request Validation

Follow the approved DTO-based validation approach defined in Document 03.

Use `class-validator` and NestJS `ValidationPipe` when implementing the
approved validation foundation.

Do not replace the approved validation strategy with Standard Schema, Zod,
Valibot, or other alternatives without explicit approval.

Business rules and authorization belong in the appropriate service or policy layer.

## 7. Database Safety - Critical

All database safety rules in the root `AGENTS.md` apply without exception.

MiniJira is a protected database.

- Follow Document 06 and Document 06D.
- Follow the approved Database First strategy.
- Never automatically synchronize the database.
- Never reset or recreate MiniJira.
- Never rerun historical SQL deployment patches.
- Preserve all approved SQL-specific database rules.
- Never execute unauthorized database operations.
- Use isolated databases for database-dependent testing.
- Obtain separate approval for migration execution and baseline registration.

NestJS agent skills must not override these restrictions.

If database access is required, stop and follow the applicable authorization
process in the root `AGENTS.md` before connecting or executing commands.

## 8. Testing

This application uses Vitest and Supertest.

### Testing Rules

- Use `vi.fn()` instead of `jest.fn()`.
- Place unit tests beside implementation files.
- Keep E2E tests in the `test` directory.
- Add or update focused tests for changed business behavior, including relevant
  failure cases and authorization rules.
- Mock repositories and external dependencies in isolated unit tests when appropriate.
- Use real PostgreSQL behavior in integration tests when required to verify
  database constraints, transactions, or concurrency behavior.
- Preserve existing test coverage and assertions.
- Verify the target test database before initializing database-dependent
  application modules.
- Run database-dependent automated tests only against isolated test databases.
- Never use MiniJira as an automated test database.
- Close application instances and database connections after tests.
- Never call real external services during automated tests without explicit approval.

### Validation Commands

Inspect the relevant `package.json` files and Vitest configuration before
running validation commands.

Relevant root-level commands, when available:

- `npm run build:api`
- `npm run lint:api`
- `npm run test`
- `npm run test:e2e`
- `npm run test:cov`

Use the corresponding `apps/api` workspace scripts when root-level scripts
are unavailable. Run the checks relevant to the requested changes.

Report executed commands, actual results, failed checks, and skipped checks.
Never claim that unexecuted tests have passed.

Do not disable tests, weaken assertions, or bypass validation to obtain a passing result.

## 9. Observability and Deployment

NestJS Observe has not been approved for this project.

Do not:

- Install `@nestjs/observe`.
- Register `ObserveModule`.
- Run NestJS Observe linking commands.
- Send telemetry to external services.
- Configure Observe MCP.
- Execute `nest deploy` without approval.

Follow the approved logging, security, and deployment conventions for SprintBoard.
Do not expose secrets or sensitive data in logs.

## 10. Implementation Workflow

Follow the implementation workflow defined in the root `AGENTS.md`.

- Keep changes focused on the authorized scope.
- Explain important implementation decisions and technical trade-offs.
- Obtain required approvals before changing architecture, database design,
  or technology choices.
- Run relevant validation and report actual results.
- Identify affected approved project documents and propose updates separately.
- Communicate explanations and completion reports in Vietnamese.
- Use English for source code identifiers and technical files.

Do not automatically proceed to another task.

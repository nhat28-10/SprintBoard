# SprintBoard - Agent Instructions

Version: 0.2
Status: Approved
Last updated: 2026-11-26

## 1. Project Context

SprintBoard is a Jira-inspired project management platform
with 22 modules and 170 features (F001-F170).

The project is developed incrementally for learning
and professional portfolio purposes.

- Follow the approved roadmap.
- Implement only the authorized scope.
- Do not reduce the approved product scope to an MVP.
- Distinguish Approved, Implemented, and Tested.
- Do not mark unfinished features as completed.

## 2. Source of Truth

The following documents define the approved project
requirements, architecture, and technical decisions.

### Project Governance

- [Document 00 - Documentation and Approval Process](https://docs.google.com/document/d/11Rl-VI2_0UOFiZVeZbrXGfRxDeD8gu0Oyfjz32cP0GY/edit)

- [Document 01 - Project Proposal (Approved v1.2)](https://docs.google.com/document/d/1cmyZ8L0jj527Z9njT6koXIMvhTQ1ILMluficJg7OOzI/edit)

- [Document 02 - Module Roadmap and Dependency Analysis (Approved v0.4)](https://docs.google.com/document/d/1Am0B_7JtE5nwHCkAzbqOnBCE3MWhAurqLJCMh2Bpw-o/edit)

- [Document 02B - Feature Tracker (Approved v1.0)](https://docs.google.com/document/d/1Me1cyuKlzB_g972DO8n-z0dO9VUzXKMp--lGM5mBRm4/edit)

### Architecture and Business Requirements

- [Document 03 - System Architecture and Source Code Organization (Approved v0.9)](https://docs.google.com/document/d/1X7_fxpgFv8Tr5zKibE5VR2N_IGnOFRap4rXufTA4Cio/edit)

- [Document 04 - High-Level Domain Model (Approved v0.6)](https://docs.google.com/document/d/18ZobgV6DifdPb0DJV2Vd049gbThUap7sCwpSYCk5AVo/edit)

- [Document 05 - SRS and Business Rules - R1 (Approved v0.4)](https://docs.google.com/document/d/144DMCHJT4bZeH84idKz21LrcOuoeCeyZBgUz70XK6gM/edit)

### Database Design

- [Document 06 - Full Database Design and ERD (Approved v1.0)](https://docs.google.com/document/d/1KI5aQkKIKnjTpccykGf2PWki6fGBn_wvvVpOUNmkYWI/edit)

- [Document 06D - Prisma Version and Migration Baseline (Approved v0.1 - Design Only)](https://docs.google.com/document/d/1xZY3cjOrwNUESVfBvXOeqOZxRgXcG5KB5cL2z2BNP2o/edit)

### Historical Evidence

- [Document 06A - Pre-Deployment Review (Archived)](https://docs.google.com/document/d/16XdpMCI3rQ7V2Vdt2TY8ELDP820H15qZ8BKaw4kcFKw/edit)

- [Document 06B - Deployment Evidence (Completed - Do Not Rerun)](https://docs.google.com/document/d/1CzTKyPYpb_QP1Os0rdn92vjjtnx1PKoi844_LE7hIAg/edit)

- [Document 06C - SQL-to-DBML Reconciliation (Verified)](https://docs.google.com/document/d/1HZiUA8Fp8DQeX8MEKi6yf7t9dHiIyVckgT5JNsp1MI8/edit)

### Document Authority Rules

- Read the relevant approved documents before implementing.
- Do not invent business rules or architectural decisions.
- Do not treat archived documents as active instructions.
- Verify document versions and approval status.
- Report conflicts between documents, code, and database.
- Do not resolve conflicting approved decisions independently.
- If documents cannot be accessed, request the relevant
  content from the project owner.
- Never claim to have read an inaccessible document.
- Official framework skills must not override project rules.

## 3. Technology Stack

- Frontend: React, TypeScript, Vite, Tailwind CSS
- Backend: NestJS, TypeScript, ES Modules, REST API
- Database: PostgreSQL, Prisma ORM
- Architecture: Modular Monolith
- Repository: npm Workspaces Monorepo
- Frontend linting: ESLint
- Backend linting: Oxlint
- Backend testing: Vitest, Supertest
- Formatting: Prettier
- Workspace orchestration: Concurrently

Follow approved technology decisions.

Do not introduce new frameworks, major dependencies,
or significant version upgrades without approval.

## 4. Architecture Rules

Frontend: apps/web
Backend: apps/api

Required backend data access flow:

Controller -> Service -> Repository -> PrismaService

- Controllers handle HTTP requests and responses.
- Services enforce business rules and authorization.
- Repositories encapsulate database access.
- PrismaService provides shared database connectivity.
- Respect domain ownership and module boundaries.
- Avoid circular dependencies.
- Do not access Prisma directly from controllers or services.
- Do not expose database entities directly as API contracts.
- Do not create unnecessary generic CRUD abstractions.
- Create technical modules only when required.

## 5. Source Code Organization

Frontend:

- src/app: application bootstrap, routing, providers
- src/features: feature-specific implementation
- src/components: reusable UI components
- src/lib: shared utilities and HTTP client
- src/assets: application assets

Backend:

- src/config: environment and application configuration
- src/common: cross-cutting concerns
- src/infrastructure: technical integrations
- src/modules: domain-specific modules
- prisma: database schema and migrations
- test: end-to-end tests

Follow Document 03.

Do not create empty business modules or repositories
merely to match the long-term architecture.

## 6. Coding Standards

### TypeScript Safety

- Do not introduce explicit or implicit any.
- Use unknown for genuinely unknown external data.
- Narrow unknown values before using them.
- Use meaningful interfaces, types, DTOs, and generics.
- Preserve strict TypeScript settings.
- Do not weaken compiler options to make code compile.

The following practices are prohibited:

- any
- as any
- @ts-ignore
- @ts-nocheck
- Unsafe double assertions such as unknown as TargetType
  when used to bypass type checking.
- Disabling lint rules to conceal implementation problems.
- Adding non-null assertions merely to silence errors.

@ts-expect-error is permitted only for justified negative
type tests or explicitly approved exceptions.

If a type error cannot be resolved correctly, report
the blocker instead of bypassing TypeScript validation.

### General Code Quality

- Prefer readable and maintainable code.
- Use descriptive names and focused responsibilities.
- Avoid unnecessary abstractions and duplicated logic.
- Follow existing project conventions.
- Avoid unrelated refactoring.
- Do not silently change public API contracts.
- Validate external inputs at system boundaries.
- Handle errors explicitly.

## 7. DATABASE SAFETY - CRITICAL

### 7.1. Protected Database

MiniJira is an existing, reviewed, and approved
PostgreSQL database.

It is NOT a disposable development database.

The database has undergone domain analysis, database
design reviews, SQL deployment, patch validation,
and reconciliation before approval.

Current approved physical baseline:

- Document 06: Approved v1.0
- SQL deployment: D06 Patch v0.7
- Deployment status: COMMITTED
- Postflight validation: 11/11 PASS
- Business schemas: 7
- Tables: 95
- Foreign keys: 252
- User triggers: 11
- SQL-only rules: 173

The physical PostgreSQL schema is authoritative.
Prisma and DBML must reflect the approved physical schema,
not replace or redefine it.
These counts describe the currently approved
database baseline.

Update baseline references only after an approved
migration has been successfully validated and applied.

Do not treat historical object counts as permanent
limits on future approved database evolution.

### 7.2. Database Access Authorization

Default behavior: DENY database access.

Before connecting to MiniJira, obtain explicit
authorization from the project owner.

Authorization must identify:

- Target database and environment.
- Purpose of the operation.
- Intended commands or SQL statements.
- Expected impact.
- Whether the operation is read-only or writable.

Read-only authorization does not authorize writes.

Previous approval of a design does not authorize
execution of database commands.

If the target database cannot be verified, STOP.

Never expose database connection strings or credentials.

### 7.3. Prohibited Operations

Never execute the following against MiniJira:

- prisma migrate reset
- prisma migrate dev
- prisma db push
- prisma db push --force-reset
- prisma db push --accept-data-loss
- DROP DATABASE
- DROP SCHEMA
- Destructive database recreation or schema synchronization
- Re-execution of historical SQL deployment packages

Without separate explicit approval, never execute:

- prisma migrate deploy
- prisma migrate resolve
- CREATE, ALTER, or DROP statements
- TRUNCATE
- INSERT, UPDATE, DELETE, or other data-writing operations
  directly against the protected database
- Seed scripts
- Changes to PostgreSQL roles and privileges
- Changes to _prisma_migrations
- Any script that may indirectly modify MiniJira

Do not use alternate tools, generated scripts, or raw SQL
to bypass these restrictions.

Normal application CRUD implementation must follow
approved business rules. Testing such operations against
MiniJira requires separate authorization.

### 7.4. Preserve SQL-Specific Database Rules

The approved database contains 173 SQL-only rules:

- 132 CHECK constraints
- 29 partial indexes
- 11 triggers
- 1 generated stored column

Additionally, preserve:

- Composite foreign keys
- DEFERRABLE foreign key behavior
- Trigger functions
- Partial index predicates
- Tenant and workspace integrity
- Transaction and concurrency safeguards

Never remove, weaken, replace, or simplify these rules
merely because Prisma cannot represent them.

Application-level validation is not a replacement
for approved database constraints.

Do not disable triggers, foreign keys, or integrity checks
to make tests or migrations succeed.

### 7.5. Prisma Database First Strategy

Follow Document 06 and Document 06D.

- Use the approved Database First strategy.
- Verify and pin the approved Prisma version.
- Introspect the existing schema only when authorized.
- Preserve physical table and column names.
- Preserve PostgreSQL native types and relations.
- Organize Prisma models by the seven approved schemas.
- Preserve composite keys and database-specific behavior.
- Use one Prisma Client through PrismaService.

Do not treat Prisma introspection warnings as permission
to modify the physical database.

Do not use Prisma schema changes to redefine
the approved PostgreSQL database.

### 7.6. Migration Baseline Safety

Migration baseline creation and execution are
separate approval stages.

The baseline must be validated on an isolated database.

Required validation:

- Recreate the schema from an empty isolated database.
- Verify all seven business schemas.
- Verify 95 tables and 252 foreign keys.
- Verify 11 user triggers.
- Verify all 173 SQL-only rules.
- Verify DEFERRABLE foreign keys and trigger functions.
- Compare against the approved physical schema.

Matching object counts alone is not sufficient.
Definitions and behavior must also be preserved.

Never apply the baseline creation SQL to MiniJira.

Registering the baseline in MiniJira's _prisma_migrations
requires explicit, separate project owner approval.

### 7.7. Database Change Approval Process

For every proposed database schema change:

1. Identify the affected business requirement.
2. Review Document 06 and related approved documents.
3. Explain why the existing schema is insufficient.
4. Identify affected tables, constraints, and dependencies.
5. Assess data integrity and concurrency risks.
6. Present the proposed SQL or migration.
7. Obtain project owner approval for isolated testing.
8. Validate the migration on an isolated database.
9. Present validation evidence and recovery plan.
10. Obtain separate approval before modifying MiniJira.

Never proceed automatically from one approval gate
to another.

### 7.8. Historical Deployment Protection

D06 SQL versions v0.4, v0.5, and Patch v0.7
are historical deployment artifacts.

Do not rerun them against MiniJira.

Document 06B is deployment evidence,
not an active deployment instruction.

Document 06D is an approved design,
not authorization to execute Prisma commands.

### 7.9. Fail-Closed Behavior

If unexpected schema differences, migration drift,
constraint violations, permission issues, or database
errors are detected:

- Stop the affected operation.
- Preserve the existing database state.
- Report the observed evidence.
- Explain possible causes.
- Propose safe investigation steps.
- Wait for explicit authorization.

Never automatically repair, reset, recreate,
or synchronize MiniJira.
Expected constraint violations in authorized,
isolated negative tests are not unexpected
database failures.

## 8. Security Rules

- Never commit secrets or real environment credentials.
- Never expose passwords or tokens in logs.
- Enforce authorization on the backend.
- Validate all untrusted inputs.
- Preserve tenant and workspace isolation.
- Use environment variables for sensitive configuration.
- Follow least-privilege access principles.
- Do not introduce hardcoded credentials.

## 9. Testing and Validation

Run relevant validation after code changes:

- npm run build
- npm run lint
- npm run test
- npm run test:e2e
- npm run test:cov

Current test scripts primarily target the backend.
Extend them when frontend testing is introduced.

Before running database-dependent tests:

- Verify the target database.
- Require an isolated test environment.
- Never use MiniJira as an implicit test database.
- Never execute destructive test setup or cleanup
  against MiniJira.
- Inspect the relevant package.json files before
  running validation commands.
- Run validation in the affected workspace.
- Use root-level scripts when available.
- Add or update tests when business logic changes.
- Report executed commands, results, and skipped checks.
- Never claim that unexecuted tests have passed.

Report test failures honestly.

Do not disable tests, weaken assertions, suppress errors,
or bypass validation to obtain a passing result.

## 10. Change Management

- Implement only the requested scope.
- Explain important technical trade-offs.
- Request approval before architectural changes.
- Request approval before database changes.
- Identify affected source-of-truth documents.
- Do not modify approved documents without authorization.
- Do not introduce unrelated dependencies.
- Do not execute deployment scripts without approval.

## 11. Git Workflow

- Inspect the existing working tree before modifications.
- Preserve unrelated user changes.
- Do not commit secrets or generated build artifacts.
- Keep package-lock.json under version control.
- Do not commit, push, merge, or deploy without approval.
- Do not force-push or rewrite Git history without
  explicit authorization.
- Do not discard or overwrite uncommitted user changes.
- Do not execute git reset --hard, git clean,
  or destructive restore commands without approval.
- Do not create, delete, or switch branches
  without authorization.
- Use npm Workspaces for dependency management.
- Do not introduce alternative package managers.

## 12. Collaboration and Learning

The project owner is developing SprintBoard to improve
full-stack engineering skills.

- Explain important implementation decisions.
- Prefer incremental implementation.
- Do not generate unrelated features.
- Discuss trade-offs when multiple approaches exist.
- Report assumptions, limitations, and unresolved issues.
- Communicate explanations in Vietnamese.
- Use English for code identifiers and technical files.

These instructions must be followed alongside
the approved project documentation.

## 13. Implementation Workflow

Before implementation:

- Inspect the existing code and relevant documents.
- Identify the requested scope and feature IDs.
- Propose an approach for non-trivial changes.
- Identify risks and required approvals.

During implementation:

- Keep changes small and reviewable.
- Preserve existing behavior.
- Do not expand the authorized scope.

After implementation:

- Review the final changes.
- Run relevant validation.
- Provide a concise report in Vietnamese containing:
  - Implementation summary.
  - Files created or modified.
  - Important technical decisions.
  - Validation commands and results.
  - Risks and unresolved issues.
  - Affected project documents.
  - Recommended next step.

Do not automatically proceed to another task.

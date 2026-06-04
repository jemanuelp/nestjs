---
name: nestjs-development
description: Flujo de trabajo de desarrollo, build, tests, migraciones y criterios de PR para arquetipos NestJS.
---

# NestJS Development Workflow

## Project Context

- This repository provides reusable skills for NestJS project archetypes.
- Keep development workflow guidance generic unless the target project provides concrete scripts, services, or infrastructure.
- Prefer validating actual project scripts before assuming command names.

## Root-level workflow

```bash
make up
make upb
make logs
make logs api
make stop
make down
make down-v
```

## API workflow (`api/`)

```bash
cd api
npm run start:dev
npm run build
npm run start:prod
npm run lint
npm run lint:check
npm run test
npm run test:unit
npm run test:e2e
npm run test:cov
```

## Migrations workflow (`migrations/`)

```bash
cd migrations
./migrations.sh status
./migrations.sh migrate
./migrations.sh execute --up "<VersionClass>"
./migrations.sh generate
```

## Database & Persistence
- TypeORM is used in `api/` with provider selection via `DB_PROVIDER`.
- Supported providers in factory: `postgres`, `mysql`, `sqlite`.
- Schema evolution is handled by Doctrine migrations in `migrations/migrations/`.
- Migration files are versioned (`VersionYYYYMMDD...php`) and should be immutable once applied.
- When adding a new NestJS entity class, include it in `api/src/database/entities.ts` (`entitiesList`) so TypeORM loads the metadata.

## Testing Guidelines
- Antes de correr cualquier suite (`test`, `test:unit`, `test:e2e`, `test:cov`), verificar que los servicios requeridos por el proyecto estén `Up`:
  ```bash
  docker compose ps
  ```
- Place new unit tests under `api/test/unit/`.
- Place new e2e tests under `api/test/e2e/`.
- For service tests, mock external dependencies (DB, queues, external APIs).
- For integration/e2e tests, keep deterministic setup/seed data.
- Every behavior change should include test updates unless impossible (document why).

## Commit & Pull Request Guidelines

### Conventional commits
- Format: `type(scope): subject` (max header length 100).
- Allowed types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `chore`, `ci`, `build`, `revert`, `security`.

### PR expectations
- Clear description of intent and behavior changes.
- If endpoint contracts change, document impact on known consumers.
- Migration impact explicitly documented when applicable.
- Testing evidence included (`unit`, `e2e`, or manual validation steps).
- Breaking changes and config changes clearly listed.

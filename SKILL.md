---
name: nestjs
description: "Trigger: NestJS archetype, local workflow, project scripts, migrations, PR notes. Complements agent-nestjs-skills with repo-specific workflow."
license: MIT
metadata:
  author: jemanuelp
  version: "1.0.0"
---

# NestJS Archetype Companion

## Activation Contract

Use this skill alongside `agent-nestjs-skills`. Let that root skill provide generic NestJS best practices. Use this companion only for local archetype discovery, workflow commands, migration/tooling checks, compatibility notes, and delivery reporting.

## Hard Rules

- Do not restate generic NestJS best practices covered by `agent-nestjs-skills`.
- Inspect the target project before assuming paths, scripts, ORM, queues, Docker services, or infrastructure.
- Use Doctrine migrations for schema changes in this archetype; do not create or run TypeORM migrations.
- Treat examples below as discovery targets, not required structure.
- Avoid product-specific domains, repositories, service names, and consumers unless the target project provides them.
- When a local convention conflicts with generic best practices, preserve the local convention only if it is already established and safe; otherwise flag the tradeoff.

## Local Archetype Discovery

Check for these project-specific surfaces before editing:

| Area | Expected pattern |
| --- | --- |
| App root | `api/` or root-level NestJS app. |
| Source layout | `src/`, `src/common/`, `src/database/`, `test/`, or project equivalents. |
| Entity registry | Central entity/model list such as `entitiesList`; keep it aligned when adding entities. |
| Messaging | Shared constants, routing keys, queue names, and microservice config helpers. |
| Runtime services | Docker Compose, Makefile, Redis, RabbitMQ, BullMQ, databases, or secrets. |
| Migrations | Doctrine migrations through the project migration scripts, commonly `migrations.sh`. |

## Workflow Commands

Validate commands before using them. Common commands to look for:

```bash
make up
make upb
make logs
make logs api
make stop
make down
make down-v
docker compose ps
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

This archetype uses Doctrine migrations, not TypeORM migrations. Inspect scripts before running. Common project-script patterns:

```bash
./migrations.sh status
./migrations.sh migrate
./migrations.sh execute --up "<VersionClass>"
./migrations.sh generate
```

## Doctrine Migrations

- This archetype uses Doctrine migrations for schema evolution.
- Do not create TypeORM migration files for this archetype.
- Do not run TypeORM migration commands unless the target project explicitly overrides this skill.
- Keep generated Doctrine migration files immutable once applied.
- When schema changes are needed, update NestJS entities/models and the Doctrine migration together.

## Decision Gates

| Situation | Default |
| --- | --- |
| Script is unknown | Read `package.json`, Makefile, compose files, or migration docs first. |
| Infrastructure-dependent tests | Check required services with `docker compose ps` or the project equivalent. |
| Entity/model added | Update the project entity registry if one exists. |
| Queue/event touched | Reuse shared constants and config helpers; do not invent names inline. |
| Contract changed | Document known consumer impact in the PR or final report. |
| Migration changed | Use Doctrine migrations; report command, rollback posture, and whether files are immutable after apply. |

## Execution Steps

1. Load/apply `agent-nestjs-skills` for generic NestJS rules.
2. Inspect project-specific scripts, source layout, Doctrine migration tooling, entity registration, and infrastructure.
3. Apply the smallest change that fits the existing project conventions.
4. Run the smallest relevant validation available: lint, typecheck, unit, e2e, migration status, or documented manual check.
5. If validation is skipped, state the exact reason and what command should be run later.

## Output Contract

Report changed files, project-specific conventions used, validation performed, skipped validation with reason, and any compatibility, config, queue, or migration impact.

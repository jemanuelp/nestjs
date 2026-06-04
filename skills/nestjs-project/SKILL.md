---
name: nestjs-project
description: Contexto de arquitectura para arquetipos NestJS. Usar cuando el trabajo requiera entender estructura, módulos, integraciones o topología de una app NestJS.
---

# NestJS Project Archetype

## Project Context

- This repository provides reusable skills for NestJS project archetypes.
- Treat the target application as a backend service built with NestJS + TypeScript unless the user provides a more specific context.
- Prefer framework-native patterns before introducing custom abstractions.

## Project Overview

The archetype assumes a backend service built primarily with NestJS + TypeScript (`api/`) and commonly backed by relational databases, Redis, message queues, and migration tooling.

## Project Structure

### Root directories
- `api/`: Main NestJS application.
- `migrations/`: PHP Doctrine migration project.
- `docker-compose.yml`: Local stack orchestration.
- `Makefile`: Common local docker lifecycle commands.
- `etc/secrets/`: Docker secrets mounted at runtime.

### `api/` architecture
- `api/src/app/`: Main app module/controller/service wiring.
- `api/src/*` modules: Domain modules (`users`, `chats`, `messages`, `contacts`, `flows`, `sessions`, etc.).
- `api/src/*/controllers`: HTTP and message handlers.
- `api/src/*/services`: Business logic.
- `api/src/*/repositories`: Data access abstractions.
- `api/src/*/entities`: ORM entities.
- `api/src/*/dtos`: Transport and validation DTOs.
- `api/src/common/`: Shared constants, guards, services, interceptors, validators.
- `api/src/database/`: DB provider configuration factory.
- `api/src/logger/`: Logging module and transports.
- `api/test/`: Unit and e2e tests.
- `api/dist/`: Build output (do not edit directly).

## Messaging topology
- RabbitMQ queues/events are centralized in `api/src/common/constants.ts`.
- RMQ microservices are configured via `api/src/microservices.config.ts`.
- Redis transport is used for specific event flows.

## Integration Patterns

### External services
- External integration behavior should depend on runtime configuration and typed clients.
- API calls to upstream services should go through dedicated client abstractions instead of ad-hoc HTTP calls scattered across modules.
- Queue event names and routing keys should be reused from constants, not hardcoded.
- New consumers should use `createMicroserviceConfig(...)` for consistency.
- API contract changes should document compatibility impact for known consumers.

### Infrastructure integrations
- RabbitMQ: primary async backbone for synced/upsert/update message flows.
- Redis: event transport/cache support where applicable.
- JWT/auth and API keys are required for protected operations.

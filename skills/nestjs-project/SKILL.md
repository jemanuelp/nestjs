---
name: whatsapp-bridge-project
description: Contexto del dominio y arquitectura de WhatsApp Bridge. Usar cuando el trabajo requiera entender módulos, integraciones o topología del sistema.
---

# WhatsApp Bridge Project

## Company Context

- This project belongs to **Anticipo**. Assume Anticipo as the default business/organizational context unless explicitly stated otherwise.
- Main endpoint consumer is **CRM** (`https://github.com/World-Tech/crm-front`).
- Additional consumers are **Pusher Agent** (`https://github.com/World-Tech/pushing-agent`) and **N8N agents**.
- Upstream integration: `whatsapp-bridge` consumes the **whatsapp** service API through `@libgot/whatsapp-sdk` (`https://github.com/World-Tech/whatsapp-sdk`).
- RabbitMQ event ownership: **whatsapp** is the producer and **whatsapp-bridge** is the consumer.

## Project Overview

WhatsApp Bridge is a backend service for WhatsApp integration workflows, built primarily with NestJS + TypeScript (`api/`) and backed by relational databases, Redis, RabbitMQ, and Doctrine migrations (`migrations/`).

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

### WhatsApp and external services
- WhatsApp integration behavior depends on runtime configuration (`WHATSAPP_*` env vars).
- API calls to the upstream **whatsapp** service should be made through `@libgot/whatsapp-sdk`.
- Queue event names and routing keys should be reused from constants, not hardcoded.
- New consumers should use `createMicroserviceConfig(...)` for consistency.
- API contract changes should consider compatibility impact first for CRM, then for Pusher Agent and N8N consumers.

### Infrastructure integrations
- RabbitMQ: primary async backbone for synced/upsert/update message flows.
- Redis: event transport/cache support where applicable.
- JWT/auth and API keys are required for protected operations.

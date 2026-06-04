---
name: whatsapp-bridge-standards
description: Estándares de código, seguridad, comunicación y confiabilidad para cambios en WhatsApp Bridge.
---

# WhatsApp Bridge Standards

## Company Context

- This project belongs to **Anticipo**. Apply these standards with Anticipo as the default organizational context.
- Endpoint consumer priority: **CRM** (`https://github.com/World-Tech/crm-front`) is the principal consumer; **Pusher Agent** (`https://github.com/World-Tech/pushing-agent`) and **N8N agents** are also consumers.
- Upstream WhatsApp service integration should use `@libgot/whatsapp-sdk` (`https://github.com/World-Tech/whatsapp-sdk`) for API calls.
- RabbitMQ ownership model: **whatsapp** is producer; **whatsapp-bridge** is consumer.

## Coding Standards & Architecture Patterns

### TypeScript and NestJS conventions
- Keep controllers thin; move logic to services.
- Use DTOs for transport contracts and validation.
- Keep module boundaries explicit and cohesive.
- Prefer repository abstractions over direct persistence usage in controllers.
- Reuse shared types/utilities from `common/` before creating duplicates.
- When creating a new NestJS `Entity`, add it to `api/src/database/entities.ts` in the `entitiesList` array.
- Reuse `@libgot/whatsapp-sdk` abstractions for upstream WhatsApp API integration instead of custom ad-hoc clients.

### Style conventions
- ESLint + Prettier rules are defined in `api/.eslintrc.js` and `api/.prettierrc`.
- Use single quotes, consistent spacing, and existing import/style conventions.
- Avoid introducing new patterns that conflict with current module structure.

### File naming
- Follow existing conventions (`*.controller.ts`, `*.service.ts`, `*.module.ts`, `*.repository.ts`, `*.entity.ts`, `*.dto.ts`).

## Security & Configuration

### Secrets and environment
- Never commit real credentials or private keys.
- Prefer Docker secrets from `etc/secrets/` for sensitive runtime values.
- Keep `.env`, `.env.template`, and `.env.test*` aligned when adding vars.

### Operational safety
- Validate and sanitize all external input.
- Respect authentication and authorization boundaries.
- Avoid logging sensitive values (tokens, private keys, credentials, personal data).
- Preserve backward compatibility in endpoint contracts whenever possible due to downstream CRM/Pusher Agent/N8N dependencies.

## Communication Standards
- User-facing communication from agents should be in Spanish (es_AR) unless requested otherwise.
- Code, identifiers, and technical comments should remain in English.
- Keep responses concise, explicit, and action-oriented.

## Performance & Reliability
- Antes de ejecutar tests del proyecto, verificar que los servicios `postgres-whatsapp-bridge` y `redis` estén activos:
  ```bash
  docker compose ps postgres-whatsapp-bridge redis
  ```
- Prefer batched/queued processing for high-volume message flows.
- Configure queue consumers with appropriate prefetch and DLX settings.
- Handle retries and failure paths explicitly (especially in asynchronous flows).
- Preserve graceful shutdown behavior when touching bootstrap or consumer lifecycle logic.

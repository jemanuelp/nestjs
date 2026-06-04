---
name: nestjs-standards
description: Estándares de código, seguridad, comunicación y confiabilidad para cambios en arquetipos NestJS.
---

# NestJS Standards

## Project Context

- This repository provides reusable standards for NestJS project archetypes.
- Apply these standards with the target project's actual domain and consumers in mind.
- Prefer NestJS-native module boundaries, dependency injection, DTO validation, and test structure.

## Coding Standards & Architecture Patterns

### TypeScript and NestJS conventions
- Keep controllers thin; move logic to services.
- Use DTOs for transport contracts and validation.
- Keep module boundaries explicit and cohesive.
- Prefer repository abstractions over direct persistence usage in controllers.
- Reuse shared types/utilities from `common/` before creating duplicates.
- When creating a new NestJS `Entity`, add it to `api/src/database/entities.ts` in the `entitiesList` array.
- Use dedicated client abstractions for upstream API integration instead of custom ad-hoc clients scattered across modules.

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
- Preserve backward compatibility in endpoint contracts whenever possible for known downstream consumers.

## Communication Standards
- User-facing communication from agents should be in Spanish (es_AR) unless requested otherwise.
- Code, identifiers, and technical comments should remain in English.
- Keep responses concise, explicit, and action-oriented.

## Performance & Reliability
- Antes de ejecutar tests del proyecto, verificar que los servicios requeridos estén activos:
  ```bash
  docker compose ps
  ```
- Prefer batched/queued processing for high-volume message flows.
- Configure queue consumers with appropriate prefetch and DLX settings.
- Handle retries and failure paths explicitly (especially in asynchronous flows).
- Preserve graceful shutdown behavior when touching bootstrap or consumer lifecycle logic.

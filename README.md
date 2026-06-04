# NestJS Skills

Reusable opencode skills for working with NestJS project archetypes. These skills give agents a shared baseline for architecture, development workflow, and coding standards when the target project follows a conventional NestJS backend structure.

## Skills

| Skill | Purpose |
| --- | --- |
| `nestjs-project` | Explains the expected project structure, module layout, messaging topology, and integration patterns. |
| `nestjs-development` | Defines common local workflow, API commands, migration commands, testing expectations, and PR criteria. |
| `nestjs-standards` | Defines coding, security, communication, performance, and reliability standards for NestJS changes. |

## Usage

Install the skill repository with:

```bash
npx skills add jemanuelp/nestjs
```

If you install it manually, register the local `skills` directory in your opencode configuration:

```json
{
  "skills": {
    "paths": ["./skills"]
  }
}
```

After changing skill files or opencode configuration, restart opencode so the updated metadata is loaded.

## Repository Structure

```text
skills/
  nestjs-project/SKILL.md
  nestjs-development/SKILL.md
  nestjs-standards/SKILL.md
```

## Maintenance

- Keep skill names and descriptions generic to NestJS archetypes.
- Avoid product-specific domains, service names, repositories, or consumers.
- Keep commands aligned with real NestJS conventions and verify project-specific scripts before assuming them.
- Prefer concise skill instructions that agents can apply quickly during implementation or review.

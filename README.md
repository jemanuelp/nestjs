# NestJS Skill

Companion root skill for NestJS project archetypes. It complements [`agent-nestjs-skills`](https://github.com/jemanuelp/agent-nestjs-skills) with local workflow, project-shape discovery, migration/tooling checks, and delivery reporting.

## Skill

| Skill | Purpose |
| --- | --- |
| `nestjs` | Complements `agent-nestjs-skills` with local archetype workflow, scripts, migrations, infrastructure checks, and reporting rules. |

## Usage

Install the skill repository with:

```bash
npx skills add jemanuelp/nestjs
```

If you install it manually from a local checkout, register this repository directory in your opencode configuration:

```json
{
  "skills": {
    "paths": ["."]
  }
}
```

After changing skill files or opencode configuration, restart opencode so the updated metadata is loaded.

## Repository Structure

```text
SKILL.md
README.md
```

## Maintenance

- Keep this skill complementary to `agent-nestjs-skills`; do not duplicate generic NestJS rules.
- Avoid product-specific domains, service names, repositories, or consumers.
- Keep commands aligned with real NestJS conventions and verify project-specific scripts before assuming them.
- Prefer concise skill instructions that agents can apply quickly during implementation or review.

# Multica Skills

A curated library of [Claude Code](https://claude.com/claude-code) skills used across our engineering teams. Each skill is a focused `SKILL.md` file that gives Claude the role context, tech-stack conventions, and project-specific rules it needs to produce consistent, on-standard work.

## Overview

Skills are split into two categories:

- **CommonSkills** — Cross-cutting, reusable skills organized by role or stack. These apply to any project.
- **ProjectSkills** — Skills scoped to a specific project, capturing that project's architecture, stack, and conventions.

The `global` skill (in `CommonSkills`) defines baseline engineering rules and is intended to be loaded **alongside** any role-specific or project-specific skill.

## Structure

Each skill lives in its own folder as a `SKILL.md` file.

```
CommonSkills/
├── Global/SKILL.md        # Cross-cutting rules — load with every other skill
├── Angular/SKILL.md       # Angular 18/19 frontend
├── React/SKILL.md         # React frontend
├── Frontend/SKILL.md      # General frontend (HTML/CSS/JS/TS, jQuery, React, Angular)
├── DotNet/SKILL.md        # .NET / ASP.NET Core backend
├── DotNetSql/SKILL.md     # .NET + SQL Server (EF Core, stored procs, tuning)
├── MAUI/SKILL.md          # .NET MAUI mobile
├── QATester/SKILL.md      # QA & testing
└── ProjectLead/SKILL.md   # Planning, review, and release coordination

ProjectSkills/
├── BlueDop/SKILL.md       # BlueDop EHR Web (Angular 19 SPA)
├── CarbPlace/SKILL.md     # Crabplace (32 interconnected web apps)
├── IPSUM/SKILL.md         # IPSUM Live Survey (.NET Framework 4.8)
├── IPSUMMobile/SKILL.md   # Colus/IPSUM (.NET MAUI mobile)
├── OWE/SKILL.md           # ConnectRecycling Job Management (WebForms)
└── ThermPix/SKILL.md      # ThermPix Web (Angular)
```

## Common Skills

| Path | Name | Focus |
|------|------|-------|
| `Global/SKILL.md` | `global` | Coding principles, OWASP security baseline, workflow conventions, token/usage efficiency |
| `Angular/SKILL.md` | `angular` | Angular 18/19, standalone components, Signals, RxJS, Tailwind |
| `React/SKILL.md` | `react` | React, TypeScript, hooks, Context/state management, Tailwind |
| `Frontend/SKILL.md` | `frontend` | HTML, CSS, JS/TS, jQuery, React, Angular |
| `DotNet/SKILL.md` | `dotnet` | ASP.NET Core Web APIs, EF Core, SQL Server, Clean Architecture |
| `DotNetSql/SKILL.md` | `dotnet-sql` | .NET + SQL Server, EF Core, stored procedures, schema changes, performance tuning (SQL MCP) |
| `MAUI/SKILL.md` | `maui` | .NET MAUI, MVVM, SQLite, REST, offline-first |
| `QATester/SKILL.md` | `qa` | Functional, API, regression, smoke testing |
| `ProjectLead/SKILL.md` | `project-lead` | Requirement analysis, task planning, review, Git workflow, release |

## Project Skills

| Path | Name | Stack |
|------|------|-------|
| `BlueDop/SKILL.md` | `bluedop-ehr` | Angular 19 standalone SPA, Tailwind 3, ngx-translate |
| `CarbPlace/SKILL.md` | `crabplace` | HTML/CSS/JS/TS, jQuery, React, Angular (32 apps) |
| `IPSUM/SKILL.md` | `ipsum-project` | .NET Framework 4.8, MVC 5, Web API 2, EF6, SQL Server |
| `IPSUMMobile/SKILL.md` | `colus-mobile` | .NET MAUI, C#, SQLite, REST API |
| `OWE/SKILL.md` | `owe` | .NET Framework 4.8, ASP.NET WebForms, EF6, Telerik UI |
| `ThermPix/SKILL.md` | `thermpix` | Angular, RxJS, SCSS |

## Skill Format

Each skill is a `SKILL.md` file with YAML frontmatter:

```markdown
---
name: <skill-name>
description: <when to use this skill — used by Claude to decide relevance>
---

# <Title>

**Stack**: ...
**Rules**: ...
...
```

- **`name`** — the identifier used to invoke or reference the skill.
- **`description`** — a clear trigger statement telling Claude *when* to load the skill. Project and role skills note that they should be combined with `global-skill`.

## Usage

1. Load `global` for baseline engineering rules.
2. Load the relevant role skill (e.g. `angular`, `dotnet`, `qa`) and/or the project skill (e.g. `bluedop-ehr`, `owe`) for the work at hand.
3. Claude applies the combined conventions when writing, reviewing, or architecting code.

## Adding a New Skill

1. Choose the right location — `CommonSkills/<Name>/` for reusable role/stack skills, `ProjectSkills/<Project>/` for project-specific ones.
2. Create a `SKILL.md` file with the frontmatter above.
3. Write a precise `description` so the skill triggers on the right tasks.
4. Keep the body concise — stack, rules, structure, and any non-obvious conventions.
5. Commit using Conventional Commits (per the `global` skill).

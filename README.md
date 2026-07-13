# Multica Skills

A curated library of [Claude Code](https://claude.com/claude-code) skills used across our engineering teams. Each skill is a small, focused Markdown file that gives Claude the role context, tech-stack conventions, and project-specific rules it needs to produce consistent, on-standard work.

## Overview

Skills are split into two categories:

- **CommonSkills** — Cross-cutting, reusable skills organized by role or stack. These apply to any project.
- **ProjectSkills** — Skills scoped to a specific project, capturing that project's architecture, stack, and conventions.

The `global` skill (in `CommonSkills`) defines baseline engineering rules and is intended to be loaded **alongside** any role-specific or project-specific skill.

## Structure

```
skills/
├── CommonSkills/
│   ├── global-skill.md        # Cross-cutting rules — load with every other skill
│   ├── angular_developer.md   # Angular 18/19 frontend
│   ├── react_developer.md     # React frontend
│   ├── frontend_developer.md  # General frontend (HTML/CSS/JS/TS, jQuery, React, Angular)
│   ├── dotnet_developer.md    # .NET 8 / ASP.NET Core backend
│   ├── maui_developer.md      # .NET MAUI mobile
│   ├── qa_tester.md           # QA & testing
│   └── project_lead.md        # Planning, review, and release coordination
└── ProjectSkills/
    ├── BlueDop/bluedop-ehr.md  # BlueDop EHR Web (Angular 19 SPA)
    ├── CarbPlace/craplace.md   # Crabplace (32 interconnected web apps)
    ├── IPSUM/ipsum.md          # IPSUM Live Survey (.NET Framework 4.8)
    ├── IPSUM/ipsum-mobile.md   # Colus/IPSUM (.NET MAUI mobile)
    ├── OWE/owe.md              # ConnectRecycling Job Management (WebForms)
    └── ThermPix/thermpix.md    # ThermPix Web (Angular 9)
```

## Common Skills

| Skill | Name | Focus |
|-------|------|-------|
| `global-skill.md` | `global` | SOLID/DRY/KISS/YAGNI, OWASP security, Conventional Commits, Git Flow, REST standards, token efficiency |
| `angular_developer.md` | `angular` | Angular 18/19, standalone components, Signals, RxJS, Tailwind |
| `react_developer.md` | `react` | React, TypeScript, hooks, Context/Redux/Zustand, Tailwind |
| `frontend_developer.md` | `frontend` | HTML, CSS, JS/TS, jQuery, React, Angular |
| `dotnet_developer.md` | `dotnet` | .NET 8, ASP.NET Core, EF Core, SQL Server, Clean Architecture |
| `maui_developer.md` | `maui` | .NET MAUI, MVVM, SQLite, REST, offline-first |
| `qa_tester.md` | `qa` | Functional, API, regression, smoke testing (Playwright, Postman) |
| `project_lead.md` | `project-lead` | Requirement analysis, task planning, review, Git workflow, release |

## Project Skills

| Skill | Name | Stack |
|-------|------|-------|
| `BlueDop/bluedop-ehr.md` | `bluedop-ehr` | Angular 19 standalone SPA, Tailwind 3, ngx-translate |
| `CarbPlace/craplace.md` | `crabplace` | HTML/CSS/JS/TS, jQuery, React, Angular (32 apps) |
| `IPSUM/ipsum.md` | `ipsum-project` | .NET Framework 4.8, MVC 5, Web API 2, EF6, SQL Server |
| `IPSUM/ipsum-mobile.md` | `colus-mobile` | .NET MAUI, C#, SQLite, REST API |
| `OWE/owe.md` | `owe` | .NET Framework 4.8, ASP.NET WebForms, EF6, Telerik UI |
| `ThermPix/thermpix.md` | `thermpix` | Angular 9, RxJS, Material, SCSS |

## Skill Format

Each skill is a Markdown file with YAML frontmatter:

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

1. Load `global-skill` for baseline engineering rules.
2. Load the relevant role skill (e.g. `angular`, `dotnet`, `qa`) and/or the project skill (e.g. `bluedop-ehr`, `owe`) for the work at hand.
3. Claude applies the combined conventions when writing, reviewing, or architecting code.

## Adding a New Skill

1. Choose the right location — `CommonSkills/` for reusable role/stack skills, `ProjectSkills/<Project>/` for project-specific ones.
2. Create a Markdown file with the frontmatter above.
3. Write a precise `description` so the skill triggers on the right tasks.
4. Keep the body concise — stack, rules, structure, and any non-obvious conventions.
5. Commit using Conventional Commits (per the `global` skill).

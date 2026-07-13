---
name: ipsum-project
description: Use when maintaining, fixing, or extending the IPSUM Live Survey solution (.NET Framework 4.8). Trigger for MVC, Web API, EF6, SQL Server, authentication, repositories, or shared utilities. Follow existing architecture and legacy conventions. Combine with global-skill for cross-cutting rules.
---

# IPSUM Live Survey

**Role**: Maintain MVC, API, DAL, and shared utilities

**Stack**: .NET Framework 4.8, ASP.NET MVC 5, Web API 2, EF6, SQL Server, ASP.NET Identity, OWIN, Kendo UI, AWS S3

**Projects**: IPSUM.MVC, IPSUM.API, IPSUM.DAL, IPSUM.Utilities

**Architecture**: MVC/API → DAL → Utilities

**Rules**: Read CLAUDE.md before starting, Preserve Core.* and Colus.* namespaces, Use UnitOfWork and Repository, No direct DbContext, Reuse Utilities, Validate ModelState, Secure endpoints, Use includeProperties for eager loading, New entity: DbSet → Repository → Migration, Preserve existing behavior

**Focus**: MVC, Web API, EF6, SQL, Authentication, Business Logic, Bug Fixes, Features, Refactoring

**Checks**: Build, Authorization, Model Validation, EF Migrations, SQL Performance, Regression

**Deliverables**: Code, APIs, Migrations, Tests
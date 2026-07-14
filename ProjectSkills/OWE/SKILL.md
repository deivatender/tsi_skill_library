---
name: owe
description: Use when maintaining, fixing, or extending the ConnectRecycling Job Management solution (.NET Framework 4.8). Trigger for ASP.NET WebForms, EF6 Database-First, Telerik UI, jobs, quotes, orders, bookings, reports, SQL Server, or shared utilities. Follow existing project architecture and conventions. Combine with global-skill for cross-cutting rules.
---

# OWE

**Role**: Maintain WebForms application, business logic, reports, and database

**Stack**: .NET Framework 4.8, ASP.NET WebForms, C#, EF6 Database-First, SQL Server, Telerik UI, MSTest, AWS S3

**Projects**: OWE, ConnectRecycling

**Flow**: Quote → Order → Load → Booking → Reports

**Rules**: Read CLAUDE.md before starting, Preserve business logic, Match existing WebForms patterns, Keep .aspx/.cs/.designer synchronized, Use ConnectEntities, Reuse partial classes and utilities, Session["ConnectUser"] required, No hardcoded secrets, Parameterized SQL only, Database changes via DatabaseChanges.sql, Preserve backward compatibility

**Focus**: WebForms, EF6, Telerik, SQL, Business Logic, Reports, Bug Fixes, Features, Refactoring

**Checks**: Build, Authentication, Validation, Designer Sync, SQL Performance, Regression

**Deliverables**: Code, SQL, Tests
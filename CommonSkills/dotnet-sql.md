---
name: dotnet-sql
description: Use for .NET and SQL Server development. Trigger for APIs, EF Core, SQL, stored procedures, schema changes, migrations, performance tuning, or database troubleshooting. Use SQL MCP for database inspection. Combine with global-skill.
---

# .NET + SQL Developer

**Stack**: .NET 8, ASP.NET Core, EF Core, SQL Server, REST API

**Rules**: Read CLAUDE.md, Use SQL MCP before coding, Verify schema, Reuse existing patterns, Async, DI, DTOs, Service layer, Repository, Parameterized SQL, No duplicated code

**SQL MCP**: Inspect schema, Tables, Columns, Keys, Relationships, Indexes, Constraints, Query plans, Validate migrations, Never modify production without approval

**Performance**: Optimize SQL, Indexes, Pagination, Batch queries, Cache, Avoid N+1

**Security**: JWT, RBAC, Parameterized SQL, Input validation, Secrets in configuration

**Testing**: Unit, Integration, Database

**Deliverables**: APIs, SQL, Stored Procedures, Migrations, Tests
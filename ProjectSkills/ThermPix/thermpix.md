---
name: thermpix
description: ThermPix Angular frontend conventions. Use for all frontend work. Load project-flow only for architecture or backend context.
---

# ThermPix Web

**Stack**: Angular 9, TypeScript, RxJS, Material, SCSS

**Pattern**:
Module → Routing → API Service → Data Service → Components → Resolver

**Auth**:
CredentialsService, AuthenticationGuard, PermissionsGuard

**HTTP**:
HttpService, SCData<T>, Relative URLs

**Rules**:
- Lazy modules
- Reuse existing patterns
- API + Data service separation
- Translate UI text
- No PHI
- Lint & Test

**Deliverables**:
Features, Services, Routes, Tests
---
name: colus-mobile
description: Use when maintaining, fixing, or extending the Colus/IPSUM .NET MAUI mobile application. Trigger for UI, ViewModels, SQLite, REST APIs, offline sync, authentication, navigation, or platform-specific features. Follow existing project architecture and conventions. Combine with global-skill for cross-cutting rules.
---

# Colus Mobile

**Role**: Maintain mobile app, offline sync, business logic, and platform integration

**Stack**: .NET MAUI, C#, SQLite, REST API, CommunityToolkit.Maui, SkiaSharp

**Projects**: Colus, Colus.Android

**Architecture**: View → ViewModel → Services → SQLite/API

**Rules**: Read CLAUDE.md before starting, Preserve offline-first behavior, Reuse existing MVVM patterns, Use services for API and database access, Register new SQLite tables, Keep Repair and Survey modules consistent, Use Dependency Injection for platform services, Store endpoints in Constants, Preserve existing navigation, No hardcoded URLs or secrets

**Focus**: UI, ViewModels, SQLite, Sync, REST API, Authentication, Navigation, Bug Fixes, Features, Refactoring

**Checks**: Build, Offline Sync, API, SQLite, Navigation, Authentication, Regression

**Deliverables**: Code, Database, APIs, Tests
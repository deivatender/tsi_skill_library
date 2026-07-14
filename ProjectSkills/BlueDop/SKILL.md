---
name: bluedop-ehr
description: Use when writing, reviewing, or architecting code in the BlueDop EHR Web frontend (Angular 19 standalone SPA, Tailwind 3, ngx-translate). Trigger for any feature, route, guard, API call, CRUD screen, data table, translation, styling, or BCTF spectral-viewer work in this repo — even small fixes, since it defines mandatory conventions.
---

# BlueDop EHR Web

**Stack**: Angular 19 (standalone components, no NgModules except TranslateModule), TypeScript, Tailwind 3 + @tailwindcss/forms, ngx-translate, Karma/Jasmine

**Commands**: `npm start` (localhost:4200), `npm run build`, `npm run watch`, `npm test` (single spec: fdescribe/fit or Karma --include)

**Structure** (src/app/): core/ (auth, config/api-endpoints.ts, i18n, interceptors), features/ (auth, dashboard, entities, clinics, patients, users, reports, settings, spectral-viewer), layout/main-layout/ (auth shell), shared/ (data-table, crud-layout, confirm-dialog, data-modal, custom-select, phone-wrapper directive), spectral/ (BCTF parsing)

**Auth**: JWT `bluedop_token` + user `bluedop_user` in localStorage; signals (currentUser, accessToken, sessionExpired) + isLoggedIn$; login form-urlencoded grant_type=password; interceptor adds Bearer, 401 → re-auth modal (no redirect); guards: authGuard, superAdminGuard, roleGuard([RoleName]), permissionGuard(['PERM']); access via RoleName enum + ROLE_PERMISSIONS (names, not IDs); roles: Super Admin, Entity Admin, Clinic Admin, Standard User

**Routing**: Authenticated routes under MainLayoutComponent + authGuard; default/wildcard → dashboard; new routes use narrowest guard (permissionGuard first); lazy loadComponent only

**API**: Dev base https://api-dev.bluedop.com; all calls `<apiBaseUrl>/api/v1` + path from API_ENDPOINTS; never hardcode URLs

**BCTF** (src/app/spectral/): bctf-spectral-loader.ts → BctfPackage.load(arrayBuffer) (JSZip); bdsf-decoder.ts (gzip spectral frames); waveform-decoder.ts (gzip A-law); bctf-types.ts (VesselSite, BctfManifest, BctfRecord)

**Shared**: DataTableComponent (server mode if totalRecords >= 0; emits pageChanged/stateChanged/sortChanged; state per stateKey in sessionStorage; date format from bluedop_user_preferences); crud-layout for list pages; reuse shared components before new primitives

**i18n**: src/assets/i18n/<lang>.json (default en); classes I18nService.t(key, params?); templates `{{ 'KEY' | translate }}`; keys bluedop_language, bluedop_user_preferences; changes dispatch bluedop-preferences-updated

**Rules**: Standalone + lazy loadComponent, API_ENDPOINTS only, guard every route (narrowest), role names not IDs, localStorage keys bluedop_*, translate all user-facing strings (every lang file), CRUD pattern list/add/edit/view, readable modular commented code, flag security/perf issues

**Deliverables**: Components, Routes + guards, Services, Translations, Tests
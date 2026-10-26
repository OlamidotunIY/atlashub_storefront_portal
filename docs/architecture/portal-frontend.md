# Storefront Portal — standalone frontend specification

**Status:** Planned separate `atlashub_storefront_portal` repository. It is **not** an app inside `atlashub_storefront`.

## Two workspaces, one account
- **Merchant**: marketplace theme previews/search/filter/buy, license and installation, store settings/forms generated from schema with defaults, assets library, custom domain and TLS, drafts/preview/publish/rollback and installed theme/version/license state. No duplicate product/inventory/order editing from Atlas Web.
- **Creator**: create independent creator profile without a business, view theme version and build status/diagnostics, submit for review, manage listing/unlisting/updates, analytics/sales/earnings. Build source in local Vite/React scaffold via CLI; no drag/drop editor at launch.
- User can be both; choose role, and merchant chooses explicit authorized organization/storefront.
- Atlas Web only contains theme suggestions and deep links into this portal.

## SSO
Same Atlas Hub identity via **separate `atlashub_auth_web`** and Java centralized IdP/OIDC server or proven provider. Portal redirects to central auth when necessary, maintains its own secure app session, validates Java/IdP roles server-side. Auth UI is not copied into Portal or Atlas Web. No browser localStorage token sharing or access token in URLs.

## Backend and SDK
- Portal calls separate TypeScript Storefront Management API from `atlashub_storefront` using versioned contract client, scoped authenticated portal session.
- Java backend is authoritative for organization creation/membership, approved account/user identity, billing/theme licenses, catalog/product/stock/cart/payment and provider policy.
- Storefront SDK for *public website themes* is separate from authenticated portal admin API; never expose merchant admin access in uploaded theme.
- The separate [`atlashub_theme_sdk`](https://github.com/OlamidotunIY/atlashub_theme_sdk) repository now exists with architecture documentation; SDK/CLI/scaffold implementation and npm publishing remain pending.
- Only authorized merchant can install/publish a store; only owning creator uploads/edits theme listing. Server rechecks every action.

## Screens/routes (proposed)
```text
/marketplace
/marketplace/:themeId
/stores
/stores/:id/themes
/stores/:id/settings
/stores/:id/assets
/stores/:id/domains
/stores/:id/preview
/stores/:id/releases
/creator
/creator/themes
/creator/themes/:id/versions
/creator/reviews
/creator/earnings
```

Merchant theme purchase through Atlas Hub **platform billing** (not business's connected payment account). Public customer checkout uses business's eligible connected providers; themes may only initiate trusted checkout.

## State and asset semantics
ThemeRelease immutable and shared; store-specific logo/banner/assets separately stored under authorized tenant namespaces. Validated settings default merge then merchant overrides; previews are nonindexed/isolation bound. Creator updating/unlisting theme does not automatically replace/disable existing installations.

## Project plan
Vite + React + TypeScript, React Router, TanStack Query, form validation, generated management API TypeScript client, accessible shared design system, independent CI/CD. No dependency on Turborepo/Nx. Theme creator portal does **not** run untrusted source build itself; sends build requests to dedicated management backend/worker.

## Review
Auth and business data contracts are planned. No live portal/SSO/pages currently exist; keep document status accurate.

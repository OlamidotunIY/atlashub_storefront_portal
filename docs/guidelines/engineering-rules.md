# Storefront Portal engineering rules

**Status:** Implementation policy. Independent frontend repo only.

## TypeScript/React
Strict TS, React+Vite, React Router, TanStack Query (cache in infrastructure), schema-based parsing of API responses, accessible component library and consistent loading/error/empty states. No `any` for public contracts. No duplicate merchant product/price/inventory state persisted as alternate truth. Route transitions support cancellation and retry safely.

## Security and identity
Shared Auth Web owns registration/login/recovery UI; Java/IdP owns identity/SSO. Portal uses separately validated app session, OIDC Code+PKCE/BFF and allowlisted redirects. **Never** copy tokens through URL/localStorage or assume org selection equals authorization. Creator profile may exist without organization. All merchant requests revalidated by Storefront management API and Java membership/entitlements, including role/permission refresh. Never expose merchant credentials to theme preview or arbitrary developer script.

## Storefront theme contracts
Merchant configurability comes from versioned developer manifest schemas with valid defaults. Build settings forms dynamically, validate overrides, track DRAFT vs published revisions, preserve merchant changes on theme upgrades. Store assets referenced by server-owned Asset IDs, not raw unvalidated URLs. Isolated preview origin, nonindexable URLs and no privileged portal cookies on merchant storefronts.

## Financial/provider boundaries
Theme marketplace purchases use AtlasHub platform billing, confirmed before license activation. Merchants optionally connect their own eligible payment/logistics providers via backend; provider-specific onboarding may require KYC/KYB but global organization registration and storefront setup do not. Public customer checkout is trusted Atlas/JAVA-owned, not a theme form. Never show provisional payment as paid.

## UI quality
Merchant and creator must see separate navigation and permissions under same account, accessible role switching, responsive/mobile, keyboard controls, localization and safe return links from Atlas Web suggestions. Errors reflect actual backend status: UPLOADED != APPROVED, DEPLOYING != PUBLISHED, domain DNS verification != TLS ready.

## Architecture/code review
Feature-based DDD/clean layers with genuine use cases, no massive `services` or `utils` catch-all and no cross-feature import from adapters. PRs require affected contracts, a11y, security/tenant impact, tests and documentation. No Turborepo/Nx or use of Java source classes directly.

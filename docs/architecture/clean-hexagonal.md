# Storefront Portal — DDD, Clean and Hexagonal Architecture

**Status:** Proposed rules. Standalone React/Vite merchant+creator frontend, not an authentication backend, commerce backend or untrusted template renderer.

## Responsibilities and bounded contexts
`MarketplaceDiscovery` (catalog/listing/preview), `ThemeAcquisition` (license/purchase requests), `StorefrontInstallations`, `MerchantCustomization`, `StorefrontAssets`, `DomainProvisioningView`, `StorefrontPublication`, `CreatorProfile`, `CreatorReleases`, `CreatorEarningsView`, `WorkspaceSession`.

Domain is **frontend representation** of intents/value objects (ThemeId, ReleaseId, DomainName, Installability, ConfigField, UploadState) and pure validation/state transitions. The **Node Storefront management API** owns actual theme/publish aggregate invariants, Java owns identity/organization/billing/commerce. UI-local domain models must not become unauthorized alternative sources of truth.

## Layers and direction
- **Domain:** pure TS value objects and format-agnostic policies/settings schema adapters; no React, browser, fetch, query cache.
- **Application:** `FindThemes`, `AcquireTheme`, `InstallTheme`, `UpdateStorefrontConfigDraft`, `RequestDomainVerification`, `PublishStorefront`, `RegisterCreator`, `SubmitThemeVersionReview`, `ReadCreatorEarnings`. Input/output and ports; no direct HTTP or React imports.
- **Ports:** `MarketplaceGateway`, `StorefrontManagementGateway`, `PlatformBillingGateway`, `CreatorGateway`, `SessionGateway`, `AssetUploadGateway`, `NavigationPort`, `TelemetryPort`.
- **Infrastructure:** validated typed HTTP clients to Storefront backend/BFF, query cache, auth redirect adapter, uploader/signed URLs, websocket/polling where available.
- **Presentation:** React pages, view models/hooks, forms generated from developer settings schema, theme previews and dashboards. Calls application use cases, displays actual backend state.
- **Composition root:** wires adapters to ports. Dependency arrows Presentation/Infrastructure → Application → Domain. Never import backend implementation source into frontend.

## Workspace and security invariants
Central **Auth Web** handles user registration/login UI; Java/IdP controls sessions, OIDC/BFF and IAM. Portal has own app session and route authorization. Independent creator requires no organization or mandatory Atlas Anchor/KYB. Merchant operations require verified membership/role in active org; creator ownership ≠ merchant authorization. User can have both identities; a zero-org user can use creator features.

Storefront merchant domain/settings/asset mutations are validated by Node backend and Java IAM; no reliance on hidden buttons for security. Merchant theme purchases via Java *platform billing*; merchant customer checkout via merchant-connected eligible providers (not required to merely create organization, install/configure theme).

## DDD and CQRS pragmatism
Use explicit application use cases and projections for workflows with business meaning. No required `CommandBus` per button click, no separate domain aggregate storage in frontend, no copy of Java implementation. Query cache belongs infrastructure, not domain.

## Cross repo interfaces
Storefront Portal consumes versioned `atlashub_storefront` management API and Java session/permission/billing APIs via approved BFF and/or typed client. Theme developer SDK/CLI lives in separate `atlashub_theme_sdk`. Atlas Web links into Portal; Portal must not duplicate POS, accounting, staff or catalog management UI.

See [folder structure](folder-structure.md) and [engineering rules](../guidelines/engineering-rules.md).

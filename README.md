# Atlas Hub Storefront Portal — Merchant & Creator Frontend

**Status: documentation only.** Standalone React + Vite + TypeScript portal for full theme marketplace, merchant storefront management and independent creator management. **Not** the Atlas Web merchant operations dashboard, Storefront management backend, or theme CLI.

## Product boundaries
- **Merchant workspace:** theme marketplace/recommendations, listing demos, purchase/license/install, draft preview, developer-provided settings and defaults, merchant logo/banner/media assets, connected custom domains and SSL status, publish/rollback/update, storefront and theme license history.
- **Creator workspace:** create/manage own theme listing and version, publish source via CLI from local React/Vite project, submit for review, view diagnostics and sales/earnings, unlist without breaking existing licensed installations. **No drag-and-drop editor** in initial phase.
- The same Atlas Hub identity can be creator and authorized member of multiple organizations. Creator signup **does not require organization registration or banking KYB**. Merchant actions require Java IAM organization/role checks.
- **Auth frontend is separate:** [atlashub_auth_web](https://github.com/OlamidotunIY/atlashub_auth_web). Portal uses shared identity SSO and its own secure app session, rather than duplicate login/password recovery pages. Backend Java/IdP must implement central OIDC/BFF.
- **Management/hosting API is separate:** [atlashub_storefront](https://github.com/OlamidotunIY/atlashub_storefront); product/inventory/orders/checkout truth remains [Java backend](https://github.com/OlamidotunIY/atlashub_backend).
- **Atlas Web** gives storefront suggestions and safe deep links into this portal, not a duplicate full marketplace.
- Merchants connect their own eligible payment/logistics providers through Atlas Hub integrations; neither creator template nor portal needs Atlas-owned banking to publish a store.

## Documentation
- [UI navigation, route definitions, account/merchant/creator contracts](docs/architecture/portal-frontend.md)
- [Pages/modules, API integration, permissions and implementation gates](docs/modules/index.md)

## Technology proposal
Standalone Vite/React/TypeScript (consistent with Atlas Web), React Router, TanStack Query, typed TypeScript clients generated from API schemas. Separate frontend CI/CD from management backend. No Turborepo/Nx or monorepo tooling.

No live portal, payments, Worker deployments or domain provisioning is claimed by this README.


## Required frontend architecture and development standards

This standalone React portal uses **DDD vocabulary for theme/storefront user flows, Clean Architecture for dependency direction and Hexagonal ports/adapters for Node Storefront API and Java IAM/billing**. Domain is pure TypeScript; presentation React does not become an independent business source of truth. CQRS is pragmatic, not one handler per button.

- [Architecture and bounded contexts](docs/architecture/clean-hexagonal.md)
- [Feature folder structure and import rules](docs/architecture/folder-structure.md)
- [Engineering/security rules](docs/guidelines/engineering-rules.md)
- [Testing/CI/release gates](docs/guidelines/testing-release.md)
- [Module backlog](docs/modules/index.md)

No Turborepo/Nx. The portal is not the SDK, the auth UI or the backend; implementation remains pending.


## Shared API contracts (2026-10-26)

[atlashub_contracts](https://github.com/OlamidotunIY/atlashub_contracts) now owns versioned Protobuf cross-repository APIs. This **authenticated Portal** may consume separately packaged management/public schema types via a browser-compatible Connect/gRPC-Web gateway or other deliberately supported HTTP adapter; private Java AI/financial RPCs must not be exposed in the browser. Auth remains separate [Auth Web](https://github.com/OlamidotunIY/atlashub_auth_web) and central OIDC/IdP, not a gRPC login call. Use public/management scoped clients, independent dependency versions and strict server-side org permissions. [Contract classification](https://github.com/OlamidotunIY/atlashub_contracts/blob/main/docs/guidelines/compatibility.md).

# Standalone Storefront Portal — folders and module ownership

**Blueprint, no implementation.** React + Vite + TypeScript + React Router, TanStack Query, typed forms, independently built/deployed, **no Turborepo or Nx**.

```text
atlashub_storefront_portal/
├── docs/
│   ├── architecture/
│   ├── modules/
│   └── guidelines/
├── src/
│   ├── app/
│   │   ├── bootstrap/
│   │   ├── router/                  # public browsing, merchant and creator routes
│   │   ├── layouts/                 # merchant, creator and neutral marketplace shells
│   │   └── composition/             # DI, query providers and API adapters
│   ├── shared/
│   │   ├── domain/                  # IDs, branded types and validation primitives
│   │   ├── application/             # common Result/Error and port contracts
│   │   ├── infrastructure/          # HTTP, SSO redirect, query cache, upload tools
│   │   └── presentation/            # design system, accessible components
│   └── features/
│       ├── identity-session/        # session view, SSO redirects, no login UI
│       ├── workspace-selection/     # creator vs merchant, organization selection
│       ├── marketplace/             # discovery, listing/demo, pricing view
│       ├── acquisitions/            # Java confirmed entitlements and purchases
│       ├── installations/           # merchant theme/store release selection
│       ├── theme-settings/          # schema-generated form, defaults/overrides
│       ├── storefront-assets/       # image/logo upload
│       ├── domains/                 # verify domain + SSL status
│       ├── previews/                # isolated draft preview links
│       ├── publications/            # publish/rollback/update
│       ├── creator-profile/         # register creator w/o organization
│       ├── creator-themes/          # versions, validation/build/review state
│       ├── creator-earnings/        # read-only sales/earnings projections
│       └── support/
├── tests/
│   ├── unit/
│   ├── contract/
│   ├── e2e/
│   └── security/
├── package.json
├── vite.config.ts
└── tsconfig.json
```

**Each meaningful feature** may organize `domain/`, `application/`, `infrastructure/`, `presentation/` folders when size warrants. Do not require four empty directories for one trivial filter/component. If separate feature folder, keep an explicit public entrypoint; disallow cross-feature deep imports into infrastructure or presentation.

## Dependency rules
`domain` has no React, network, router or TanStack imports. `application` relies on domain + ports. `infrastructure` provides implementations; React hooks/presentation depend on application/use-case or view-model facade. `app/composition` wires dependencies; no backend source imports.

## Route examples
`/marketplace`, `/marketplace/:themeId`; merchant `/stores`, `/stores/:id/settings`, `/stores/:id/assets`, `/stores/:id/domains`, `/stores/:id/publish`; creator `/creator/themes`, `/creator/themes/:id/versions`, `/creator/reviews`, `/creator/earnings`. Route checks are UX; server checks are authoritative.

## Non-goals
No drag-drop builder at launch; no login/password form duplicated from Auth Web; no arbitrary React theme source running in Portal; no Java business DB copies.

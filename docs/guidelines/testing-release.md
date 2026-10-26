# Storefront Portal — testing and release rules

**Status:** Planned CI policy, not yet implemented.

## Tests
- Domain/unit: settings schema/default application, domain and URL normalization, navigation allowlisting, role/workspace selection, draft state, money/currency formatting (display only).
- Application with fake ports: theme browsing/install, entitlement failure, cross-org denial, creator-only flows, incomplete publish, disconnected provider checkout unavailable.
- Contract: Storefront Node management API, Java identity/IAM/entitlement and creator earnings events; TypeScript schemas/version handling.
- Component/a11y: logo/color settings forms, image upload, theme listing, preview state, domain/TLS warnings, creator review and sales. Keyboard/mobile/screenreader.
- E2E: signed-in Atlas Web -> Portal via safe theme deep link; central Auth Web SSO; merchant purchases and installs, previews, connects domain and publishes; independent creator without organization uploads candidate through CLI and sees review result.
- Security: forged org/store ID, permission revocation, tampered review status, upload other merchant asset, unsanitized theme descriptions, malicious iframe preview, open redirect, XSS, CSRF, session fixation and cross-origin cookie leakage.
- Release: lint, formatting, TS check, unit/contract/component/e2e, audit, build with Vite and environment allowlists; preview separate from production; rollback and feature gates.

## Compatibility
Pin API schema versions, expose actionable errors and deprecations, no silent deployment of incompatible theme settings. Portal deploy independent from Storefront backend; backwards-compatible APIs/migrations mandatory. Never place API keys in Vite env, never treat fake paid provider status as success.

## Definition of done
Architecture import check, tenant/role tests, accessible interaction, API schema fixture, docs update and observed backend outcome; portal cannot authorize backend business mutation by itself.

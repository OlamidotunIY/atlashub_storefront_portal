# Standalone Storefront Portal frontend modules

**Status:** Planning documentation, not implemented.

| Module | Merchant or creator flow | Required backend |
|---|---|---|
| SSO session/workspace | Existing login auto-auth through central IdP; switch creator/org role | Java identity and Storefront BFF |
| Marketplace | List, search, filter, preview/demo, purchase themes | Storefront catalog + Java billing |
| Installations | Create/select store, install approved theme, pin release | Management API + Java IAM entitlement |
| Merchant configuration | Schema-driven form defaults, logo/banner/color/font/text, asset library | Storefront config/upload APIs |
| Preview/publish | Draft, domain verification, deploy, rollback | Workers/Cloudflare + management API |
| Domains | DNS ownership, certificate status, redirects | Storefront DomainProvisioning |
| Creator registration | User can create profile without business registration | Java User + CreatorProfile store |
| Creator library | Theme versions, source upload build diagnostics, review request, unlist | Storefront management/build API |
| Creator commercial | Sales, earnings, refunds, payout state, licenses | Java platform billing + report projection |
| App navigation | Deep links from Atlas Web recommendations, return links | Allowlisted links + central SSO |

## Acceptance tests
- Creator without organization logs in and manages themes.
- Merchant without provider connection still manages products/theme/store and can preview/publish public catalog, but unavailable payment-dependent checkout features are clearly disabled.
- Merchant switches organization without viewing/editing other merchant's logo/themes/domains.
- Purchased theme appears only after Java-confirmed entitlement, with upgrade/rollback preserving merchant overrides.
- Asset ownership and page preview/deploy fail closed; unknown hostname never resolves wrong merchant.
- Theme portal doesn't duplicate Atlas Web finance/HR/inventory pages.
- No privileged credentials or cookies sent to uploaded theme code, and no auth tokens in cross-origin redirects.


## Required engineering references

Feature implementations follow [DDD/Clean/Hexagonal policy](../architecture/clean-hexagonal.md), [frontend feature folder conventions](../architecture/folder-structure.md), [security/engineering rules](../guidelines/engineering-rules.md) and [testing/release criteria](../guidelines/testing-release.md). Server-owned IAM, entitlements, pricing and provider status cannot be authoritatively decided in React.

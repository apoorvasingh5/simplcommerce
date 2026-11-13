# User admin API has no authentication check on its actions

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000002` |
| Severity | **Critical** |
| Status | open |
| CWE | CWE-862 — Missing Authorization |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

`UserApiController` exposes search, grid, create, update and delete endpoints for users under `/api/users`. It derives from `Controller` and is annotated only by route attributes, so an anonymous client may be able to list, create or delete users, including administrators.

1. **attacker** sends an unauthenticated `POST /api/users/grid` or `DELETE /api/users/{id}`. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:54-56`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L54-L56))
2. **the controller** handles the request with the repository and user manager and no per-action authorization attribute. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:123-150`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L123-L150))
3. **the delete action** marks the user as deleted. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:231-240`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L231-L240))

## Assessment

If the endpoints really are anonymous, an attacker gets full control of the user directory without logging in. The attacker needs only network access to the shop. The impact is account takeover and loss of customer data.

## Recommendation

Require the admin role on the controller.

Fix criteria: Every action of the controller rejects anonymous and non-admin callers.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:14-30`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L14-L30)
- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:54-60`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L54-L60)
- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:231-245`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs#L231-L245)

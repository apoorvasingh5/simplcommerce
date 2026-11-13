# Registration binds request fields into the user, so a new account can claim the admin role

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000001` |
| Severity | **Critical** |
| Status | open |
| CWE | CWE-915 — Improperly Controlled Modification of Dynamically-Determined Object Attributes |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The register action takes a view model from the request body and creates a `User` from it. An attacker who adds extra form fields such as `Roles` or `IsAdmin` can try to have them bound onto the new account and become an administrator without any approval.

1. **attacker** posts `/register` with extra fields next to `Email` and `Password`. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:111-116`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L111-L116))
2. **the action** builds `new User` from the posted model and calls `CreateAsync`. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:120-121`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L120-L121))
3. **the role step** adds the account to a role after creation. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:130`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L130))

## Assessment

Registration is open to anyone, so the attacker needs no account. If extra fields are bound, the result is a full administrator account on the shop, with access to all orders, customers, settings and the admin APIs.

## Recommendation

Bind only the fields the form needs and set roles on the server.

Fix criteria: No field in the registration request can change a role or any privileged property of the new user.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:111-135`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L111-L135)
- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/ViewModels/Account/RegisterViewModel.cs:5-28`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/ViewModels/Account/RegisterViewModel.cs#L5-L28)

# Forgot-password lets anyone test which emails are registered

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000009` |
| Severity | **Medium** |
| Status | open |
| CWE | CWE-204 — Observable Response Discrepancy |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The forgot-password form takes an email address and looks the user up. An attacker who can tell the two outcomes apart learns whether an email has an account, which helps targeted phishing and credential stuffing.

1. **attacker** posts a list of email addresses to the forgot-password endpoint. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:274-280`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L274-L280))
2. **the action** calls `FindByNameAsync` and branches on whether a user was found. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:282-288`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L282-L288))
3. **the response** differs between the known-user and unknown-user branches. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:285-300`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L285-L300))

## Assessment

No account is needed. The attacker gets a yes or no per address. The harm is limited to account discovery, but it feeds later attacks on those accounts. There is no rate limiting on the endpoint.

## Recommendation

Return an identical response for known and unknown addresses.

Fix criteria: The endpoint gives the same page and timing class whether or not the address exists.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:274-303`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L274-L303)

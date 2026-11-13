# SetLanguage redirects to an arbitrary returnUrl

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000008` |
| Severity | **Medium** |
| Status | open |
| CWE | CWE-601 — URL Redirection to Untrusted Site ('Open Redirect') |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The `SetLanguage` action takes `returnUrl` from the request and redirects to it after saving the culture. A crafted form post or link can bounce a user to an attacker-controlled site right after a trusted application action.

1. **attacker** gets a user to submit `POST /Localization/SetLanguage` with `returnUrl=https://evil.example`. ([`src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs:22-24`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs#L22-L24))
2. **the action** saves the culture, then returns `LocalRedirect(returnUrl)` with the user-supplied value. ([`src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs:25-30`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs#L25-L30))

## Assessment

The attacker needs the victim to submit a request to the endpoint, for example through a hidden form on another site. The effect is an open redirect from the shop domain, useful for phishing. There is no data disclosure by itself.

## Recommendation

Validate `returnUrl` before redirecting.

Fix criteria: The action never sends the browser to a host other than the shop's own.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs:20-31`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs#L20-L31)

# Login and registration redirect to an attacker-supplied returnUrl

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000003` |
| Severity | **High** |
| Status | open |
| CWE | CWE-601 — URL Redirection to Untrusted Site ('Open Redirect') |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

An attacker can send a victim a link to the login page with a `returnUrl` that points at an attacker site. After the victim signs in or registers, the application redirects them to that site, which can show a convincing fake session-expired or payment page. The login and register actions pass the query value to `RedirectToLocal`, which ends in a `Redirect` call.

1. **attacker** crafts `/login?returnUrl=https://evil.example/pay` and sends it to a customer. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:62-66`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L62-L66))
2. **the Login action** stores `returnUrl` and, after a successful sign-in, returns `RedirectToLocal(returnUrl)`. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:66-76`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L66-L76))
3. **RedirectToLocal** calls `Redirect(returnUrl)` for the value it was given. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:477-482`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L477-L482))

## Assessment

The attacker needs only a victim who clicks a crafted link and has, or creates, an account. No authentication is needed to send the link. The impact is phishing from a trusted domain after a real login, which can lead to credential theft on a look-alike page.

## Recommendation

Redirect only to local URLs after login and registration.

Fix criteria: A `returnUrl` that is not a local path is ignored and the user lands on the home page.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:62-82`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L62-L82)
- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:111-135`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L111-L135)
- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:477-487`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L477-L487)

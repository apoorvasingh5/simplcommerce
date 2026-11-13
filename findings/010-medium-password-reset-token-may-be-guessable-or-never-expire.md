# Password reset token may be guessable or never expire

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000010` |
| Severity | **Medium** |
| Status | open |
| CWE | CWE-330 — Use of Insufficiently Random Values |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The forgot-password action builds a reset link with a token and emails it. If the token is predictable or has no lifetime, an attacker can compute or reuse one and set a new password for any account.

1. **attacker** requests a reset for a victim's email and collects the token format. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:274-291`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L274-L291))
2. **the action** calls `GeneratePasswordResetTokenAsync` and puts the token in the emailed link. ([`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:291-296`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L291-L296))
3. **the Identity setup** supplies the token providers. ([`src/SimplCommerce.WebHost/Extensions/ServiceCollectionExtensions.cs:140-154`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/SimplCommerce.WebHost/Extensions/ServiceCollectionExtensions.cs#L140-L154))

## Assessment

The attacker needs the victim's email address. If tokens are guessable or long-lived, the attacker resets the password and takes over the account. Impact is account takeover.

## Recommendation

Use unpredictable, short-lived reset tokens.

Fix criteria: A reset token cannot be guessed and stops working after a short time or one use.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:274-303`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs#L274-L303)
- [`src/SimplCommerce.WebHost/Extensions/ServiceCollectionExtensions.cs:138-156`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/SimplCommerce.WebHost/Extensions/ServiceCollectionExtensions.cs#L138-L156)

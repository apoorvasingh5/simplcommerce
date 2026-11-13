# Order details endpoint loads any order by id for any signed-in user

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000004` |
| Severity | **High** |
| Status | open |
| CWE | CWE-639 — Authorization Bypass Through User-Controlled Key |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

`GET /user/orders/{orderId}` looks the order up by its numeric id only. Order ids are sequential, so a signed-in customer can walk the id range and read other customers' names, shipping addresses, items and totals.

1. **attacker** signs in as an ordinary customer and requests `/user/orders/1`, `/user/orders/2`, and so on. ([`src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs:70-73`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs#L70-L73))
2. **the action** queries `_orderRepository` with `x.Id == orderId` and includes the shipping address and customer. ([`src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs:76-83`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs#L76-L83))
3. **the view model** is built from that order and returned. ([`src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs:95-100`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs#L95-L100))

## Assessment

The attacker needs any customer account, which anyone can create through registration. Ids are sequential integers, so enumeration is trivial. The impact is disclosure of other customers' personal data and addresses.

## Recommendation

Bind the lookup to the current customer.

Fix criteria: An order that does not belong to the signed-in user is never returned, whatever its id.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs:70-100`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs#L70-L100)

# Product review title and comment are rendered into the page as HTML

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000007` |
| Severity | **High** |
| Status | open |
| CWE | CWE-79 — Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting') |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

Customers can post a review with a title and a comment. The product reviews list prints both values back into the page. A review that contains a script tag could run in the browser of every visitor who opens the reviews, including administrators who moderate them.

1. **attacker** posts a review whose comment holds `<script>` code. ([`src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml:100-105`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml#L100-L105))
2. **the list view** prints `review.Title` and `review.Comment` for every visitor. ([`src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml:102-105`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml#L102-L105))
3. **the reply loop** prints `reply.Comment` the same way. ([`src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml:112-116`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml#L112-L116))

## Assessment

Any customer can post a review, so the attacker needs a normal account. Script running in an admin session could change prices or create admin users. Impact is session hijack and stored script execution for all viewers.

## Recommendation

Encode review text when it is written to the page.

Fix criteria: Markup in a review is displayed as text and never runs.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml:95-120`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml#L95-L120)

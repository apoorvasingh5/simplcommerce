# Product search builds its database query from the raw search keyword

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000006` |
| Severity | **High** |
| Status | open |
| CWE | CWE-89 — Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The search page takes the `query` parameter and uses it in the product and localized-content queries. An attacker could append SQL to the keyword to read or change data in the shop database, including customer records and password hashes.

1. **attacker** requests `/search?query=x' OR 1=1--`. ([`src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs:55-58`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs#L55-L58))
2. **the action** trims the keyword and uses it to filter localized content properties. ([`src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs:76-88`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs#L76-L88))
3. **the product query** filters name, descriptions and specification with the same keyword. ([`src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs:90-96`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs#L90-L96))

## Assessment

Search is public, so the attacker needs no account. A working injection would allow reading any table. Impact is full database disclosure.

## Recommendation

Use parameterized queries for all search filters.

Fix criteria: No keyword can change the structure of the generated SQL statement.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs:52-100`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs#L52-L100)

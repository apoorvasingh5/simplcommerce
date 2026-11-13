# Claude Security findings — simplcommerce

Exported on 2026-11-13. 10 findings: 2 critical, 5 high, 3 medium, 0 low.

| # | Severity | CWE | Title | Primary location |
|---|---|---|---|---|
| 1 | critical | CWE-915 | [Registration binds request fields into the user, so a new account can claim the admin role](001-critical-registration-binds-request-fields-into-the-user-so-a-new.md) | `src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:120` |
| 2 | critical | CWE-862 | [User admin API has no authentication check on its actions](002-critical-user-admin-api-has-no-authentication-check-on-its-actions.md) | `src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/UserApiController.cs:18` |
| 3 | high | CWE-601 | [Login and registration redirect to an attacker-supplied returnUrl](003-high-login-and-registration-redirect-to-an-attacker-supplied-retur.md) | `src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:477` |
| 4 | high | CWE-639 | [Order details endpoint loads any order by id for any signed-in user](004-high-order-details-endpoint-loads-any-order-by-id-for-any-signed-i.md) | `src/Modules/SimplCommerce.Module.Orders/Areas/Orders/Controllers/OrderController.cs:83` |
| 5 | high | CWE-22 | [Uploaded file name from the multipart header reaches the storage path](005-high-uploaded-file-name-from-the-multipart-header-reaches-the-stor.md) | `src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs:224` |
| 6 | high | CWE-89 | [Product search builds its database query from the raw search keyword](006-high-product-search-builds-its-database-query-from-the-raw-search.md) | `src/Modules/SimplCommerce.Module.Search/Areas/Search/Controllers/SearchController.cs:84` |
| 7 | high | CWE-79 | [Product review title and comment are rendered into the page as HTML](007-high-product-review-title-and-comment-are-rendered-into-the-page-a.md) | `src/Modules/SimplCommerce.Module.Reviews/Areas/Reviews/Views/Review/List.cshtml:102` |
| 8 | medium | CWE-601 | [SetLanguage redirects to an arbitrary returnUrl](008-medium-setlanguage-redirects-to-an-arbitrary-returnurl.md) | `src/Modules/SimplCommerce.Module.Localization/Areas/Localization/Controllers/LocalizationController.cs:30` |
| 9 | medium | CWE-204 | [Forgot-password lets anyone test which emails are registered](009-medium-forgot-password-lets-anyone-test-which-emails-are-registere.md) | `src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:282` |
| 10 | medium | CWE-330 | [Password reset token may be guessable or never expire](010-medium-password-reset-token-may-be-guessable-or-never-expire.md) | `src/Modules/SimplCommerce.Module.Core/Areas/Core/Controllers/AccountController.cs:291` |

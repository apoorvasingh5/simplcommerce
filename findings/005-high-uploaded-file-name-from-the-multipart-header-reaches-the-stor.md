# Uploaded file name from the multipart header reaches the storage path

| Field | Value |
|---|---|
| ID | `find_SC0000000000000000000005` |
| Severity | **High** |
| Status | open |
| CWE | CWE-22 — Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal') |
| Created | 2026-11-13T09:00:00.000000Z |

## Description

The category API takes the file name from the `Content-Disposition` header of an uploaded image and passes it into the local storage service, which joins it onto the `user-content` folder. A name such as `..\..\Themes\x.cshtml` could write outside the media folder and overwrite application files.

1. **attacker** uploads a category thumbnail with a crafted `filename` in the multipart header. ([`src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs:221-223`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs#L221-L223))
2. **SaveFile** parses the header file name and builds the stored name from it. ([`src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs:223-224`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs#L223-L224))
3. **LocalStorageService.SaveMediaAsync** joins the name onto the media folder and writes the stream with `FileMode.Create`. ([`src/Modules/SimplCommerce.Module.StorageLocal/LocalStorageService.cs:18-30`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.StorageLocal/LocalStorageService.cs#L18-L30))

## Assessment

The upload endpoints are reachable by users with the admin or vendor role, so the attacker needs a vendor account. A vendor can be created by an admin, and vendor self-service is a supported feature. A successful write outside `user-content` can overwrite views or static files and lead to code execution on the next request.

## Recommendation

Do not build storage paths from client-supplied file names.

Fix criteria: Whatever file name a client sends, the file is written only inside the media folder.

## Evidence

**https://github.com/apoorvasingh5/simplcommerce**

- [`src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs:219-228`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.Catalog/Areas/Catalog/Controllers/CategoryApiController.cs#L219-L228)
- [`src/Modules/SimplCommerce.Module.StorageLocal/LocalStorageService.cs:16-32`](https://github.com/apoorvasingh5/simplcommerce/blob/f731dfe942d49a7ff761d200ec364baf2c3f141a/src/Modules/SimplCommerce.Module.StorageLocal/LocalStorageService.cs#L16-L32)

---
name: Manage content access and permissions
description: Find content, review who can access it, and grant a user or group a viewer/collaborator role.
api: openapi/posit-connect-openapi-original.json
operations: [getContents, listContentPermissions, addContentPermission, getGroups]
---

# Manage content access and permissions

Use the Posit Connect Server API to audit and adjust access on a content item.

## Auth
```
Authorization: Key <YOUR_API_KEY>
```

## Steps
1. **Find the content** — `GET /v1/content` (`getContents`) to list content, or
   filter by owner/name. Capture the target content `guid`.
2. **Review current access** — `GET /v1/content/{guid}/permissions`
   (`listContentPermissions`) to see every principal (user or group) and its
   role (`viewer` or `owner`/collaborator).
3. **Resolve a group (optional)** — `GET /v1/groups` (`getGroups`) to look up a
   group `guid` when granting access to a team rather than an individual.
4. **Grant access** — `POST /v1/content/{guid}/permissions`
   (`addContentPermission`) with `{ "principal_guid": "<guid>",
   "principal_type": "user"|"group", "role": "viewer"|"owner" }`.

## Conventions & errors
- Principals and content are addressed by `guid`; roles are `viewer` or `owner`.
- Adding a permission is an upsert — re-adding the same principal updates its role.
- Errors return `{ code, error, payload }`; expect `403` if the caller is not a
  content owner, `404` for an unknown guid. See `errors/posit-problem-types.yml`.

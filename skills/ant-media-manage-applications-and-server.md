---
name: ant-media-manage-applications-and-server
description: Authenticate against the Ant Media management panel REST API and create, configure or remove applications and users.
api: Ant Media Management API
generated: '2026-09-02'
method: generated
source: >-
  Grounded in openapi/ant-media-management-api-openapi.yml (harvested verbatim from
  https://antmedia.io/rest/3.1.0-management/swagger.json) and
  https://docs.antmedia.io/guides/developer-sdk-and-api/rest-api-guide/management-rest-apis/.
operations:
  - authenticateUser
  - getApplications
  - createApplication
  - deleteApplication
  - getSettings
  - changeSettings
  - getServerSettings
  - changeServerSettings
  - addUser
  - deleteUser
  - getSystemResourcesInfo
  - getLicenceStatus
---

# Drive the Ant Media management panel from the API

This is a **different API from the application REST API** and it lives at a different base:
`https://{your-server}:5443/rest/` with no `{application}` segment.

## Authenticate — two ways, both documented

**JWT.** Set `server.jwtServerControlEnabled=true` and `server.jwtServerSecretKey=<32+ chars>` in
`conf/red5.properties`, restart, then send the token in a **`ProxyAuthorization`** header — not
`Authorization`, and with no `Bearer ` prefix.

**Session.** `authenticateUser` — `POST /v2/users/authenticate` with
`{"email": "...", "password": "<MD5 hex of the password>"}`. Keep the returned `JSESSIONID`
cookie and send it on every subsequent call. The password goes over the wire as an MD5 digest,
so this must be HTTPS on 5443, never plain HTTP on 5080.

Check where you stand with `isAuthenticatedRest` (`GET /v2/authentication-status`) and `isAdmin`
(`GET /v2/admin-status`).

## Applications

- `getApplications` — `GET /v2/applications` returns `{"applications":["live"]}`.
- `createApplication` — `POST /v2/applications/{appName}`, or
  `createApplication_1` — `PUT /v2/applications/{appName}` when uploading a WAR.
- `deleteApplication` — `DELETE /v2/applications/{appName}`. **Destructive and irreversible** —
  settings and content go with it, and there is no undelete.
- `getAppLiveStreams` — `GET /v2/applications/live-streams/{appname}`.
- `getAppMetricsHistory` — `GET /v2/applications/{appName}/metrics-history`.

## Settings — read before you write

`changeSettings` (`POST /v2/applications/settings/{appname}`) and `changeServerSettings`
(`POST /v2/server-settings`) **replace** configuration. Nothing is versioned and the response does
not return the previous value. If you may need to roll back, call `getSettings` /
`getServerSettings` first and keep the response. That read is the only rollback that exists.

The same applies to `configureSsl` (`POST /v2/ssl-settings`).

## Users

`addUser`, `editUser`, `deleteUser`, `getUserList`, `changeUserPassword`, `getBlockedStatus`.
`addInitialUser` (`POST /v2/users/initial`) works only on a first-run server — check
`isFirstLogin` (`GET /v2/first-login-status`) first.

## Health and licence

`getSystemResourcesInfo`, `getCPUInfo`, `getJVMMemoryInfo`, `getSystemMemoryInfo`,
`getFileSystemInfo`, `getGPUInfo`, `getNetworkStatus`, `liveness`, `getVersion`,
`isEnterpriseEdition`, `getLicenceStatus`.

## Do not

- Do not call `triggerGc`, `getHeapDump` or `setShutdownStatus` as part of a routine loop. They
  act on the running JVM and are not transactional.
- Do not call `resetBroadcast` (`POST /v2/applications/{appname}/reset`) to fix a count. It
  rewrites viewer counts and broadcast statuses in the database with no snapshot of what they were.

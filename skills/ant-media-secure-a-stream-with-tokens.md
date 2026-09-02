---
name: ant-media-secure-a-stream-with-tokens
description: Issue, validate and revoke publish/play tokens and manage subscribers on an Ant Media Server stream.
api: Ant Media Broadcasts API
generated: '2026-09-02'
method: generated
source: >-
  Grounded in openapi/ant-media-broadcasts-api-openapi.yml and
  https://docs.antmedia.io/guides/stream-security/. Every operationId is verbatim from the spec.
operations:
  - getTokenV2
  - getJwtTokenV2
  - validateTokenV2
  - listTokensV2
  - revokeTokensV2
  - addSubscriber
  - getTOTP
  - blockSubscriber
  - deleteSubscriber
  - revokeSubscribers
---

# Secure an Ant Media stream

Stream security is separate from REST authentication. REST auth controls who may call the API;
these operations control who may publish or play the media.

## One-time tokens

- `getTokenV2` — `GET /v2/broadcasts/{id}/token` issues a hash-based token for a stream.
- `getJwtTokenV2` — `GET /v2/broadcasts/{id}/jwt-token` issues a JWT-format token instead.
- `validateTokenV2` — `POST /v2/broadcasts/validate-token` checks one.
- `listTokensV2` — `GET /v2/broadcasts/{id}/tokens/list/{offset}/{size}` — offset and size are
  required path segments.
- `revokeTokensV2` — `DELETE /v2/broadcasts/{id}/tokens` revokes **all** tokens on the stream.
  There is no single-token revoke.

Both issuing operations declare HTTP 400 "When there is an error in creating token" — that is
normally the token/JWT stream security filter being switched off on the application.

## Subscribers and TOTP

`addSubscriber` — `POST /v2/broadcasts/{id}/subscribers`

The spec is explicit about two things worth obeying:

- `type` is `publish` or `play`. A `publish` subscriber can also play, which is what conferencing
  needs; a `play` subscriber can only play.
- If `b32Secret` is unset it falls back to AppSettings. Its length must be a multiple of 8 and it
  must use base32 characters (A–Z, 2–7).

Then `getTOTP` — `GET /v2/broadcasts/{id}/subscribers/{sid}/totp` returns the current code.

## Removing access

- `blockSubscriber` — `PUT /v2/broadcasts/{id}/subscribers/{sid}/block/{seconds}/{type}`. The
  block expires on its own after `{seconds}`; there is no unblock operation.
- `deleteSubscriber` — `DELETE /v2/broadcasts/{id}/subscribers/{sid}` removes one.
- `revokeSubscribers` — `DELETE /v2/broadcasts/{id}/subscribers` removes **all** subscriber data
  for the stream, including its ConnectionEvents. Irreversible.

## Reversibility

Every grant here has a matching revoke and none of them has a deadline. The only one-way doors
are `revokeSubscribers` (deletes the event history with the subscribers) and `revokeTokensV2`
(all-or-nothing).

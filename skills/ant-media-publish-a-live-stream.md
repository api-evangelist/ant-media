---
name: ant-media-publish-a-live-stream
description: Create a broadcast on an Ant Media Server, start it from a stream source, watch its statistics, and stop it cleanly.
api: Ant Media Broadcasts API
generated: '2026-09-02'
method: generated
source: >-
  Grounded in openapi/ant-media-broadcasts-api-openapi.yml (Ant Media Server 3.1.0) and
  https://docs.antmedia.io/guides/developer-sdk-and-api/rest-api-guide/. Every operationId
  below is verbatim from the published spec.
operations:
  - createBroadcast
  - startStreamSourceV2
  - getBroadcast
  - getBroadcastStatistics
  - stopStreamingV2
  - deleteBroadcast
---

# Publish a live stream on Ant Media Server

## Before you start

- Base URL is `https://{your-server}:5443/{application}/rest/` — the application is usually
  `LiveApp`. Ant Media Server is self-hosted; there is no vendor endpoint.
- If the JWT REST API filter is enabled, send `Authorization: Bearer {JWTToken}`. If it is not,
  the server is protected by an IP allow-list instead and your address must be on it.
- **Read `Result.success`, not the HTTP status.** Almost every operation declares only a 200.
  A failure normally arrives as HTTP 200 with `{"success": false, "message": "..."}`.

## 1. Create the broadcast

`createBroadcast` — `POST /v2/broadcasts/create`

Supply your own `streamId`. Doing so is what makes this call retry-safe: a duplicate id returns
HTTP 400 ("stream id is already used in the data store") rather than creating a second stream.

Useful fields on the Broadcast body: `name`, `description`, `streamUrl` (for a pull source),
`listenerHookURL` (a webhook URL for this stream only), `mp4Enabled`, `webRTCViewerLimit`,
`metaData`.

## 2. Start it, if it is a stream source

`startStreamSourceV2` — `POST /v2/broadcasts/{id}/start`

Only needed for pull sources (RTSP/RTMP/HLS/SRT ingest). A publisher pushing over WebRTC or RTMP
starts the stream by connecting.

## 3. Watch it

- `getBroadcast` — `GET /v2/broadcasts/{id}` for state.
- `getBroadcastStatistics` — `GET /v2/broadcasts/{id}/broadcast-statistics` for viewer counts.
- `getAppLiveStatistics` — `GET /v2/broadcasts/active-live-stream-count` for the application total.

Prefer the `liveStreamStarted` / `liveStreamEnded` webhooks over polling — see
`asyncapi/ant-media-webhooks.yml`.

## 4. Stop and clean up

`stopStreamingV2` — `POST /v2/broadcasts/{id}/stop` ends delivery immediately.
`deleteBroadcast` — `DELETE /v2/broadcasts/{id}` removes the record. There is no time limit on
either, and no restore: deletion is permanent.

## Reversibility

| Action | Reversal | Window |
|---|---|---|
| createBroadcast | deleteBroadcast | unbounded |
| startStreamSourceV2 | stopStreamingV2 | any time while live |
| enableRecording(true) | enableRecording(false) | any time — but footage already written stays; remove it with `deleteVoD` |
| deleteBroadcastsBulk | none | irreversible |

## Do not

- Do not call `deleteBroadcastsBulk` to tidy up. There is no undo.
- Do not omit `offset` and `size` when listing — they are required path segments
  (`/v2/broadcasts/list/{offset}/{size}`), not query parameters with defaults.

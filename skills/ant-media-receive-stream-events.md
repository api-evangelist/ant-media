---
name: ant-media-receive-stream-events
description: Register an Ant Media webhook and handle the twelve documented stream lifecycle events.
api: Ant Media Broadcasts API
generated: '2026-09-02'
method: generated
source: >-
  Grounded in https://docs.antmedia.io/guides/advanced-usage/webhooks/ (HTTP 200, 2026-09-02) and
  the Broadcast schema in openapi/ant-media-broadcasts-api-openapi.yml.
operations:
  - createBroadcast
---

# Receive Ant Media stream events

Ant Media POSTs JSON to a URL you register. There is no AsyncAPI document; the catalog lives in
`asyncapi/ant-media-webhooks.yml`.

## Register

**Application-wide:** set the Webhook URL in the application settings in the web panel (or via
`changeSettings` on the management API, in `AppSettings`).

**Per stream:** set `listenerHookURL` on the Broadcast body you pass to `createBroadcast`. This is
the one to use when different streams belong to different tenants.

## What arrives

`POST`, `Content-Type: application/json` by default (switchable to
`application/x-www-form-urlencoded` via the `webhookContentType` advanced setting). Every payload
carries `id` (the streamId), `action` (the event name), and `timestamp` (server milliseconds as a
string). Most also carry `streamName`, `category` and `metadata`.

The twelve events:

| Event | Fires when |
|---|---|
| `liveStreamStarted` | a stream starts |
| `liveStreamEnded` | a stream ends |
| `vodReady` | recording completes — adds `vodId`, `vodName`, `duration`, `app` |
| `endpointFailed` | an RTMP republish fails — `metadata` is the RTMP URL |
| `publishTimeoutError` | no frames received — `metadata` carries `subscriberId` |
| `encoderNotOpenedError` | the encoder could not be opened |
| `playStarted` / `playStopped` | a WebRTC player starts/stops — adds `subscriberId` |
| `subtrackAddedInTheMainTrack` | a participant joined a conference room |
| `subtrackLeftTheMainTrack` | a participant left |
| `firstActiveTrackAddedInMainTrack` | the room went from empty to occupied |
| `noActiveSubtracksLeftInMainTrack` | the room emptied |

## Handle it correctly

- **Answer 200 fast.** The docs are explicit that hooks are called on the event-loop thread.
  Enqueue and return; do not do work inline.
- **Turn retries on.** `webhookRetryCount` ships as `0`, so by default a failed delivery is simply
  lost. Set `webhookRetryCount` and `webhookRetryDelay` (ms) in advanced application settings.
- **There is no signature.** No shared secret, no signing header, no replay protection is
  documented. Treat the URL itself as the credential: make it unguessable, put it behind network
  controls, and re-verify anything that matters by calling `getBroadcast` before acting on it.
- **`metadata` shape shifts.** It is the Broadcast's `metaData` string, but Ant Media parses it
  into a JSON object when it is valid JSON. Handle both.

# Ant Media (ant-media)
Ant Media Server is a scalable, open-source media server for ultra-low latency live streaming and WebRTC-based video applications. It supports WebRTC, RTMP, RTSP, SRT, HLS, and CMAF protocols, enabling developers to build real-time video applications with sub-second latency.

**URL:** [Visit APIs.json](https://raw.githubusercontent.com/api-evangelist/ant-media/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Broadcasting, Live Streaming, Media, Streaming, Video, WebRTC

## Timestamps

- **Created:** 2025-03-01
- **Modified:** 2026-04-19

## APIs

### Ant Media Server REST API
The Ant Media Server REST API provides programmatic access to all streaming server management functions including stream management, broadcast configuration, recording control, token authentication, cluster management, and server settings.

**Human URL:** [https://antmedia.io/rest/](https://antmedia.io/rest/)

#### Tags:

 - Broadcasting, HLS, Live Streaming, Media Server, RTMP, Streaming, Video, WebRTC

#### Properties

- [Documentation](https://antmedia.io/rest/)
- [GettingStarted](https://antmedia.io/docs/guides/getting-started/quick-start/)
- [Authentication](https://antmedia.io/docs/guides/developer-sdk-and-api/rest-api-guide/)
- [GitHubRepository](https://github.com/ant-media/Ant-Media-Server)
- [SDK — JavaScript SDK](https://antmedia.io/docs/guides/developer-sdk-and-api/sdk-integration/)
- [SDK — Android SDK](https://antmedia.io/docs/guides/developer-sdk-and-api/sdk-integration/)
- [SDK — iOS SDK](https://antmedia.io/docs/guides/developer-sdk-and-api/sdk-integration/)

## Common Properties

- [Portal](https://antmedia.io)
- [Documentation](https://antmedia.io/docs/)
- [GettingStarted](https://antmedia.io/docs/guides/getting-started/quick-start/)
- [Pricing](https://antmedia.io/pricing/)
- [Blog](https://antmedia.io/blog/)
- [GitHubOrganization](https://github.com/ant-media)
- [GitHubRepository](https://github.com/ant-media/Ant-Media-Server)
- [Support](https://antmedia.io/support/)
- [TermsOfService](https://antmedia.io/terms/)
- [PrivacyPolicy](https://antmedia.io/privacy-policy/)
- [JSONSchema — Broadcast Schema](json-schema/ant-media-broadcast-schema.json)
- [Vocabulary](vocabulary/ant-media-vocabulary.yaml)

## Features

| Name | Description |
|------|-------------|
| Ultra-Low Latency WebRTC Streaming | Achieve sub-500ms latency with WebRTC-based publish and play, enabling real-time interactive video applications. |
| Multi-Protocol Support | Ingest and deliver streams via RTMP, RTSP, SRT, WebRTC, HLS, CMAF, and LL-HLS. |
| Adaptive Bitrate Streaming | Automatically transcode streams to multiple bitrate/resolution ladders based on viewer bandwidth. |
| Video Recording and VoD | Record live streams to MP4 or HLS on local disk or cloud storage. |
| Cluster and Auto-Scaling | Deploy in horizontal cluster mode with auto-scaling on AWS, Azure, GCP, and Alibaba Cloud. |
| REST API Management | Full programmatic control of streams, broadcasts, conferences, and server settings. |

## Use Cases

| Name | Description |
|------|-------------|
| Telehealth and Remote Consultations | Enable HIPAA-compliant real-time video consultations between patients and providers. |
| Live E-Commerce and Auctions | Power interactive live shopping experiences and real-time bidding platforms. |
| E-Learning and Virtual Classrooms | Deliver interactive live lectures, webinars, and virtual classrooms. |
| Gaming and Esports Broadcasting | Broadcast gaming sessions and esports events with RTMP ingest from OBS and HLS/WebRTC delivery. |
| Video Surveillance | Ingest RTSP streams from IP cameras and provide browser-based WebRTC viewing. |

## Integrations

| Name | Description |
|------|-------------|
| OBS Studio | Ingest live streams from OBS Studio via RTMP for broadcast to WebRTC, HLS, or RTSP viewers. |
| AWS / Azure / GCP | Deploy Ant Media Server clusters with auto-scaling on major cloud platforms. |
| Zoom | Integrate Zoom meetings and webinars with Ant Media Server for re-broadcasting to larger audiences. |

## Artifacts

Machine-readable API specifications organized by format.

### JSON Schema

- [Broadcast Schema](json-schema/ant-media-broadcast-schema.json)

### JSON Structure

- [Broadcast Structure](json-structure/ant-media-broadcast-structure.json)

### JSON-LD

- [Ant Media Context](json-ld/ant-media-context.jsonld)

### Examples

- [Broadcast Example](examples/ant-media-broadcast-example.json)

## Vocabulary

- [Ant Media Vocabulary](vocabulary/ant-media-vocabulary.yaml) — Unified taxonomy mapping 6 resources, 7 actions, and streaming protocol enumerations

## Maintainers

**FN:** Kin Lane

**Email:** info@apievangelist.com

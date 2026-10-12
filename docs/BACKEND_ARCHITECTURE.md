# AnomaEYE — Backend Architecture & API Specification

### High-Throughput Incident Pipeline, Multi-Channel Alert Dispatch & Audit Engine

- **Author:** Muhammad Husnain Aslam ([@heyhusn](https://github.com/heyhusn)) · Backend Engineer
- **Project:** [AnomaEYE](https://www.anomaeye.com) — AI-Powered Real-Time Video Anomaly Detection
- **Submission:** 4th International AI Championship (AI Showdown), AIEF · Category: AI in Urban Tech
- **Core Stack:** Python 3.11 · FastAPI · Google Cloud Storage · Firebase (Auth & Firestore) · Redis (Upstash) · MediaMTX

---

## 1. Overview & System Responsibilities

The **AnomaEYE Backend** serves as the central orchestration backbone connecting edge camera agents, the STAGE/Qwen-VL GPU inference pipeline, IoT sirens, and user interfaces (Flutter mobile app and React web dashboard).

```text
 ┌────────────────┐          ┌───────────────────────┐          ┌─────────────────────┐
 │ Edge Camera    │ RTSP/SRT │ MediaMTX Server       │ WebRTC   │ Flutter App &       │
 │ Agent (Python) ├─────────►│ 5-Min Rolling Buffer  ├─────────►│ React Dashboard     │
 └────────────────┘          └──────────┬────────────┘          └─────────────────────┘
                                        │
                                        │ Stream Ingest
                                        ▼
                             ┌───────────────────────┐
                             │ GPU Inference Service │
                             │ Scene Gate + Verifier │
                             └──────────┬────────────┘
                                        │ Verified Event (POST /events/ingest)
                                        ▼
                             ┌───────────────────────┐
                             │   FastAPI Backend     │
                             │   Incident Engine     │
                             └──────────┬────────────┘
         ┌──────────────────────────────┼──────────────────────────────┐
         ▼                              ▼                              ▼
┌──────────────────┐           ┌──────────────────┐          ┌──────────────────┐
│ Cloud Storage    │           │ Cloud Firestore  │          │ Alert Dispatcher │
│ Evidence Clips   │           │ Multi-Tenant DB  │          │ FCM, SMS, Resend │
│ Expiring URLs    │           │ Append-Only Log  │          │ IoT Siren Relay  │
└──────────────────┘           └──────────────────┘          └──────────────────┘
```

### Core Backend Responsibilities:
1. **Event Ingestion & Deduplication:** Ingest candidate events verified by the vision-language verifier with guaranteed idempotency.
2. **Evidence Packaging:** Coordinate keyframe snapshot and video snippet storage in Google Cloud Storage with short-lived signed URLs.
3. **Multi-Channel Alert Fan-Out:** Fan out instant notifications across Firebase Cloud Messaging (FCM), Twilio (SMS), Resend (Email), and local ESP32 hardware sirens.
4. **State Machine & Lifecycle:** Manage incident state transitions (`verified` ➔ `alerted` ➔ `acknowledged` ➔ `escalated` ➔ `resolved`).
5. **Tamper-Evident Audit Logging:** Enforce an immutable, append-only log of every model verdict, alert dispatch, and human response.

---

## 2. Data Models & Schemas

The backend uses strictly validated Pydantic models for request parsing and serialization.

### 2.1 Ingestion Payload (`InferenceEventPayload`)
Delivered by the GPU inference service when an anomaly exceeds calibrated hysteresis thresholds and passes vision-language verification:

```json
{
  "event_id": "evt_01HZX8N8M9KQ294PZ8K5XYZ123",
  "camera_id": "cam_lhr_mall_04",
  "organization_id": "org_umt_security",
  "timestamp": "2026-10-24T14:32:10.500Z",
  "duration_seconds": 6.5,
  "confidence_score": 0.942,
  "crime_group": "violence",
  "crime_class": "assault",
  "verifier_status": "confirmed",
  "verifier_summary": "Two individuals observed in physical altercation near south parking entrance.",
  "keyframe_base64": "/9j/4AAQSkZJRgABAQ...",
  "clip_buffer_reference": "mtx_stream_cam_lhr_mall_04_20261024_143204_143210.mp4"
}
```

### 2.2 Incident Record (`IncidentDocument`)
Stored in Google Cloud Firestore under `/organizations/{org_id}/incidents/{incident_id}`:

```json
{
  "incident_id": "inc_984128",
  "organization_id": "org_umt_security",
  "camera": {
    "camera_id": "cam_lhr_mall_04",
    "name": "Gate 3 Outer Boulevard",
    "zone": "North Perimeter",
    "rtsp_endpoint": "rtsp://edge-agent-01:8554/cam04"
  },
  "detection": {
    "crime_group": "violence",
    "crime_class": "assault",
    "confidence": 0.94,
    "explanation": "Two individuals observed in physical altercation near south parking entrance.",
    "started_at": "2026-10-24T14:32:04Z",
    "ended_at": "2026-10-24T14:32:10Z"
  },
  "evidence": {
    "keyframe_url": "https://storage.googleapis.com/anomaeye-evidence/org_umt/keyframe_inc_984128.jpg",
    "clip_url": "https://storage.googleapis.com/anomaeye-evidence/org_umt/clip_inc_984128.mp4",
    "retention_expires_at": "2026-11-24T14:32:10Z"
  },
  "status": "alerted",
  "severity": "high",
  "history": [
    {
      "status": "verified",
      "updated_at": "2026-10-24T14:32:11.200Z",
      "actor": "system:qwen3_vl_verifier"
    },
    {
      "status": "alerted",
      "updated_at": "2026-10-24T14:32:11.850Z",
      "actor": "system:dispatcher",
      "channels": ["fcm_push", "sms", "siren"]
    }
  ]
}
```

---

## 3. Core REST & WebSocket API Specification

### 3.1 Endpoint Summary

| Method | Path | Access | Description |
|---|---|---|---|
| `POST` | `/api/v1/events/ingest` | Service-to-Service (API Key) | Ingest candidate detections from GPU verifier |
| `GET` | `/api/v1/incidents` | Authenticated (JWT) | List recent incidents with filtering & pagination |
| `GET` | `/api/v1/incidents/{id}` | Authenticated (JWT) | Retrieve single incident details with signed media links |
| `PATCH` | `/api/v1/incidents/{id}/status` | Authenticated (Guard/Admin) | Update incident state (`acknowledged`, `escalated`, `resolved`) |
| `GET` | `/api/v1/cameras` | Authenticated (JWT) | List registered camera nodes and stream statuses |
| `POST` | `/api/v1/alerts/siren/trigger` | Admin / Dispatcher | Manually or rule-trigger on-site ESP32 relay siren |
| `GET` | `/api/v1/audit/logs` | Admin (JWT) | Query append-only audit trail with hash integrity check |
| `WS` | `/api/v1/ws/live-incidents` | Authenticated (JWT) | Real-time WebSocket stream for web dashboard & mobile |

---

### 3.2 Key Endpoint Details

#### `POST /api/v1/events/ingest`
- **Headers:** `X-Worker-Signature: <hmac-sha256>`, `Content-Type: application/json`
- **Response:** `201 Created`
```json
{
  "success": true,
  "incident_id": "inc_984128",
  "alert_dispatched": true,
  "channels": ["fcm", "sms", "siren"]
}
```

#### `PATCH /api/v1/incidents/{incident_id}/status`
- **Request:**
```json
{
  "new_status": "acknowledged",
  "notes": "Guard patrol unit 2 dispatched to Gate 3."
}
```
- **Response:** `200 OK`
```json
{
  "incident_id": "inc_984128",
  "status": "acknowledged",
  "updated_at": "2026-10-24T14:32:38Z",
  "actor": "guard:m_husnain"
}
```

---

## 4. Multi-Channel Alert Fan-Out Architecture

When an incident is classified as high or critical severity, AnomaEYE executes concurrent fan-out across 4 distinct channels:

```text
                     ┌──────────────────────────────┐
                     │   Incident Ingestion Event   │
                     └──────────────┬───────────────┘
                                    │
                                    ▼
                     ┌──────────────────────────────┐
                     │ Severity & Rules Evaluator   │
                     └──────────────┬───────────────┘
                                    │
     ┌──────────────────┬───────────┴──────────┬──────────────────┐
     ▼                  ▼                      ▼                  ▼
┌──────────────┐ ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Flutter App  │ │ Twilio SMS   │     │ Resend Email │     │ IoT Hardware │
│ FCM Push     │ │ Critical     │     │ Daily Audit  │     │ Siren Relay  │
│ (< 1.2 sec)  │ │ Fallback     │     │ & Incident   │     │ (ESP32 MQTT) │
└──────────────┘ └──────────────┘     └──────────────┘     └──────────────┘
```

1. **Mobile Push (FCM):** High-priority data message containing the keyframe image, incident type, and deep link to the incident video stream.
2. **SMS (Twilio):** Fallback notification sent when security personnel do not acknowledge within 30 seconds or when mobile data is unavailable.
3. **Email (Resend):** Sends instant structured report with keyframe attachment to site administrators and safety supervisors.
4. **Site Siren (ESP32):** Triggers an on-site 12V 110dB siren via an ESP32 microcontroller listening over local encrypted MQTT or authenticated webhook for immediate deterrence of violent attacks.

---

## 5. Security, Tenancy & Privacy by Design

- **Strict Multi-Tenancy:** All Firestore queries, storage buckets, and alert queues are segregated by `organization_id`.
- **Zero Face Recognition:** Video streams are analyzed purely for temporal behavioral anomalies and motion dynamics. No biometric, facial, or identity matching is performed.
- **Expiring Media URLs:** Media clips and keyframes in Cloud Storage are never public. Access is granted solely through HMAC-signed URLs with a 15-minute time-to-live (TTL).
- **5-Minute Rolling Buffer:** Raw camera footage is buffered in memory/ephemeral storage on MediaMTX and automatically discarded after 5 minutes unless tagged as part of a verified incident.
- **Append-Only Audit Trail:** Every status update, alert dispatch, and human decision is logged with cryptographic event chaining (hash of previous entry) to guarantee auditability for law enforcement or administrative reviews.

---

## 6. Implementation & Local Development

### Requirements
- Python 3.11+
- Redis (or Upstash Redis)
- Firebase Admin SDK credentials (`service-account.json`)

```bash
# Setup virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install fastapi uvicorn pydantic firebase-admin twilio resend redis

# Run dev server
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

---

*Document maintained by Team AnomaEYE. For architecture questions, open an issue or reach out to Muhammad Husnain Aslam ([@heyhusn](https://github.com/heyhusn)).*

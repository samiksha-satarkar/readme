# readme

## Architecture

```mermaid
flowchart LR
    Browser[React Dashboard]
    API[API Server]
    Worker[Background Worker]
    DB[(PostgreSQL)]
    BSE[Mock BSE /getTrades]
    SSE[SSE Event Stream]

    Browser -->|GET /api/trades| API
    Browser -->|POST /api/pulls 202| API
    Browser -->|GET /api/events| SSE
    API -->|create job| DB
    API -->|enqueue, do not await| Worker
    Worker -->|GET /getTrades| BSE
    Worker -->|upsert trades| DB
    Worker --> SSE
    SSE --> Browser
    API --> DB

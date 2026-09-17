# AI Photographer Helper API

Base URL: `https://api.aiphotographer.example/v1`

Authenticate every request with a Bearer token:

```http
Authorization: Bearer <your_api_token>
```

## Endpoints

### Analyze a photo

`POST /analyze`

Upload or reference an image and receive quality scores, scene tags, and issue flags.

```bash
curl -X POST https://api.aiphotographer.example/v1/analyze \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://cdn.example.com/shoots/portrait-01.jpg",
    "options": { "include_faces": true }
  }'
```

**Response (200)**

```json
{
  "id": "9f2c1a4e-7b8d-4c21-9a11-0e5f6d8a1b2c",
  "scores": {
    "overall": 82,
    "sharpness": 88,
    "exposure": 74,
    "composition": 80
  },
  "scene_tags": ["portrait", "outdoor", "golden-hour"],
  "issues": [
    {
      "code": "soft_focus_background",
      "message": "Subject is sharp; background blur is uneven on the left edge.",
      "severity": "info"
    }
  ]
}
```

### Composition suggestions

`POST /suggest/composition`

Returns framing, rule-of-thirds, leading-line, and crop recommendations.

### Lighting suggestions

`POST /suggest/lighting`

Recommends exposure, white-balance, and lighting setup changes.

### List presets

`GET /presets?category=portrait`

Returns creative presets filtered by category (`portrait`, `landscape`, `night`, `product`, `street`, `all`).

### Sessions

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/sessions` | Create a shoot session |
| `GET` | `/sessions` | List sessions (`limit`, `cursor`) |
| `GET` | `/sessions/{sessionId}` | Get one session |
| `DELETE` | `/sessions/{sessionId}` | Delete a session |

## Errors

All errors use a consistent envelope:

```json
{
  "error": {
    "code": "invalid_request",
    "message": "image_url is required",
    "request_id": "req_01HXYZ..."
  }
}
```

| Status | Meaning |
|--------|---------|
| `400` | Invalid request body or parameters |
| `401` | Missing or invalid Bearer token |
| `404` | Resource not found |
| `413` | Image payload too large |

## Spec

Machine-readable OpenAPI 3.0 definition: [`openapi.yaml`](./openapi.yaml)

Interactive HTML reference: [`api.html`](./api.html)

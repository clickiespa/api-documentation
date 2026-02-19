# Time Patterns

## Endpoints
- [Create or get pattern](#create-or-get-pattern)
- [Get pattern by ID](#get-pattern-by-id)

Reusable schedules for monitor triggers.

## Create or get pattern
Creates a time pattern if it does not exist and returns its normalized representation.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /time_patterns
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `hours` | No | string | No | Comma-separated list of hours in 24h format |
| `dayweek` | No | string | No | Comma-separated list of weekday numbers (1–7) |
| `daymonth` | No | string | No | Comma-separated list of day-of-month values |
| `months` | No | string | No | Comma-separated list of month numbers (1–12) |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  "/time_patterns?hours=0,12&dayweek=1,5"
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_time_pattern": 1,
    "pattern_name": "0,12|1,5|*|*",
    "pattern_hours": "0,12",
    "pattern_days_of_week": "1,5",
    "pattern_days_of_month": null,
    "pattern_months": null
  },
  "context": {},
  "instance": "/time_patterns"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Time pattern could not be processed",
  "data": {},
  "context": {
    "errors": [
      "Parameter hours must contain numeric values"
    ]
  },
  "instance": "/time_patterns"
}
```

### Status codes
- `200` — Time pattern retrieved successfully.
- `201` — Time pattern was created during the request.
- `400` — One or more query parameters are invalid.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Requested time pattern could not be resolved.
- `500` — Internal server error.

## Get pattern by ID
Retrieves a single pattern by its identifier.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /time_patterns/{id_time_pattern}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_time_pattern` | Yes | int | Numeric identifier of the pattern |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  /time_patterns/1
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_time_pattern": 1,
    "pattern_name": "0,12|1,5|*|*",
    "pattern_hours": "0,12",
    "pattern_days_of_week": "1,5",
    "pattern_days_of_month": null,
    "pattern_months": null
  },
  "context": {},
  "instance": "/time_patterns/1"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Time pattern not found",
  "data": {},
  "context": {
    "errors": [
      "Time pattern 1 does not exist"
    ]
  },
  "instance": "/time_patterns/1"
}
```

### Status codes
- `200` — Time pattern retrieved successfully.
- `400` — Invalid identifier format.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Time pattern was not found.
- `500` — Internal server error.


# Time Zones

## Endpoints
- [List time zones](#list-time-zones)

Retrieve time zones supported by the API.

## List time zones
Returns the list of supported time zones, optionally including UTC offsets.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /time_zones
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_time_zone` | No | int | No | Filters the response by a specific time zone identifier |
| `offsets` | No | bool | `false` | When `true`, includes UTC offset records |

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
  "/time_zones?offsets=true"
```

### Sample response
```json
{
  "status": "success",
  "message": "Time zones retrieved",
  "data": [
    {
      "id_time_zone": 1,
      "time_zone": "UTC"
    },
    {
      "id_time_zone": 2,
      "id_offset": 8,
      "time_zone": "America/New_York",
      "offset": "-05:00",
      "offset_start": "2023-11-05T07:00:00Z",
      "offset_end": "2024-03-10T07:00:00Z",
      "offset_start_utc": "2023-11-05T07:00:00Z",
      "offset_end_utc": "2024-03-10T07:00:00Z"
    }
  ],
  "context": {},
  "instance": "/time_zones"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Time zones not available",
  "data": {},
  "context": {
    "errors": [
      "The requested time zone does not exist"
    ]
  },
  "instance": "/time_zones"
}
```

### Status codes
- `200` — Time zones retrieved successfully.
- `400` — Invalid request parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Time zones were not found.
- `500` — Internal server error.


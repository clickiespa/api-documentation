# Methods

## Endpoints
- [Aggregation methods](#aggregation-methods)
- [Interpolation methods](#interpolation-methods)

Reference data for aggregation and interpolation methods.

## Aggregation methods
Retrieves the catalog of aggregation functions that can be assigned to metrics and units of measure.

Clearance level 6 or lower is required to use this endpoint.

### Endpoint
```
GET /methods/aggregation
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

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
  /methods/aggregation
```

### Sample response
```json
{
  "status": "success",
  "message": "Methods retrieved",
  "data": [
    {
      "id_aggregation": 1,
      "aggregation_name": "avg",
      "aggregation_description": "Average of values",
      "aggregation_function": "AVG"
    }
  ],
  "context": {},
  "instance": "/methods/aggregation"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Aggregation methods not available",
  "data": {},
  "context": {
    "errors": [
      "Supported aggregations are temporarily unavailable"
    ]
  },
  "instance": "/methods/aggregation"
}
```

### Status codes
- `200` — Aggregation methods retrieved successfully.
- `400` — Invalid request configuration.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Aggregation methods were not found.
- `500` — Internal server error.

## Interpolation methods
Retrieves the catalog of interpolation strategies that can be applied when filling missing data points.

Clearance level 6 or lower is required to use this endpoint.

### Endpoint
```
GET /methods/interpolation
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

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
  /methods/interpolation
```

### Sample response
```json
{
  "status": "success",
  "message": "Methods retrieved",
  "data": [
    {
      "id_interpolation": 1,
      "interpolation_name": "linear",
      "interpolation_description": "Linearly interpolates between the closest samples",
      "interpolation_function": "LINEAR"
    }
  ],
  "context": {},
  "instance": "/methods/interpolation"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Interpolation methods not available",
  "data": {},
  "context": {
    "errors": [
      "Supported interpolations are temporarily unavailable"
    ]
  },
  "instance": "/methods/interpolation"
}
```

### Status codes
- `200` — Interpolation methods retrieved successfully.
- `400` — Invalid request configuration.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Interpolation methods were not found.
- `500` — Internal server error.


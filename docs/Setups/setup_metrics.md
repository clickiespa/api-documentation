# Setup Metrics

Associations between setups and the metrics they expose.

## Endpoints
- [Metrics by setup](#metrics-by-setup)
- [Setups by metric](#setups-by-metric)

## Metrics by setup

List every metric attached to the requested setup.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /setups/{id_setup}/metrics
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_setup` | Yes | int | Numeric identifier of the setup whose metric associations you want to inspect. Retrieve it from [Get setup](./setups.md#get-setup). |

### Query parameters

This endpoint does not accept query parameters.

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
  "/setups/3/metrics"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Metrics retrieved",
  "data": [
    {
      "id_setup_metric": 12,
      "id_setup": 3,
      "id_metric": 5,
      "id_alias": null
    }
  ],
  "context": {},
  "instance": "/setups/3/metrics"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_setup_metric` | int | Numeric identifier of the setup-metric relationship. |
| `id_setup` | int | Setup identifier. See [Get setup](./setups.md#get-setup). |
| `id_metric` | int | Metric identifier linked to the setup. Retrieve it from [Get metric](../Metrics_and_data/metrics.md#get-metric). |
| `id_alias` | int | Optional alias identifier used for display names. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup metrics retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to inspect setup metrics. |
| `404` | The referenced setup does not exist. |
| `500` | Unexpected server error. |

### Error response (403)
```json
{
  "status": "error",
  "message": "Insufficient permissions",
  "data": null,
  "context": {
    "headers": {
      "Account": "<ID_ACCOUNT>"
    }
  },
  "instance": "/setups/3/metrics"
}
```

## Setups by metric

List every setup that uses the requested metric.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /metrics/{metric_identifier}/setups
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `metric_identifier` | Yes | string | UUID that identifies the metric whose setup associations you want to inspect. You can obtain it from [List metrics](../Metrics_and_data/metrics.md#list-metrics). |

### Query parameters

This endpoint does not accept query parameters.

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
  "/metrics/<METRIC_UUID>/setups"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Setups retrieved",
  "data": [
    {
      "id_setup_metric": 12,
      "id_setup": 3,
      "id_metric": 5,
      "id_alias": null
    }
  ],
  "context": {},
  "instance": "/metrics/<METRIC_UUID>/setups"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_setup_metric` | int | Numeric identifier of the setup-metric relationship. |
| `id_setup` | int | Setup identifier. See [Get setup](./setups.md#get-setup). |
| `id_metric` | int | Metric identifier linked to the setup. Retrieve it from [Get metric](../Metrics_and_data/metrics.md#get-metric). |
| `id_alias` | int | Optional alias identifier used for display names. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup associations retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to inspect setup metrics. |
| `404` | The referenced metric does not exist. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Metric 00000000-0000-0000-0000-000000000000 was not found",
  "data": null,
  "context": {
    "headers": {
      "Account": "<ID_ACCOUNT>"
    }
  },
  "instance": "/metrics/00000000-0000-0000-0000-000000000000/setups"
}
```

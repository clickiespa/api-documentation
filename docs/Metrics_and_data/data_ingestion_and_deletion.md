# Data ingestion and deletion

## Endpoints
- [Insert datapoints](#insert-datapoints)
- [Delete datapoints](#delete-datapoints)

## Insert datapoints

### Endpoint
```
POST /metrics/{metric_identifier}/datapoints
```

Only metrics created with an **API** source can store readings. Calculated metrics are read-only.

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | Must be `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |

### Request body

| Attribute | Required | Type | Description |
| --- | --- | --- | --- |
| *timestamp* | yes | number | Unix timestamp in seconds as key |
| *value* | yes | number | Numeric reading for that timestamp |

Body example:

```json
{"1610845600": 0.83, "1610845800": 2.81}
```

Legacy clients may wrap readings under a top-level `readings` key (`{"readings": {"1610845600": 0.83}}`). Both formats are accepted.

### Sample request

```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"1610845600": 0.83}' \
  /metrics/<metric_uuid>/datapoints
```

### Sample response

```json
{
  "status": "Data queued for ingestion"
}
```

## Delete datapoints

### Endpoint
```
DELETE /metrics/{metric_identifier}/datapoints
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `ts_from` | yes | int | Start of UTC range (Unix timestamp) |
| `ts_to` | yes | int | End of UTC range (Unix timestamp) |

### Sample request

```bash
curl -X DELETE \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  '/metrics/<metric_uuid>/datapoints?ts_from=1700000000&ts_to=1700003600'
```

### Sample response

```json
{
  "status": "Data deleted successfully.",
  "deletions": 24
}
```

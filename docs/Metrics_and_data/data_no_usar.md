# Data

Endpoints to fetch and manage metric data.

## Fetch metric data

`GET /data/metric`

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| metric | yes | string | Metric UUID or internal ID |
| from | no* | int | Start of UTC range |
| to | no* | int | End of UTC range |
| earliest_recorded | no | bool | Use first recorded sample |
| latest_recorded | no | bool | Use last recorded sample |
| resolution | no | string/int | Aggregation bucket |
| dimension | no | string | Aggregation method |
| interpolation | no | string | Gap filling method |
| filters | no | string | Comma list of filter IDs |
| id_time_zone | no | int | Time zone ID for aggregation |
| context | no | bool | Include request context |

*Requires `from` and `to` or one of the recorded flags.*



Sample request:
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" \
     'https://v4.api.clickie.io/data/metric?metric=temperature&from=1700000000&to=1700003600&resolution=1h'
```

Sample response:
```json
{
  "status": "success",
  "message": "Data fetched",
  "data": {"1700000000": 20.5},
  "context": null,
  "instance": "/data/metric"
}
```
Fields:
- **status** – request outcome
- **message** – explanation
- **data** – timestamp/value mapping
- **context** – details returned when enabled
- **instance** – endpoint path


## Update metric data

`PUT /data/metric/{metric_identifier}`

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| metric_identifier | yes | string | Metric UUID in the path |
| body | yes | object | JSON timestamps and values |

Body example:
```json
{"1610845600": 0.83, "1610845800": 2.81}
```

## Delete metric data

`DELETE /data/metric/{metric_identifier}`

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| metric_identifier | yes | string | Metric UUID in the path |
| ts_from | yes | int | Start of the UTC range (Unix timestamp) |
| ts_to | yes | int | End of the UTC range (Unix timestamp) |

## Evaluate metric formulas

`GET /data/formula`

Use this endpoint to evaluate a calculated metric expression on demand.

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| formula | yes | string | Formula expression to evaluate. |

### Sample request

```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" \
     'https://v4.api.clickie.io/data/formula?formula=sum(%3Cid_metric%3E)%2Bavg(%3Canother_metric%3E)'
```

### Sample response

```json
{
  "status": "success",
  "message": "Formula evaluated",
  "data": {"1700000000": 42.0},
  "context": null,
  "instance": "/data/formula"
}
```

### Edge cases

URL-encode arithmetic operators to avoid evaluation errors when the formula is passed as a query parameter. The most common mappings are:

- `+` → `%2B`
- `/` → `%2F`
- `-` → `%2D`
- `*` → `%2A`

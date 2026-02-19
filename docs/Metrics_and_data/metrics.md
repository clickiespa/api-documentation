# Metrics

## Endpoints
- [List metrics](#list-metrics)
- [Create metric](#create-metric)
- [Get metric](#get-metric)
- [Update metric](#update-metric)
- [Delete metric](#delete-metric)
- [Events](#events)
  - [List events](#list-events)
  - [Get event](#get-event)
  - [Create event](#create-event)
  - [Update event](#update-event)
  - [Delete event](#delete-event)

Metrics store time series data.

## List metrics

Retrieve every metric configured for the authenticated account.

Clearance level 7 or lower with read permission over metrics is required to use this endpoint.

### Endpoint

```
GET /metrics
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination |
| `limit` | no | int | Max records to return |

### Sample request

```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics?skip=0&limit=100
```

### Sample response

```json
{
  "status": "success",
  "message": "Metrics retrieved",
  "data": [{"id_metric": 1, "metric_identifier": "<metric_uuid>", "metric_name": "Temperature"}],
  "context": null,
  "instance": "/metrics"
}
```

## Create metric

Register a new metric for the authenticated account.

Clearance level 5 or lower with create permission over metrics is required to use this endpoint.

### Endpoint

```
POST /metrics
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | Must be `application/json` | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_metric_source` | no | int | 4 | Metric source identifier. System managed; defaults to `4` (API) or switches to `2` when `metric_formula` is provided. |
| `id_uom` | yes | int | No | Unit of measure identifier returned by [`GET /uoms`](../Uoms/uoms.md#list-uoms). |
| `id_aggregation` | no | int | null | Aggregation identifier obtained from [`GET /methods/aggregation`](../Global_resources/methods.md#aggregation-methods). |
| `id_interpolation` | no | int | null | Interpolation identifier obtained from [`GET /methods/interpolation`](../Global_resources/methods.md#interpolation-methods). |
| `metric_name` | yes | string | No | Display name of the metric |
| `metric_description` | no | string | null | Detailed description |
| `metric_tags` | no | string | null | Comma-separated tags |
| `metric_formula` | no | string | null | Metric formula |
| `metric_resolution` | no | int | null | Resolution in seconds |
| `metric_force_availability` | no | bool | false | Force metric availability |
| `metric_impacts_health` | no | bool | true | Metric impacts health |



### Sample request

```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"metric_name": "Humidity", "id_uom": 2}' \
  /metrics
```

### Sample response

```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_metric": 2, "metric_identifier": "<metric_uuid>", "metric_name": "Humidity", "id_metric_source": null},
  "context": null,
  "instance": "/metrics"
}
```

## Get metric

Retrieve a metric by its identifier.

Clearance level 7 or lower with read permission over metrics is required to use this endpoint.

### Endpoint
```
GET /metrics/{metric_identifier}
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

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics/<metric_uuid>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_metric": 2, "metric_identifier": "<metric_uuid>", "metric_name": "Humidity"},
  "context": null,
  "instance": "/metrics/<metric_uuid>"
}
```

## Update metric

Modify an existing metric.

Clearance level 5 or lower with update permission over metrics is required to use this endpoint.

### Endpoint
```
PUT /metrics/{metric_identifier}
```

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

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_uom` | no | int | null | Unit of measure identifier returned by [`GET /uoms`](../Uoms/uoms.md#list-uoms). |
| `id_aggregation` | no | int | null | Aggregation identifier obtained from [`GET /methods/aggregation`](../Global_resources/methods.md#aggregation-methods). |
| `id_interpolation` | no | int | null | Interpolation identifier obtained from [`GET /methods/interpolation`](../Global_resources/methods.md#interpolation-methods). |
| `metric_name` | no | string | null | Display name of the metric |
| `metric_description` | no | string | null | Detailed description |
| `metric_tags` | no | string | null | Comma-separated tags |
| `metric_formula` | no | string | null | Metric formula |
| `metric_resolution` | no | int | null | Resolution in seconds |
| `metric_force_availability` | no | bool | null | Force metric availability |
| `metric_impacts_health` | no | bool | null | Metric impacts health |


### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"metric_name":"New name"}' \
  /metrics/<metric_uuid>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_metric": 2, "metric_identifier": "<metric_uuid>", "metric_name": "New name"},
  "context": null,
  "instance": "/metrics/<metric_uuid>"
}
```

## Delete metric

Archive a metric that is no longer needed.

Clearance level 2 or lower with delete permission over metrics is required to use this endpoint.

### Endpoint
```
DELETE /metrics/{metric_identifier}
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

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics/<metric_uuid>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/metrics/<metric_uuid>"
}
```

## Events

Events annotate metrics with contextual information such as maintenance windows or data anomalies.

## List events

Retrieve every event configured for the authenticated account.

Clearance level 7 or lower with read permission over metric events is required to use this endpoint.

### Endpoint
```
GET /metrics/{metric_identifier}/events
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
| `skip` | no | int | Offset for pagination |
| `limit` | no | int | Max records to return |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics/<metric_uuid>/events?skip=0&limit=100
```

### Sample response
```json
{
  "status": "success",
  "message": "Metric events retrieved",
  "data": [],
  "context": null,
  "instance": "/metrics/<metric_uuid>/events"
}
```

## Get event

Retrieve a single event by its identifier.

Clearance level 7 or lower with read permission over metric events is required to use this endpoint.

### Endpoint
```
GET /metrics/{metric_identifier}/events/{id_metric_event}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |
| `{id_metric_event}` | Event numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics/<metric_uuid>/events/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_metric_event": 3},
  "context": null,
  "instance": "/metrics/<metric_uuid>/events/3"
}
```

## Create event

Register a new event that is associated with a metric.

Clearance level 5 or lower with create permission over metric events is required to use this endpoint.

### Endpoint
```
POST /metrics/{metric_identifier}/events
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_event_type` | no | int | 1 | Event type identifier |
| `event_start` | yes | string or int | No | Start timestamp (ISO 8601 or Unix timestamp) |
| `event_end` | yes | string or int | No | End timestamp (ISO 8601 or Unix timestamp) |
| `event_description` | yes | string | No | Event description |


### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"event_start":"2023-11-14T08:00:00Z","event_end":"2023-11-14T08:15:00Z","event_description":"Maintenance"}' \
  /metrics/<metric_uuid>/events
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_metric_event": 4},
  "context": null,
  "instance": "/metrics/<metric_uuid>/events/4"
}
```

## Update event

Modify an existing event.

Clearance level 5 or lower with update permission over metric events is required to use this endpoint.

### Endpoint
```
PUT /metrics/{metric_identifier}/events/{id_metric_event}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |
| `{id_metric_event}` | Event numeric identifier | int |

### Request body

Any fields from the create table may be updated.

> **Validation**
> Same rules as create: the event type must exist and `event_end` must be after `event_start`.

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"event_description":"Updated"}' \
  /metrics/<metric_uuid>/events/4
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_metric_event": 4},
  "context": null,
  "instance": "/metrics/<metric_uuid>/events/4"
}
```

## Delete event

Remove an event from the account history.

Clearance level 5 or lower with delete permission over metric events is required to use this endpoint.

### Endpoint
```
DELETE /metrics/{metric_identifier}/events/{id_metric_event}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{metric_identifier}` | Metric UUID that identifies the metric | string |
| `{id_metric_event}` | Event numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /metrics/<metric_uuid>/events/4
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/metrics/<metric_uuid>/events/4"
}
```

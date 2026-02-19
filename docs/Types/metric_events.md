# Metric Event Types

## Endpoints
- [List event types](#list-event-types)
- [Create event type](#create-event-type)
- [Get event type](#get-event-type)
- [Update event type](#update-event-type)
- [Delete event type](#delete-event-type)

Catalog of event types that can be associated with metrics.

## List event types

### Endpoint
```
GET /types/metric_events
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `skip` | no | int | `0` | Offset for pagination |
| `limit` | no | int | `100` | Max records to return |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" "/types/metric_events?skip=0&limit=100"
```

### Sample response
```json
{
  "status": "success",
  "message": "Event types retrieved",
  "data": [{"id_event_type": 1, "type_name": "alarm", "type_description": "Alarm events"}],
  "context": null,
  "instance": "/types/metric_events"
}
```

## Create event type

### Endpoint
```
POST /types/metric_events
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `type_name` | yes | string | - | Display name |
| `type_description` | no | string | `null` | Optional details |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"type_name":"alarm","type_description":"Alarm events"}' \
  /types/metric_events
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_event_type": 2, "type_name": "alarm", "type_description": "Alarm events"},
  "context": null,
  "instance": "/types/metric_events"
}
```

## Get event type

### Endpoint
```
GET /types/metric_events/{id_event_type}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_event_type}` | Event type numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/metric_events/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_event_type": 2, "type_name": "alarm", "type_description": "Alarm events"},
  "context": null,
  "instance": "/types/metric_events/2"
}
```

## Update event type

### Endpoint
```
PUT /types/metric_events/{id_event_type}
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
| `{id_event_type}` | Event type numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `type_name` | no | string | `null` | Display name |
| `type_description` | no | string | `null` | Optional details |

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"type_name":"critical","type_description":"Critical events"}' \
  /types/metric_events/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_event_type": 2, "type_name": "critical", "type_description": "Critical events"},
  "context": null,
  "instance": "/types/metric_events/2"
}
```

## Delete event type

### Endpoint
```
DELETE /types/metric_events/{id_event_type}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_event_type}` | Event type numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/metric_events/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/types/metric_events/2"
}
```

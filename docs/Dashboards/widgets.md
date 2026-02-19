# Widgets

Catalog endpoints for reusable widgets. These endpoints are exposed under the `dashboards` tag and manage only the widget catalog, not dashboard widget instances.

## Endpoints
- [List widgets](#list-widgets)
- [Create widget](#create-widget)
- [Get widget](#get-widget)
- [Update widget](#update-widget)
- [Delete widget](#delete-widget)

## List widgets
Retrieve widget catalog entries. Clearance level 7 is required.

### Endpoint
```
GET /widgets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination. Default: 0 |
| `limit` | no | int | Page size. Default: 100 |
| `archived` | no | bool | Filter archived widgets. Default: `false` |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /widgets?skip=0&limit=20
```

### Sample response
```json
{
  "status": "success",
  "message": "Widgets retrieved",
  "data": [
    {
      "id_widget": 1,
      "id_widget_type": 2,
      "id_form": 3,
      "widget_name": "Chart",
      "widget_code": null,
      "widget_description": "Line chart",
      "widget_tags": "metrics,lines",
      "widget_order": 10,
      "widget_archived": false
    }
  ],
  "context": {"query": {"skip": "0", "limit": "20"}},
  "instance": "/widgets"
}
```

## Create widget
Create a widget catalog entry. Clearance level 1 is required.

### Endpoint
```
POST /widgets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_widget_type` | yes | int | — | Widget type identifier |
| `widget_name` | yes | string | — | Widget display name |
| `id_form` | no | int | null | Optional form used to seed dashboard widget configuration |
| `widget_code` | no | string | null | Optional internal code |
| `widget_description` | no | string | null | Widget description |
| `widget_tags` | no | string | null | Comma-separated tags |
| `widget_order` | no | int | 100 | Ordering hint |
| `widget_archived` | no | bool | false | Archive flag |

### Sample request
```
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"id_widget_type":2,"widget_name":"Chart","widget_order":1}' \
  /widgets
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_widget": 5,
    "id_widget_type": 2,
    "id_form": null,
    "widget_name": "Chart",
    "widget_code": null,
    "widget_description": null,
    "widget_tags": null,
    "widget_order": 1,
    "widget_archived": false
  },
  "context": {
    "body": {
      "id_widget_type": 2,
      "widget_name": "Chart",
      "widget_order": 1
    }
  },
  "instance": "/widgets"
}
```

## Get widget
Retrieve a widget by ID. Clearance level 7 is required.

### Endpoint
```
GET /widgets/{id_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_widget` | yes | int | Widget identifier |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /widgets/5
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_widget": 5,
    "id_widget_type": 2,
    "id_form": null,
    "widget_name": "Chart",
    "widget_code": null,
    "widget_description": null,
    "widget_tags": null,
    "widget_order": 1,
    "widget_archived": false
  },
  "context": {"path": {"id_widget": 5}},
  "instance": "/widgets/5"
}
```

## Update widget
Update an existing widget. Clearance level 2 is required.

### Endpoint
```
PUT /widgets/{id_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_widget` | yes | int | Widget identifier |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_widget_type` | no | int | No | Widget type identifier |
| `widget_name` | no | string | No | Widget display name |
| `id_form` | no | int | No | Optional form used to seed dashboard widget configuration |
| `widget_code` | no | string | No | Optional internal code |
| `widget_description` | no | string | No | Widget description |
| `widget_tags` | no | string | No | Comma-separated tags |
| `widget_order` | no | int | No | Ordering hint |
| `widget_archived` | no | bool | No | Archive flag |

### Sample request
```
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"widget_name":"Updated chart","widget_archived":false}' \
  /widgets/5
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_widget": 5,
    "id_widget_type": 2,
    "id_form": null,
    "widget_name": "Updated chart",
    "widget_code": null,
    "widget_description": null,
    "widget_tags": null,
    "widget_order": 1,
    "widget_archived": false
  },
  "context": {
    "path": {"id_widget": 5},
    "body": {"widget_name": "Updated chart", "widget_archived": false}
  },
  "instance": "/widgets/5"
}
```

## Delete widget
Delete a widget. Clearance level 1 is required. Archived widgets cannot be modified.

### Endpoint
```
DELETE /widgets/{id_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_widget` | yes | int | Widget identifier |

### Sample request
```
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /widgets/5
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": {"path": {"id_widget": 5}},
  "instance": "/widgets/5"
}
```

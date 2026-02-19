# Dashboards and dashboard widgets

Visual collections of dashboards and their widget instances.

## Endpoints
- [List dashboards](#list-dashboards)
- [Create dashboard](#create-dashboard)
- [Get dashboard](#get-dashboard)
- [Update dashboard](#update-dashboard)
- [Delete dashboard](#delete-dashboard)
- [List dashboard widgets](#list-dashboard-widgets)
- [Create dashboard widget](#create-dashboard-widget)
- [Get dashboard widget](#get-dashboard-widget)
- [Update dashboard widget](#update-dashboard-widget)
- [Delete dashboard widget](#delete-dashboard-widget)

## List dashboards
Retrieve a list of dashboards. Clearance level 8 is required. Users with clearance levels 1 or 2 can choose any `archived` filter value (`true`, `false`, or omitted). Other clearances filter archived dashboards according to the supplied flag, defaulting to `false` when omitted.

### Endpoint
```
GET /dashboards
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
| `archived` | no | bool | Archive filter. Clearances 1–2 may omit, `true`, or `false`. Other clearances default to `false` when omitted. |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards?skip=0&limit=20
```

### Sample response
```json
{
  "status": "success",
  "message": "Dashboards retrieved",
  "data": [
    {
      "id_dashboard": 1,
      "id_parent_dashboard": null,
      "dashboard_name": "Overview"
    }
  ],
  "context": {"query": {"skip": "0", "limit": "20"}},
  "instance": "/dashboards"
}
```

## Create dashboard
Create a new dashboard. Clearance level 2 is required.

### Endpoint
```
POST /dashboards
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
| `id_icon` | no | int | 2462 | Icon identifier |
| `id_parent_dashboard` | no | int | null | Parent dashboard identifier for nesting |
| `dashboard_name` | yes | string | — | Display name (max 100 characters) |
| `dashboard_description` | no | string | No | Description |
| `dashboard_color` | no | string | No | Hex color code (max 7 characters) |

### Sample request
```
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"dashboard_name":"New","id_icon":1}' \
  /dashboards
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_dashboard": 2, "id_parent_dashboard": null, "dashboard_name": "New"},
  "context": {"body": {"dashboard_name": "New", "id_icon": 1}},
  "instance": "/dashboards"
}
```

## Get dashboard
Retrieve a single dashboard by ID. Clearance level 8 is required.

### Endpoint
```
GET /dashboards/{id_dashboard}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard numeric identifier | int |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_dashboard": 2, "id_parent_dashboard": null, "dashboard_name": "New"},
  "context": {"path": {"id_dashboard": "2"}},
  "instance": "/dashboards/2"
}
```

## Update dashboard
Update an existing dashboard. Clearance level 5 is required.

### Endpoint
```
PUT /dashboards/{id_dashboard}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard numeric identifier | int |

### Request body
| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_icon` | no | int | No | Icon identifier |
| `id_parent_dashboard` | no | int | null | Parent dashboard identifier for nesting (set to `null` to remove) |
| `dashboard_name` | no | string | No | Display name (max 100 characters) |
| `dashboard_description` | no | string | No | Description |
| `dashboard_color` | no | string | No | Hex color code |
| `dashboard_archived` | no | bool | No | Archive flag |

### Sample request
```
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"dashboard_description":"Updated"}' \
  /dashboards/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_dashboard": 2, "id_parent_dashboard": null, "dashboard_name": "New"},
  "context": {"body": {"dashboard_description": "Updated"}, "path": {"id_dashboard": "2"}},
  "instance": "/dashboards/2"
}
```

## Delete dashboard
Delete an existing dashboard. Clearance level 2 is required.

### Endpoint
```
DELETE /dashboards/{id_dashboard}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard numeric identifier | int |

### Sample request
```
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": null,
  "context": {"path": {"id_dashboard": "2"}},
  "instance": "/dashboards/2"
}
```

## List dashboard widgets
List widgets attached to a dashboard (configuration data is omitted). Clearance 7 is required to use this endpoint.

### Endpoint
```
GET /dashboards/{id_dashboard}/widgets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard identifier | int |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards/2/widgets
```

### Sample response
```json
{
  "status": "success",
  "message": "1 elements obtained successfully",
  "data": [
    {
      "id_dashboard_widget": 3,
      "id_dashboard": 2,
      "id_widget": 10,
      "widget_width": 6,
      "widget_height": 4,
      "widget_order": 0
    }
  ],
  "context": {"path": {"id_dashboard": "2"}},
  "instance": "/dashboards/2/widgets"
}
```

## Create dashboard widget
Attach a widget to a dashboard. Configuration is optional because the API seeds defaults, including subform defaults. Clearance 6 is required to use this endpoint.

### Endpoint
```
POST /dashboards/{id_dashboard}/widgets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_widget` | yes | int | — | Widget catalog identifier |
| `widget_width` | no | int | No | Width allocation |
| `widget_height` | no | int | No | Height allocation |
| `widget_order` | no | int | 100 | Ordering position |
| `config_data` | no | object | No | Optional configuration payload that includes a `form_data` object and nested subforms |

When `config_data` is omitted, the API builds default values for the form and at least one subform entry for each nested input.
Fields not defined in the widget form inputs are rejected with a `400` response.

### Sample request
```
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"id_widget":3,"widget_width":6,"widget_height":4,"config_data":{"form_data":{"title":"Line chart","moments":[{"form_data":{"metric":"temperature"}}]}}}' \
  /dashboards/2/widgets
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_dashboard_widget": 3,
    "id_dashboard": 2,
    "id_widget": 3,
    "widget_width": 6,
    "widget_height": 4,
    "widget_order": 0,
    "config_data": {
      "id_form_data": 55,
      "form_data": {
        "title": "Line chart",
        "moments": [
          {
            "id_form_data": 56,
            "form_data": {"metric": "temperature"}
          }
        ]
      }
    }
  },
  "context": {
    "body": {
      "id_widget": 3,
      "widget_width": 6,
      "widget_height": 4
    },
    "path": {"id_dashboard": "2"}
  },
  "instance": "/dashboards/2/widgets"
}
```

## Get dashboard widget
Retrieve a specific widget instance with its configuration. Clearance 7 is required to use this endpoint.

### Endpoint
```
GET /dashboards/{id_dashboard}/widgets/{id_dashboard_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard identifier | int |
| `{id_dashboard_widget}` | Dashboard widget identifier | int |

### Sample request
```
curl -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards/2/widgets/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_dashboard_widget": 3,
    "id_dashboard": 2,
    "id_widget": 3,
    "widget_width": 6,
    "widget_height": 4,
    "widget_order": 0,
    "config_data": {
      "id_form_data": 55,
      "form_data": {
        "title": "Line chart",
        "moments": [
          {
            "id_form_data": 56,
            "form_data": {"metric": "temperature"}
          }
        ]
      }
    }
  },
  "context": {"path": {"id_dashboard": "2", "id_dashboard_widget": "3"}},
  "instance": "/dashboards/2/widgets/3"
}
```

## Update dashboard widget
Update widget placement or configuration. When `config_data` is provided, unspecified `form_data` keys retain their previous values and subform entries can be created, updated, or deleted. Clearance 7 is required to use this endpoint.

### Endpoint
```
PUT /dashboards/{id_dashboard}/widgets/{id_dashboard_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard identifier | int |
| `{id_dashboard_widget}` | Dashboard widget identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_widget` | no | int | No | Widget catalog identifier |
| `widget_width` | no | int | No | Width allocation |
| `widget_height` | no | int | No | Height allocation |
| `widget_order` | no | int | No | Ordering position |
| `config_data` | no | object | No | Configuration payload containing `form_data` plus nested subforms with `id_form_data` for updates |

When updating subforms, include `id_form_data` for existing entries. To delete a subform record, send the entry with `"form_data": null`.
Fields not defined in the widget form inputs are rejected with a `400` response.

### Sample request
```
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"config_data": {"form_data": {"title": "Updated title","moments":[{"id_form_data":56,"form_data":{"metric":"temperature"}}]}}}' \
  /dashboards/2/widgets/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_dashboard_widget": 3,
    "id_dashboard": 2,
    "id_widget": 3,
    "widget_width": 6,
    "widget_height": 4,
    "widget_order": 0,
    "config_data": {
      "id_form_data": 55,
      "form_data": {
        "title": "Updated title",
        "moments": [
          {
            "id_form_data": 56,
            "form_data": {"metric": "temperature"}
          }
        ]
      }
    }
  },
  "context": {
    "body": {"config_data": {"title": "Updated title"}},
    "path": {"id_dashboard": "2", "id_dashboard_widget": "3"}
  },
  "instance": "/dashboards/2/widgets/3"
}
```

## Delete dashboard widget
Remove a widget instance from a dashboard. Clearance 6 is required to use this endpoint.

### Endpoint
```
DELETE /dashboards/{id_dashboard}/widgets/{id_dashboard_widget}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_dashboard}` | Dashboard identifier | int |
| `{id_dashboard_widget}` | Dashboard widget identifier | int |

### Sample request
```
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: 1" /dashboards/2/widgets/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": null,
  "context": {"path": {"id_dashboard": "2", "id_dashboard_widget": "3"}},
  "instance": "/dashboards/2/widgets/3"
}
```

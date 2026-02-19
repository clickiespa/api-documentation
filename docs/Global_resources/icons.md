# Icons

## Endpoints
- [List icons](#list-icons)
- [Get icon](#get-icon)

List of icons available for dashboards and assets.

## List icons
Retrieves the catalog of icon definitions that can be used in dashboards and assets.

This endpoint is currently unavailable to all clearance levels.

### Endpoint
```
GET /icons
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `skip` | No | int | `0` | Offset applied to the result set |
| `limit` | No | int | `50` | Maximum number of records to return |
| `icon_name` | No | string | No | Filters icons by name (partial match) |

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
  "/icons?icon_name=home"
```

### Sample response
```json
{
  "status": "success",
  "message": "Icons retrieved",
  "data": [
    {
      "id_icon": 1,
      "icon_name": "home",
      "icon_tags": "navigation,default"
    }
  ],
  "context": {},
  "instance": "/icons"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Icon list not available",
  "data": {},
  "context": {
    "errors": [
      "The filter provided is invalid"
    ]
  },
  "instance": "/icons"
}
```

### Status codes
- `200` — Icon list retrieved successfully.
- `400` — Validation error in the supplied filters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Icons could not be found.
- `500` — Internal server error.

## Get icon
Retrieves the metadata for a specific icon by its identifier.

This endpoint is currently unavailable to all clearance levels.

### Endpoint
```
GET /icons/{id_icon}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_icon` | Yes | int | Numeric identifier of the icon |

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
  /icons/1
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_icon": 1,
    "icon_name": "home",
    "icon_tags": "navigation,default"
  },
  "context": {},
  "instance": "/icons/1"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Icon not found",
  "data": {},
  "context": {
    "errors": [
      "Icon 1 does not exist"
    ]
  },
  "instance": "/icons/1"
}
```

### Status codes
- `200` — Icon retrieved successfully.
- `400` — Invalid identifier format.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Icon was not found.
- `500` — Internal server error.


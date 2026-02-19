# Asset Categories

## Endpoints
- [List categories](#list-categories)
- [Create category](#create-category)
- [Get category](#get-category)
- [Update category](#update-category)
- [Delete category](#delete-category)

Categories used to group assets.

## List categories

### Endpoint
```
GET /types/assets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `skip` | no | int | `0` | Offset for pagination |
| `limit` | no | int | `100` | Max records to return |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" "/types/assets?skip=0&limit=100"
```

### Sample response
```json
{
  "status": "success",
  "message": "Categories retrieved",
  "data": [{"id_category": 1, "category_name": "Sensors"}],
  "context": null,
  "instance": "/types/assets"
}
```

## Create category

### Endpoint
```
POST /types/assets
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
| `category_name` | yes | string | - | Display name |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"category_name":"Sensors"}' \
  /types/assets
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_category": 2, "category_name": "Sensors"},
  "context": null,
  "instance": "/types/assets"
}
```

## Get category

### Endpoint
```
GET /types/assets/{id_category}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_category}` | Category numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_category": 2, "category_name": "Sensors"},
  "context": null,
  "instance": "/types/assets/2"
}
```

## Update category

### Endpoint
```
PUT /types/assets/{id_category}
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
| `{id_category}` | Category numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `category_name` | no | string | `null` | Display name |

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"category_name":"Other"}' \
  /types/assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_category": 2, "category_name": "Other"},
  "context": null,
  "instance": "/types/assets/2"
}
```

## Delete category

### Endpoint
```
DELETE /types/assets/{id_category}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_category}` | Category numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/types/assets/2"
}
```

# EAV Entities

## Endpoints
- [List entities](#list-entities)
- [Get entity](#get-entity)

Entities used in the attribute-value model. An entity represents an account-specific table or model that can be referenced from dynamic forms and relationships.

## List entities

Retrieve every EAV entity configured for the authenticated account.

Clearance level 2 (Read) or any lower clearance level is required to use this endpoint.

### Endpoint
```
GET /entities
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

This endpoint does not accept query parameters.

### Request body

This endpoint does not accept a request body.

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X GET \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  /entities
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Entities retrieved",
  "data": [
    {
      "id_entity": 1,
      "entity_name": "location"
    }
  ],
  "context": {},
  "instance": "/entities"
}
```

### Response envelope

| Field | Type | Description |
| --- | --- | --- |
| `status` | string | Always `"success"` when the request finishes correctly. |
| `message` | string | Human readable summary of the operation. |
| `data` | array of objects | Collection of entity objects. See [Entity object](#entity-object). |
| `context` | object | Additional metadata supplied by the API. Empty when not required. |
| `instance` | string | Relative path for the requested resource. |

### Entity object

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `id_entity` | int | No | Numeric identifier of the entity. |
| `entity_name` | string | No | Unique name assigned to the entity. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Entities retrieved successfully. |
| `400` | The request could not be processed due to invalid headers or account metadata. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to list entities. |
| `500` | Unexpected server error. |

### Error response (401)
```json
{
  "status": "error",
  "message": "Invalid or expired API key",
  "data": null,
  "context": {},
  "instance": "/entities"
}
```

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
  "instance": "/entities"
}
```

### Error response (500)
```json
{
  "status": "error",
  "message": "Unexpected server error",
  "data": null,
  "context": {},
  "instance": "/entities"
}
```

## Get entity

Retrieve a single EAV entity by its numeric identifier.

Clearance level 2 (Read) or any lower clearance level is required to use this endpoint.

### Endpoint
```
GET /entities/{id_entity}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_entity` | Yes | int | Numeric identifier of the entity. |

### Query parameters

This endpoint does not accept query parameters.

### Request body

This endpoint does not accept a request body.

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X GET \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  /entities/1
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_entity": 1,
    "entity_name": "location"
  },
  "context": {},
  "instance": "/entities/1"
}
```

### Response envelope

| Field | Type | Description |
| --- | --- | --- |
| `status` | string | Always `"success"` when the entity is returned. |
| `message` | string | Human readable summary of the operation. |
| `data` | object | Entity payload. See [Entity object](#entity-object). |
| `context` | object | Additional metadata supplied by the API. Empty when not required. |
| `instance` | string | Relative path for the requested resource. |

### Entity object

This endpoint returns the [Entity object](#entity-object) described in the List entities section.

### Status codes

| Status | Description |
| --- | --- |
| `200` | Entity retrieved successfully. |
| `400` | The request could not be processed due to invalid headers or account metadata. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to view the entity. |
| `404` | The requested entity was not found. |
| `500` | Unexpected server error. |

### Error response (401)
```json
{
  "status": "error",
  "message": "Invalid or expired API key",
  "data": null,
  "context": {},
  "instance": "/entities/1"
}
```

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
  "instance": "/entities/1"
}
```

### Error response (404)
```json
{
  "status": "error",
  "message": "Entity not found",
  "data": null,
  "context": {
    "path_params": {
      "id_entity": 9999
    }
  },
  "instance": "/entities/9999"
}
```

### Error response (500)
```json
{
  "status": "error",
  "message": "Unexpected server error",
  "data": null,
  "context": {},
  "instance": "/entities/1"
}
```

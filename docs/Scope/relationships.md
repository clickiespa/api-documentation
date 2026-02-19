# Resource Relationships

## Endpoints
- [List relationships](#list-relationships)
- [List relationships by type](#list-relationships-by-type)
- [Create relationship](#create-relationship)
- [Get specific relationship](#get-specific-relationship)
- [Delete relationship](#delete-relationship)

A relationship links two resources together in a parent-child hierarchy. Supported entities are collaborators, assets, dashboards and metrics.

## List relationships

### Endpoint
```
GET /relationships/{entity}/{id}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `{entity}` | yes | Parent entity type | string | No |
| `{id}` | yes | Parent entity ID | int | No |

### Query parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `direction` | no | `child` or `parent` | string | `child` |
| `skip` | no | Offset for pagination | int | `0` |
| `limit` | no | Max records to return | int | `100` |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /relationships/assets/10
```

### Sample response
```json
{
  "status": "success",
  "message": "Relationships retrieved",
  "data": [{"id_child": 5, "child_type": "metrics"}],
  "context": null,
  "instance": "/relationships/assets/10"
}
```

The response uses the standard JSON envelope.

### Status codes
- `200` Relationships retrieved successfully
- `400` Invalid request
- `401` Unauthorized
- `403` Forbidden
- `404` Parent entity not found
- `500` Internal server error

## List relationships by type

### Endpoint
```
GET /relationships/{entity}/{id}/{entity_type}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `{entity}` | yes | Parent entity type | string | No |
| `{id}` | yes | Parent entity ID | int | No |
| `{entity_type}` | yes | Filter by child or parent entity type | string | No |

### Query parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `direction` | no | `child` or `parent` | string | `child` |
| `skip` | no | Offset for pagination | int | `0` |
| `limit` | no | Max records to return | int | `100` |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /relationships/assets/10/metrics
```

### Sample response
```json
{
  "status": "success",
  "message": "Relationships retrieved",
  "data": [{"id_child": 20, "child_type": "metrics"}],
  "context": null,
  "instance": "/relationships/assets/10/metrics"
}
```

The response uses the standard JSON envelope.

### Status codes
- `200` Relationships retrieved successfully
- `400` Invalid request
- `401` Unauthorized
- `403` Forbidden
- `404` Parent entity not found
- `500` Internal server error

## Create relationship

### Endpoint
```
POST /relationships/{entity}/{id}/{child_entity}/{child_id}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `{entity}` | yes | Parent entity type | string | No |
| `{id}` | yes | Parent entity ID | int | No |
| `{child_entity}` | yes | Child entity type | string | No |
| `{child_id}` | yes | Child entity ID | int | No |

### Sample request
```bash
curl -X POST -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /relationships/assets/10/metrics/20
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_parent": 10, "id_child": 20},
  "context": null,
  "instance": "/relationships/assets/10/metrics/20"
}
```

The response uses the standard JSON envelope.

### Status codes
- `201` Relationship created
- `400` Invalid request
- `401` Unauthorized
- `403` Forbidden
- `404` Entity not found
- `500` Internal server error

### Error response (400)
```json
{
  "status": "error",
  "message": "Relationship already exists",
  "data": null,
  "context": null,
  "instance": "/relationships/assets/10/metrics/20"
}
```

## Get specific relationship

### Endpoint
```
GET /relationships/{entity}/{id}/{child_entity}/{child_id}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `{entity}` | yes | Parent entity type | string | No |
| `{id}` | yes | Parent entity ID | int | No |
| `{child_entity}` | yes | Child entity type | string | No |
| `{child_id}` | yes | Child entity ID | int | No |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /relationships/assets/10/metrics/20
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_parent": 10, "id_child": 20},
  "context": null,
  "instance": "/relationships/assets/10/metrics/20"
}
```

The response uses the standard JSON envelope.

### Status codes
- `200` Relationship retrieved
- `400` Invalid request
- `401` Unauthorized
- `403` Forbidden
- `404` Relationship not found
- `500` Internal server error

### Error response (404)
```json
{
  "status": "error",
  "message": "Element not found",
  "data": null,
  "context": null,
  "instance": "/relationships/assets/10/metrics/999"
}
```

## Delete relationship

### Endpoint
```
DELETE /relationships/{entity}/{id}/{child_entity}/{child_id}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Required | Description | Type | Default |
| --- | --- | --- | --- | --- |
| `{entity}` | yes | Parent entity type | string | No |
| `{id}` | yes | Parent entity ID | int | No |
| `{child_entity}` | yes | Child entity type | string | No |
| `{child_id}` | yes | Child entity ID | int | No |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /relationships/assets/10/metrics/20
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/relationships/assets/10/metrics/20"
}
```

The response uses the standard JSON envelope.

### Status codes
- `200` Relationship deleted
- `400` Invalid request
- `401` Unauthorized
- `403` Forbidden
- `404` Relationship not found
- `500` Internal server error

# Roles

Roles define the level of access granted to collaborators for the resources that belong to an account.

## Endpoints
- [List roles](#list-roles)

## List roles

Clearance with read permission over roles is required to use this endpoint.

### Endpoint
```
GET /roles
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
| `limit` | no | int | `100` | Maximum number of roles to return |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
     -H "Account: <ID_ACCOUNT>" \
     /roles?skip=0&limit=100
```

### Sample response
```json
[
  {
    "id_role": 1,
    "role_name": "Admin",
    "role_description": "Full access to all features"
  }
]
```

### Error responses

`403 Forbidden`
```json
{
  "detail": "Insufficient permissions"
}
```

### Status codes
- `200` Roles retrieved successfully
- `400` Invalid query parameters
- `401` Missing or invalid authentication credentials
- `403` Insufficient permissions to list roles
- `404` Endpoint not found
- `500` Unexpected server error


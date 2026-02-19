# Clearances

Clearances represent the authorization level that determines which resources a collaborator can manage within an account. Level `1` is the most restrictive and level `7` grants the broadest access.

Refer to the [Resources permissions by clearance matrix](https://www.google.com) for a cross-reference of API resources, operations and the clearance levels that can execute them.

## Endpoints
- [List clearances](#list-clearances)
- [Retrieve my clearance](#retrieve-my-clearance)

## List clearances

Clearance with read permission over clearances is required to use this endpoint.

### Endpoint
```
GET /clearances
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

This endpoint does not accept query parameters.

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
     -H "Account: <ID_ACCOUNT>" \
     /clearances
```

### Sample response
```json
[
  {
    "id_clearance": 1,
    "clearance_code": "A1",
    "clearance_description": "Entry level clearance"
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
- `200` Clearances retrieved successfully
- `400` Invalid request
- `401` Missing or invalid authentication credentials
- `403` Insufficient permissions to list clearances
- `404` Endpoint not found
- `500` Unexpected server error

## Retrieve my clearance

You must belong to the requested account to use this endpoint.
Clearance associated with your membership in the target account is required to use this endpoint.

### Endpoint
```
GET /clearances/me
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

This endpoint does not accept query parameters.

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
     -H "Account: <ID_ACCOUNT>" \
     /clearances/me
```

### Sample response
```json
{
  "id_clearance": 1,
  "clearance_code": "A1",
  "clearance_description": "Entry level clearance"
}
```

### Error responses

`401 Unauthorized`
```json
{
  "detail": "No authorizer context"
}
```

### Status codes
- `200` Clearance retrieved successfully
- `400` Invalid request
- `401` Missing or invalid authentication credentials
- `403` Caller does not belong to the account
- `404` Endpoint not found
- `500` Unexpected server error


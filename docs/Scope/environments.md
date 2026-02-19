# Environments

/table of contents

## Endpoints
- [List environments](#list-environments)

Environments define the top-level tenant grouping for accounts and related resources.

## List environments
Retrieves the environments available to the authenticated session.

Clearance level 5 or lower is required to use this endpoint.

### Endpoint
```
GET /environments
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| None | This endpoint does not use path parameters. | - |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| None | No | - | - | This endpoint does not accept query parameters. |

### Response envelope
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {},
  "context": {},
  "instance": "/endpoint/path"
}
```

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
  /environments
```

### Sample response
```json
{
  "status": "success",
  "message": "Environments retrieved",
  "data": [
    {
      "id_environment": 1,
      "environment_name": "Production",
      "environment_description": "Primary tenant environment"
    }
  ],
  "context": {},
  "instance": "/environments"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Environments not available",
  "data": {},
  "context": {
    "errors": [
      "No environments found for this session"
    ]
  },
  "instance": "/environments"
}
```

### Status codes
- `200` — Environment list retrieved successfully.
- `400` — Invalid request parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Environments were not found.
- `500` — Internal server error.

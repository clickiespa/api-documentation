# Languages

## Endpoints
- [List languages](#list-languages)

Available languages for collaborator profiles.

## List languages
Retrieves the catalog of languages that collaborators can select in their profiles.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /languages
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

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
  /languages
```

### Sample response
```json
{
  "status": "success",
  "message": "Languages retrieved",
  "data": [
    {
      "id_language": 1,
      "language_code": "en",
      "language_name": "English"
    }
  ],
  "context": {},
  "instance": "/languages"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Languages not available",
  "data": {},
  "context": {
    "errors": [
      "No languages have been configured for this account"
    ]
  },
  "instance": "/languages"
}
```

### Status codes
- `200` — Language list retrieved successfully.
- `400` — Invalid request filters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — No languages were found.
- `500` — Internal server error.


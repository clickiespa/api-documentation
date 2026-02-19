# Templates

## Endpoints
- [List templates](#list-templates)
- [Create template](#create-template)
- [Get template](#get-template)
- [Update template](#update-template)
- [Delete template](#delete-template)

Templates for communications such as emails or notifications.

## List templates
Retrieves the templates available to the current account.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /templates
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
| `limit` | No | int | `50` | Maximum number of templates to return |

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
  "/templates?skip=0&limit=20"
```

### Sample response
```json
{
  "status": "success",
  "message": "Templates retrieved",
  "data": [
    {
      "id_template": 1,
      "id_communication_type": 2,
      "template_name": "Alert",
      "template_description": "Triggered when a monitor detects an anomaly",
      "template_body": "An issue has been detected.",
      "created_at": "2024-01-07T18:12:46Z"
    }
  ],
  "context": {},
  "instance": "/templates"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Templates not available",
  "data": {},
  "context": {
    "errors": [
      "No templates were found for the requested account"
    ]
  },
  "instance": "/templates"
}
```

### Status codes
- `200` — Template list retrieved successfully.
- `400` — Invalid pagination parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — No templates were found.
- `500` — Internal server error.

## Create template
Creates a new communication template for the account.

Clearance level 1 or lower is required to use this endpoint.

### Endpoint
```
POST /templates
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |
| `Content-Type` | Yes | Must be `application/json` | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_communication_type` | No | int | `1` | Communication type associated with the template. See [Communication types](../Types/types.md#communication-types). |
| `template_name` | Yes | string | No | Display name shown in the UI |
| `template_description` | No | string | `None` | Short description of the template |
| `template_body` | No | string | `None` | Content that is delivered when the template is used |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>",
  "Content-Type": "application/json"
}
```

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{
    "template_name": "Alert",
    "template_description": "Triggered when a monitor detects an anomaly",
    "template_body": "An issue has been detected."
  }' \
  /templates
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_template": 2,
    "id_communication_type": 1,
    "template_name": "Alert",
    "template_description": "Triggered when a monitor detects an anomaly",
    "template_body": "An issue has been detected.",
    "created_at": "2024-01-09T08:21:11Z"
  },
  "context": {},
  "instance": "/templates"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Template could not be created",
  "data": {},
  "context": {
    "errors": [
      "template_name must be unique"
    ]
  },
  "instance": "/templates"
}
```

### Status codes
- `201` — Template created successfully.
- `400` — The request payload is invalid.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Dependent resources were not found.
- `500` — Internal server error.

## Get template
Retrieves a template by its identifier.

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /templates/{id_template}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_template` | Yes | int | Template numeric identifier |

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
  /templates/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_template": 2,
    "id_communication_type": 1,
    "template_name": "Alert",
    "template_description": "Triggered when a monitor detects an anomaly",
    "template_body": "An issue has been detected.",
    "created_at": "2024-01-09T08:21:11Z"
  },
  "context": {},
  "instance": "/templates/2"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Template not found",
  "data": {},
  "context": {
    "errors": [
      "Template 2 does not exist"
    ]
  },
  "instance": "/templates/2"
}
```

### Status codes
- `200` — Template retrieved successfully.
- `400` — Invalid identifier format.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Template was not found.
- `500` — Internal server error.

## Update template
Updates a template with new metadata or body content.

Clearance level 2 or lower is required to use this endpoint.

### Endpoint
```
PUT /templates/{id_template}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |
| `Content-Type` | Yes | Must be `application/json` | string |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_template` | Yes | int | Template numeric identifier |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_communication_type` | No | int | `None` | New communication type identifier. See [Communication types](../Types/types.md#communication-types). |
| `template_name` | No | string | `None` | Updated display name |
| `template_description` | No | string | `None` | Updated short description |
| `template_body` | No | string | `None` | Updated template content |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>",
  "Content-Type": "application/json"
}
```

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{
    "template_description": "Updated short description"
  }' \
  /templates/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_template": 2,
    "id_communication_type": 1,
    "template_name": "Alert",
    "template_description": "Updated short description",
    "template_body": "An issue has been detected.",
    "created_at": "2024-01-09T08:21:11Z"
  },
  "context": {},
  "instance": "/templates/2"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Template could not be updated",
  "data": {},
  "context": {
    "errors": [
      "No changes were provided"
    ]
  },
  "instance": "/templates/2"
}
```

### Status codes
- `200` — Template updated successfully.
- `400` — The request payload is invalid.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Template was not found.
- `500` — Internal server error.

## Delete template
Deletes an existing template.

Clearance level 1 or lower is required to use this endpoint.

### Endpoint
```
DELETE /templates/{id_template}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_template` | Yes | int | Template numeric identifier |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X DELETE \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  /templates/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": {},
  "context": {},
  "instance": "/templates/2"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Template could not be deleted",
  "data": {},
  "context": {
    "errors": [
      "Template 2 is referenced by an active monitor"
    ]
  },
  "instance": "/templates/2"
}
```

### Status codes
- `200` — Template deleted successfully.
- `400` — Invalid identifier format.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Template was not found.
- `500` — Internal server error.


# Collaborators

## Endpoints
- [List collaborators](#list-collaborators)
- [Create collaborator](#create-collaborator)
- [Get collaborator](#get-collaborator)
- [Update collaborator](#update-collaborator)
- [Delete collaborator](#delete-collaborator)
- [Collaborator login history](#collaborator-login-history)

Endpoints related to user management within an account.

Collaborator is the given name of a user registered on the platform.
A collaborator exists in a specific environment and can be [assigned to an account](../Scope/accounts.md#add-collaborator-to-account) to interact with.
A collaborator assigned to an account can interact with a defined set of resources based on the collaborator’s clearance level on the account.

## List collaborators

Retrieve all collaborators linked to the current account.

### Endpoint
```
GET /collaborators
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination |
| `limit` | no | int | Max records to return |

### Request headers example
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" <HOST_NAME>/collaborators
```

### Sample response
```json
{
  "status": "success",
  "message": "Collaborators retrieved",
  "data": [{"id_user": 1, "user_email": "john@example.com"}],
  "context": null,
  "instance": "/collaborators"
}
```

 

## Create collaborator

Create a new collaborator for the current account.

### Endpoint
```
POST /collaborators
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Request body

| Field | Required | Type | Description |
| --- | --- | --- | --- |
| `user_email` | yes | string | Email address |
| `user_name` | yes | string | First name |
| `user_last_name` | yes | string | Last name |
| `id_role` | no | int | Role ID |
| `id_language` | no | int | Language ID (see [List languages](../Global_resources/languages.md#list-languages)) |
| `id_time_zone` | yes | int | Time zone ID (see [List time zones](../Global_resources/time_zones.md#list-time-zones)) |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"user_email":"new@example.com","user_name":"New","user_last_name":"User","id_time_zone":1}' \
  <HOST_NAME>/collaborators
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_user": 2, "user_email": "new@example.com"},
  "context": null,
  "instance": "/collaborators"
}
```

## Get collaborator

Retrieve details for a specific collaborator.

### Endpoint
```
GET /collaborators/{id_user}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_user}` | Collaborator numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" <HOST_NAME>/collaborators/<ID_USER>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_user": 2, "user_email": "new@example.com"},
  "context": null,
  "instance": "/collaborators/<ID_USER>"
}
```

## Update collaborator

Modify details of an existing collaborator.

### Endpoint
```
PUT /collaborators/{id_user}
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
| `{id_user}` | Collaborator numeric identifier | int |

### Request body

| Field | Required | Type | Description |
| --- | --- | --- | --- |
| `user_email` | no | string | Email address |
| `user_name` | no | string | First name |
| `user_last_name` | no | string | Last name |
| `id_role` | no | int | Role ID |
| `id_language` | no | int | Language ID (see [List languages](../Global_resources/languages.md#list-languages)) |
| `id_time_zone` | no | int | Time zone ID (see [List time zones](../Global_resources/time_zones.md#list-time-zones)) |

Updating the email changes login credentials.

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"user_name":"Updated"}' \
  <HOST_NAME>/collaborators/<ID_USER>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_user": 2, "user_email": "new@example.com"},
  "context": null,
  "instance": "/collaborators/<ID_USER>"
}
```

## Delete collaborator

Remove a collaborator from the current account.

### Endpoint
```
DELETE /collaborators/{id_user}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_user}` | Collaborator numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" <HOST_NAME>/collaborators/<ID_USER>
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/collaborators/<ID_USER>"
}
```

## Collaborator login history

Retrieve authentication events for a collaborator.

### Endpoint
```
GET /collaborators/{id_user}/login_history
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_user}` | Collaborator numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" <HOST_NAME>/collaborators/<ID_USER>/login_history
```

### Sample response
```json
[
  {"event": "SignIn", "timestamp": "2023-09-01T00:00:00Z"}
]
```

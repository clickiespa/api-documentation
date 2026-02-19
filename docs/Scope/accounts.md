# Accounts

## Endpoints
- [List accounts](#list-accounts)
- [Create account](#create-account)
- [Get account](#get-account)
- [Update account](#update-account)
- [Delete account](#delete-account)
- [Add collaborator to account](#add-collaborator-to-account)
- [Update collaborator in account](#update-collaborator-in-account)
- [Remove collaborator from account](#remove-collaborator-from-account)

An account is the entity that represents your business. If you are assigned to an account, you can access global information regarding the status and processes of your business.

An account can own uoms, metrics, monitors, assets and dashboards.

## List accounts

Clearance level 5 or lower is required to list accounts.
Users with clearance level 1 are not scoped to the current account and can retrieve every account in the environment.

### Endpoint
```
GET /accounts
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Number of records to skip |
| `limit` | no | int | Max records to return |
| `archived` | no | bool | Include archived accounts |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /accounts?skip=0&limit=10&archived=false
```

### Sample response
```json
{
  "status": "success",
  "message": "Accounts retrieved",
  "data": [
    {
      "id_account": 1,
      "id_environment": 2,
      "account_name": "Main",
      "ids_time_zone": "11,2,4",
      "environment": {
        "id_environment": 2,
        "environment_name": "Production",
        "environment_description": "Primary tenant environment"
      }
    }
  ],
  "context": null,
  "instance": "/accounts"
}
```

## Create account

Clearance level 3 or lower is required to create an account.
Clearance A1 is required to set `id_environment` when creating an account.

### Endpoint
```
POST /accounts
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_environment` | no | int | null | Environment ID to assign the account (clearance A1 only) |
| `id_file_picture` | no | int | null | Picture file ID |
| `account_name` | yes | string | No | Display name |
| `account_description` | no | string | null | Optional description |
| `ids_time_zone` | yes | string | No | Comma-separated time zone IDs |

### Sample request
```bash
curl -X POST   -H "Authorization: <API_KEY>"   -H "Account: <ID_ACCOUNT>"   -H "Content-Type: application/json"   -d '{"id_environment":2,"id_file_picture":1,"account_name":"Main","account_description":"Sample","ids_time_zone":"11,2,4"}'   /accounts
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_account": 2,
    "id_environment": 2,
    "account_name": "Main",
    "ids_time_zone": "11,2,4",
    "environment": {
      "id_environment": 2,
      "environment_name": "Production",
      "environment_description": "Primary tenant environment"
    }
  },
  "context": null,
  "instance": "/accounts"
}
```

## Get account

Clearance level 5 or lower is required to get an account by ID.
Users with clearance level 1 can fetch any account, even if it is not linked to the Account header they are using.

### Endpoint
```
GET /accounts/{id_account}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_account}` | Account numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /accounts/1
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_account": 1,
    "id_environment": 2,
    "account_name": "Main",
    "ids_time_zone": "11,2,4",
    "environment": {
      "id_environment": 2,
      "environment_name": "Production",
      "environment_description": "Primary tenant environment"
    }
  },
  "context": null,
  "instance": "/accounts/1"
}
```

## Update account

Clearance level 4 or lower is required to update an account.
Clearance A1 is required to set `id_environment` when updating an account.

### Endpoint
```
PUT /accounts/{id_account}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_account}` | Account numeric identifier | int |

### Request body
| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_environment` | no | int | null | New environment ID (clearance A1 only) |
| `id_file_picture` | no | int | null | Picture file ID |
| `account_name` | no | string | null | Display name |
| `account_description` | no | string | null | Optional description |
| `ids_time_zone` | no | string | null | Comma-separated time zone IDs |
| `account_archived` | no | bool | null | Archive flag |

### Sample request
```bash
curl -X PUT   -H "Authorization: <API_KEY>"   -H "Account: <ID_ACCOUNT>"   -H "Content-Type: application/json"   -d '{"id_environment":2,"id_file_picture":1,"account_name":"Main","account_description":"Updated","ids_time_zone":"1,2,3","account_archived":false}'   /accounts/1
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_account": 1,
    "id_environment": 2,
    "account_name": "Main",
    "ids_time_zone": "1,2,3",
    "environment": {
      "id_environment": 2,
      "environment_name": "Production",
      "environment_description": "Primary tenant environment"
    }
  },
  "context": null,
  "instance": "/accounts/1"
}
```

## Delete account

Clearance level 3 or lower is required to delete an account.

### Endpoint
```
DELETE /accounts/{id_account}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_account}` | Account numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /accounts/1
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/accounts/1"
}
```

The `/accounts/collaborators` endpoints manage collaborators assigned directly to an account. If you need an aggregated view of every collaborator under an account, refer to the [`/collaborators` endpoints](../Collaborators/collaborators.md). To retrieve collaborators linked to a specific asset, use the [`/relationships` endpoints](relationships.md) documented for that resource.

## Add collaborator to account

Clearance level 4 or lower is required to add collaborators to an account.

The requesting user must already be a collaborator of the target account unless the session clearance level is `1`, in which case the caller can manage collaborators for any account. When other callers are not part of the account, the API returns `400` with the message `You must be a collaborator of the account to manage its collaborators.` If the caller lacks enough clearance to assign the requested level, the API responds with `400` and the message `Your clearance level is not high enough to assign id clearance <id_clearance>`.

### Endpoint
```
POST /accounts/collaborators
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Query parameters

This endpoint does not accept query parameters.

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `user_email` | yes | string | No | Collaborator email |
| `id_clearance` | yes | int | No | Clearance level |

### Sample request
```bash
curl -X POST   -H "Authorization: <API_KEY>"   -H "Account: <ID_ACCOUNT>"   -H "Content-Type: application/json"   -d '{"user_email":"user@example.com","id_clearance":2}'   /accounts/collaborators
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_user_account": 42,
    "id_user": 18,
    "id_account": 7,
    "id_clearance": 2,
    "is_limited": false,
    "created_at": "2025-01-12T16:41:07Z"
  },
  "context": {},
  "instance": "/accounts/collaborators"
}
```

### Error response (400 - caller is not a collaborator)
```json
{
  "status": "error",
  "message": "You must be a collaborator of the account to manage its collaborators.",
  "data": null,
  "context": {
    "session": {
      "id_account": 1,
      "id_user": 23
    }
  },
  "instance": "/accounts/collaborators"
}
```

### Error response (400 - insufficient clearance)
```json
{
  "status": "error",
  "message": "Your clearance level is not high enough to assign id clearance 2.",
  "data": null,
  "context": {
    "body": {
      "user_email": "user@example.com",
      "id_clearance": 2
    }
  },
  "instance": "/accounts/collaborators"
}
```

## Update collaborator in account

Clearance level 5 or lower is required to update collaborators.

Callers with clearance level `1` can update collaborators for any account without already belonging to it. Other callers must be collaborators of the account.

### Endpoint
```
PUT /accounts/collaborators/{collaborator_email}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{collaborator_email}` | Email of the collaborator to update | string |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_clearance` | no | int | null | New clearance level |

### Sample request
```bash
curl -X PUT   -H "Authorization: <API_KEY>"   -H "Account: <ID_ACCOUNT>"   -H "Content-Type: application/json"   -d '{"id_clearance":3}'   /accounts/collaborators/user@example.com
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_user_account": 42,
    "id_user": 18,
    "id_account": 7,
    "id_clearance": 3,
    "is_limited": false,
    "created_at": "2025-01-12T16:41:07Z"
  },
  "context": {},
  "instance": "/accounts/collaborators/user@example.com"
}
```

## Remove collaborator from account

Clearance level 4 or lower is required to remove collaborators from an account.

Clearance level `1` callers can remove collaborators from any account even if they are not currently assigned to it. Other callers must be collaborators of the account.

### Endpoint
```
DELETE /accounts/collaborators/{collaborator_email}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{collaborator_email}` | Email of the collaborator to remove | string |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /accounts/collaborators/user@example.com
```

### Sample response
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": {
    "id_user_account": 42,
    "id_user": 18,
    "id_account": 7,
    "id_clearance": 3,
    "is_limited": false,
    "created_at": "2025-01-12T16:41:07Z"
  },
  "context": {},
  "instance": "/accounts/collaborators/user@example.com"
}
```

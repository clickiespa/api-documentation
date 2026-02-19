# Assets

## Endpoints
- [List assets](#list-assets)
- [Create asset](#create-asset)
- [Get asset](#get-asset)
- [Update asset](#update-asset)
- [Delete asset](#delete-asset)

An asset is a physical entity with a specific real location and is mainly used as a central node on which other entities such as metrics or dashboards are associated. Each asset exists only within a specific account and has an asset type that is used to differentiate between assets and their designated function in one or more accounts.

## List assets

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /assets
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
| `archived` | no | bool | Filter by archive state. Omit to return both active and archived assets. |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /assets
```

### Sample response
```json
{
  "status": "success",
  "message": "Assets retrieved",
  "data": [{"id_asset": 1, "asset_name": "Building", "asset_archived": false}],
  "context": null,
  "instance": "/assets"
}
```

## Create asset

Clearance level 5 or lower is required to use this endpoint.

### Endpoint
```
POST /assets
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
| `id_category` | yes | int | No | Asset category ID from [List categories](../Types/asset_categories.md#list-categories). |
| `asset_name` | yes | string | No | Display name |
| `asset_description` | no | string | null | Description |
| `asset_observations` | no | string | null | Notes |

> **Note:** Asset creation temporarily does not accept picture or location references. Support for `id_file_picture` and `id_location` will return in a future update.

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"id_category":1,"asset_name":"Building"}' \
  /assets
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_asset": 2, "asset_name": "Building", "asset_archived": false},
  "context": null,
  "instance": "/assets"
}
```

## Get asset

Clearance level 7 or lower is required to use this endpoint.

### Endpoint
```
GET /assets/{id_asset}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_asset}` | Asset numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_asset": 2, "asset_name": "Building", "asset_archived": false},
  "context": null,
  "instance": "/assets/2"
}
```


## Update asset

Clearance level 5 or lower is required to use this endpoint.

### Endpoint
```
PUT /assets/{id_asset}
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
| `{id_asset}` | Asset numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_category` | no | int | null | Asset category ID from [List categories](../Types/asset_categories.md#list-categories). |
| `id_file_picture` | no | int | null | Picture file ID (icon management is currently restricted). |
| `id_location` | no | int | null | Location ID (location directories are not yet available). |
| `asset_name` | no | string | null | Display name |
| `asset_description` | no | string | null | Description |
| `asset_observations` | no | string | null | Notes |
| `asset_archived` | no | bool | null | Archive state |

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"asset_description":"Updated"}' \
  /assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_asset": 2, "asset_name": "Building", "asset_archived": false},
  "context": null,
  "instance": "/assets/2"
}
```

## Delete asset

Clearance level 5 or lower is required to use this endpoint.

### Endpoint
```
DELETE /assets/{id_asset}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_asset}` | Asset numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /assets/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/assets/2"
}
```

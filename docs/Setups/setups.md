# Setups

Configuration packages that define how devices interact with assets and gateways.

## Endpoints
- [List setups](#list-setups)
- [Get setup](#get-setup)
- [Create setup](#create-setup)
- [Update setup](#update-setup)
- [Delete setup](#delete-setup)

## List setups

Retrieve every setup visible to the authenticated account. Optionally filter by asset, device model or name and include setups that are not attached to an asset.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /setups
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | No | int | Number of setups to skip before collecting the result set. |
| `limit` | No | int | Maximum number of setups to return. |
| `id_asset` | No | int | Filters setups that belong to the provided asset. Retrieve the identifier from [List assets](../Assets/assets.md#list-assets). |
| `id_device_model` | No | int | Filters setups that use the provided device model. Obtain the identifier from [Get device model](../Devices/device_models.md#get-device-model). |
| `setup_name` | No | string | Performs a case-insensitive search over setup names. |
| `include_without_asset` | No | bool | When `true`, also returns setups without an associated asset. |

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
  "/setups?limit=50&include_without_asset=true"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Setups retrieved",
  "data": [
    {
      "id_setup": 12,
      "id_setup_gateway": 3,
      "id_asset": 25,
      "id_device_model": 4,
      "setup_name": "Packaging Line",
      "setup_description": "Default configuration for packaging",
      "setup_identifier": "setup-00012",
      "setup_recommended_equipment": "Gateway X2",
      "setup_gateway_order": 1,
      "setup_gateway_port": "A1",
      "setup_archived": false,
      "asset": {
        "asset_name": "Packaging Line 1",
        "asset_identifier": "asset-25"
      }
    }
  ],
  "context": {},
  "instance": "/setups"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_setup` | int | Numeric identifier of the setup. |
| `id_setup_gateway` | int | Identifier of the gateway assigned to the setup. |
| `id_asset` | int | Asset associated with the setup. May be `null` when `include_without_asset` is enabled. Retrieve full details through [Get asset](../Assets/assets.md#get-asset). |
| `id_device_model` | int | Device model configured in the setup. See [Get device model](../Devices/device_models.md#get-device-model). |
| `setup_name` | string | Human-readable setup name. |
| `setup_description` | string | Optional description of the setup. |
| `setup_identifier` | string | Unique identifier used to reference the setup in external systems. |
| `setup_recommended_equipment` | string | Optional note describing the recommended equipment. |
| `setup_gateway_order` | int | Order in which the setup communicates with the gateway. |
| `setup_gateway_port` | string | Gateway port assigned to this setup. |
| `setup_archived` | bool | Indicates whether the setup is archived. |
| `asset` | object | Summary of the linked asset, including the name and identifier returned by [Get asset](../Assets/assets.md#get-asset). |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setups retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to list setups. |
| `500` | Unexpected server error. |

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
  "instance": "/setups"
}
```

## Get setup

Retrieve a single setup by its numeric identifier. Use this endpoint to inspect the configuration for a specific asset-device pairing.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /setups/{id_setup}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_setup` | Yes | int | Numeric identifier of the setup to retrieve. You can obtain it from [List setups](#list-setups). |

### Query parameters

This endpoint does not accept query parameters.

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
  "/setups/12"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_setup": 12,
    "id_setup_gateway": 3,
    "id_asset": 25,
    "id_device_model": 4,
    "setup_name": "Packaging Line",
    "setup_description": "Default configuration for packaging",
    "setup_identifier": "setup-00012",
    "setup_recommended_equipment": "Gateway X2",
    "setup_gateway_order": 1,
    "setup_gateway_port": "A1",
    "setup_archived": false,
    "asset": {
      "asset_name": "Packaging Line 1",
      "asset_identifier": "asset-25"
    }
  },
  "context": {},
  "instance": "/setups/12"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_setup` | int | Numeric identifier of the setup. |
| `id_setup_gateway` | int | Identifier of the gateway assigned to the setup. |
| `id_asset` | int | Asset associated with the setup. May be `null` when the setup is not linked to an asset. Retrieve full details through [Get asset](../Assets/assets.md#get-asset). |
| `id_device_model` | int | Device model configured in the setup. See [Get device model](../Devices/device_models.md#get-device-model). |
| `setup_name` | string | Human-readable setup name. |
| `setup_description` | string | Optional description of the setup. |
| `setup_identifier` | string | Unique identifier used to reference the setup externally. |
| `setup_recommended_equipment` | string | Optional note describing the recommended equipment. |
| `setup_gateway_order` | int | Order in which the setup communicates with the gateway. |
| `setup_gateway_port` | string | Gateway port assigned to this setup. |
| `setup_archived` | bool | Indicates whether the setup is archived. |
| `asset` | object | Summary of the linked asset, including the name and identifier returned by [Get asset](../Assets/assets.md#get-asset). |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to view this setup. |
| `404` | Setup not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Setup 999 was not found",
  "data": null,
  "context": {
    "headers": {
      "Account": "<ID_ACCOUNT>"
    }
  },
  "instance": "/setups/999"
}
```

## Create setup

Create a setup associated with the authenticated account. Provide the device model and optional asset and gateway references.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
POST /setups
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_setup_gateway` | No | int | No | Identifier of the gateway setup to chain. Retrieve it from [Get setup](#get-setup). |
| `id_asset` | No | int | No | Asset associated with the setup. Retrieve it from [Get asset](../Assets/assets.md#get-asset). |
| `id_device_model` | Yes | int | No | Device model configured by the setup. Obtain it from [Get device model](../Devices/device_models.md#get-device-model). |
| `setup_name` | Yes | string | No | Human-readable name for the setup. |
| `setup_description` | No | string | No | Optional description that clarifies the purpose of the setup. |
| `setup_recommended_equipment` | No | string | No | Notes about the preferred equipment for this setup. |
| `setup_gateway_order` | No | int | 100 | Order in which the setup communicates with the gateway. |
| `setup_gateway_port` | No | string | No | Gateway port assigned to the setup. |
| `setup_archived` | No | bool | false | Indicates whether the setup starts archived. |

The `setup_identifier` is generated automatically during creation and cannot be supplied in the request.

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X POST -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{
    "id_device_model": 4,
    "id_asset": 25,
    "setup_name": "Packaging Line B",
    "setup_description": "Secondary packaging flow",
    "setup_gateway_order": 2
  }' \
  /setups
```

### Sample response (201)
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_setup": 120,
    "id_setup_gateway": null,
    "id_asset": 25,
    "id_device_model": 4,
    "setup_name": "Packaging Line B",
    "setup_description": "Secondary packaging flow",
    "setup_identifier": "setup-00100",
    "setup_recommended_equipment": null,
    "setup_gateway_order": 2,
    "setup_gateway_port": null,
    "setup_archived": false,
    "asset": null
  },
  "context": {
    "body": {
      "id_device_model": 4,
      "id_asset": 25,
      "setup_name": "Packaging Line B",
      "setup_description": "Secondary packaging flow",
      "setup_gateway_order": 2
    }
  },
  "instance": "/setups"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `201` | Setup created successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to create setups. |
| `404` | Referenced asset, gateway or device model was not found. |
| `500` | Unexpected server error. |

### Error response (400)
```json
{
  "status": "error",
  "message": "DeviceModel with id_device_model 999 does not exist",
  "data": null,
  "context": {
    "body": {
      "id_device_model": 999,
      "setup_name": "Invalid setup"
    }
  },
  "instance": "/setups"
}
```

## Update setup

Modify an existing setup. Only the provided attributes are updated; omitted fields keep their current values.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
PUT /setups/{id_setup}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_setup` | Yes | int | Numeric identifier of the setup to update. Retrieve it from [List setups](#list-setups). |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_setup_gateway` | No | int | No | Gateway setup to chain. Omit to keep the current value. |
| `id_asset` | No | int | No | Asset associated with the setup. Send `null` to detach the current asset or omit to keep the existing link. |
| `id_device_model` | No | int | No | Device model configured in the setup. |
| `setup_name` | No | string | No | New name for the setup. |
| `setup_description` | No | string | No | Updated description. |
| `setup_recommended_equipment` | No | string | No | Updated equipment notes. |
| `setup_gateway_order` | No | int | No | Updated gateway order. |
| `setup_gateway_port` | No | string | No | Updated gateway port. |
| `setup_archived` | No | bool | No | Set to `true` to archive the setup. |

The `setup_identifier` returned by the API is read-only and cannot be modified after creation.

To detach a setup from its current asset, include `"id_asset": null` in the payload.

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X PUT -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{
    "setup_name": "Packaging Line B - Updated",
    "setup_gateway_port": "B2",
    "id_asset": null
  }' \
  /setups/120
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_setup": 120,
    "id_setup_gateway": null,
    "id_asset": null,
    "id_device_model": 4,
    "setup_name": "Packaging Line B - Updated",
    "setup_description": "Secondary packaging flow",
    "setup_identifier": "setup-00100",
    "setup_recommended_equipment": null,
    "setup_gateway_order": 2,
    "setup_gateway_port": "B2",
    "setup_archived": false,
    "asset": null
  },
  "context": {
    "body": {
      "setup_name": "Packaging Line B - Updated",
      "setup_gateway_port": "B2",
      "id_asset": null
    },
    "path": {
      "id_setup": "120"
    }
  },
  "instance": "/setups/120"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup updated successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to update setups. |
| `404` | Setup or referenced asset, gateway or device model was not found. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Setup 120 was not found",
  "data": null,
  "context": {
    "path": {
      "id_setup": "120"
    }
  },
  "instance": "/setups/120"
}
```

## Delete setup

Remove a setup from the authenticated account.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
DELETE /setups/{id_setup}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_setup` | Yes | int | Numeric identifier of the setup to delete. Retrieve it from [List setups](#list-setups). |

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  /setups/120
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": null,
  "context": {
    "path": {
      "id_setup": "120"
    }
  },
  "instance": "/setups/120"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup removed successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to delete setups. |
| `404` | Setup not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Setup 120 was not found",
  "data": null,
  "context": {
    "path": {
      "id_setup": "120"
    }
  },
  "instance": "/setups/120"
}
```

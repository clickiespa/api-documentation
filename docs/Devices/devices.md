# Devices

## Endpoints
- [List devices](#list-devices)
- [Get device](#get-device)
- [Create device](#create-device)
- [Update device](#update-device)
- [Delete device](#delete-device)

Registered hardware devices available to the authenticated account.

**Clearance requirements:** create requires clearance level 2 or lower, read requires clearance level 5 or lower, update requires clearance level 2 or lower and delete requires clearance level 2 or lower.

## List devices

Retrieve every device visible to the authenticated account, optionally filtering by inventory, model, operational status or a custom identifier.

Clearance level 5 or lower is required to read devices through this endpoint.

### Endpoint
```
GET /devices
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `skip` | No | int | `0` | Pagination offset. |
| `limit` | No | int | `100` | Maximum number of records to return. |
| `id_inventory` | No | int | No | Filter devices associated with a specific inventory returned by [List inventories](../Setups/inventories.md#list-inventories). |
| `id_device_model` | No | int | No | Filter devices by the related model identifier returned by [List device models](./device_models.md#list-device-models). |
| `id_device_status` | No | int | No | Filter devices by their operational status identifier (`1` for connected, `2` for disconnected). |
| `device_custom_id` | No | string | No | Filter devices by the account-specific custom identifier. |

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
  "/devices?limit=20&device_custom_id=NODE-01"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Devices retrieved successfully",
  "data": [
    {
      "id_device": 1,
      "id_inventory": 8,
      "id_device_model": 3,
      "id_device_status": 2,
      "device_custom_id": "NODE-01",
      "device_observations": "Outdoor gateway",
      "device_configuration": null,
      "device_archived": false,
      "device_status_since": "2024-03-17T15:24:51Z"
    },
    {
      "id_device": 2,
      "id_inventory": null,
      "id_device_model": 5,
      "id_device_status": 1,
      "device_custom_id": "NODE-02",
      "device_observations": null,
      "device_configuration": "{}",
      "device_archived": false,
      "device_status_since": null
    }
  ],
  "context": {},
  "instance": "/devices"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device` | int | Numeric identifier of the device. Use [Get device](#get-device) to retrieve its details. |
| `id_inventory` | int | Inventory identifier where the device is located. |
| `id_device_model` | int | Identifier of the device model that defines the capabilities of the hardware. |
| `id_device_status` | int | Identifier representing the device operational status (`1` is connected, `2` is disconnected). |
| `device_custom_id` | string | Account-specific identifier assigned to the device. |
| `device_observations` | string | Free-form notes about the device. |
| `device_configuration` | string | Serialized configuration applied to the device. |
| `device_archived` | bool | Indicates whether the device is archived. |
| `device_status_since` | string | ISO-8601 timestamp for the last status change. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Devices retrieved successfully. |
| `400` | Pagination or filter validation failed. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to list devices. |
| `404` | Not used for this collection endpoint. |
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
  "instance": "/devices"
}
```

## Get device

Retrieve a device by its numeric identifier.

Clearance level 5 or lower is required to read devices through this endpoint.

### Endpoint
```
GET /devices/{id_device}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_device` | Yes | int | Numeric identifier of the device to retrieve. |

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
  /devices/1
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_device": 1,
    "id_inventory": 8,
    "id_device_model": 3,
    "id_device_status": 2,
    "device_custom_id": "NODE-01",
    "device_observations": "Outdoor gateway",
    "device_configuration": null,
    "device_archived": false,
    "device_status_since": "2024-03-17T15:24:51Z"
  },
  "context": {},
  "instance": "/devices/1"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device` | int | Numeric identifier of the device. |
| `id_inventory` | int | Inventory identifier where the device is located. |
| `id_device_model` | int | Identifier of the device model that defines the capabilities of the hardware. Use [Get device model](./device_models.md#get-device-model) to retrieve its catalog entry. |
| `id_device_status` | int | Identifier representing the device operational status (`1` is connected, `2` is disconnected). |
| `device_custom_id` | string | Account-specific identifier assigned to the device. |
| `device_observations` | string | Free-form notes about the device. |
| `device_configuration` | string | Serialized configuration applied to the device. |
| `device_archived` | bool | Indicates whether the device is archived. |
| `device_status_since` | string | ISO-8601 timestamp for the last status change. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device retrieved successfully. |
| `400` | Validation failed for the provided identifier. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to read devices. |
| `404` | Device not found for the supplied identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Device not found",
  "data": null,
  "context": {
    "params": {
      "id_device": 99999
    }
  },
  "instance": "/devices/99999"
}
```

## Create device

Register a new device within the authenticated account.

Clearance level 2 or lower is required to use this endpoint.

### Endpoint
```
POST /devices
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_inventory` | No | int | No | Inventory where the device is stored. Retrieve it from [List inventories](../Setups/inventories.md#list-inventories). |
| `id_device_model` | Yes | int | No | Device model that defines the capabilities of the hardware. Obtain it from [Get device model](./device_models.md#get-device-model). |
| `id_device_status` | No | int | No | Operational status identifier (`1` for connected, `2` for disconnected). |
| `device_custom_id` | No | string | No | Account-specific identifier assigned to the device. |
| `device_observations` | No | string | No | Free-form notes about the device. |
| `device_configuration` | No | string | No | Serialized configuration that should be applied to the device. |
| `device_archived` | No | bool | false | Set to `true` to create the device in an archived state. |

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
    "id_device_model": 3,
    "id_inventory": 8,
    "device_custom_id": "NODE-100",
    "device_observations": "Installed in warehouse"
  }' \
  /devices
```

### Sample response (201)
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_device": 120,
    "id_inventory": 8,
    "id_device_model": 3,
    "id_device_status": null,
    "device_custom_id": "NODE-100",
    "device_observations": "Installed in warehouse",
    "device_configuration": null,
    "device_archived": false,
    "device_status_since": null
  },
  "context": {
    "body": {
      "id_device_model": 3,
      "id_inventory": 8,
      "device_custom_id": "NODE-100",
      "device_observations": "Installed in warehouse"
    }
  },
  "instance": "/devices"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `201` | Device created successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to create devices. |
| `404` | Referenced inventory or device model was not found. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "DeviceModel with id_device_model 999 does not exist",
  "data": null,
  "context": {
    "body": {
      "id_device_model": 999,
      "device_custom_id": "INVALID"
    }
  },
  "instance": "/devices"
}
```

## Update device

Modify an existing device. Omitted fields keep their current value.

Clearance level 2 or lower is required to use this endpoint.

### Endpoint
```
PUT /devices/{id_device}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_device` | Yes | int | Numeric identifier of the device to update. |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_inventory` | No | int | No | Inventory where the device is stored. |
| `id_device_model` | No | int | No | Device model assigned to the hardware. |
| `id_device_status` | No | int | No | Operational status identifier. |
| `device_custom_id` | No | string | No | Account-specific identifier assigned to the device. |
| `device_observations` | No | string | No | Updated notes about the device. |
| `device_configuration` | No | string | No | Updated serialized configuration. |
| `device_archived` | No | bool | No | Set to `true` to archive the device. |

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
    "id_inventory": 9,
    "device_archived": true
  }' \
  /devices/120
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_device": 120,
    "id_inventory": 9,
    "id_device_model": 3,
    "id_device_status": null,
    "device_custom_id": "NODE-100",
    "device_observations": "Installed in warehouse",
    "device_configuration": null,
    "device_archived": true,
    "device_status_since": null
  },
  "context": {
    "body": {
      "id_inventory": 9,
      "device_archived": true
    },
    "path": {
      "id_device": "120"
    }
  },
  "instance": "/devices/120"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device updated successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to update devices. |
| `404` | Device or referenced inventory or model was not found. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Device 120 was not found",
  "data": null,
  "context": {
    "path": {
      "id_device": "120"
    }
  },
  "instance": "/devices/120"
}
```

## Delete device

Remove a device from the authenticated account.

Clearance level 2 or lower is required to use this endpoint.

### Endpoint
```
DELETE /devices/{id_device}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_device` | Yes | int | Numeric identifier of the device to delete. |

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
  /devices/120
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": null,
  "context": {
    "path": {
      "id_device": "120"
    }
  },
  "instance": "/devices/120"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device removed successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to delete devices. |
| `404` | Device not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Device 120 was not found",
  "data": null,
  "context": {
    "path": {
      "id_device": "120"
    }
  },
  "instance": "/devices/120"
}
```

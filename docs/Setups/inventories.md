# Inventories

Collections of devices grouped for organizational purposes.

## Endpoints
- [List inventories](#list-inventories)
- [Get inventory](#get-inventory)
- [Create inventory](#create-inventory)
- [Update inventory](#update-inventory)
- [Delete inventory](#delete-inventory)

## List inventories

Retrieve every inventory visible to the authenticated account. You can optionally filter results by name.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /inventories
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | No | int | Number of inventories to skip before collecting the result set. |
| `limit` | No | int | Maximum number of inventories to return. |
| `inventory_name` | No | string | Performs a case-insensitive search over inventory names. |

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
  "/inventories?inventory_name=Main"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Inventories retrieved",
  "data": [
    {
      "id_inventory": 1,
      "inventory_name": "Main",
      "inventory_identifier": "inv-main",
      "inventory_description": "Primary devices"
    }
  ],
  "context": {},
  "instance": "/inventories"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_inventory` | int | Numeric identifier of the inventory. |
| `inventory_name` | string | Human-readable inventory name. |
| `inventory_identifier` | string | Unique identifier used to reference the inventory. |
| `inventory_description` | string | Optional description of the inventory. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Inventories retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to list inventories. |
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
  "instance": "/inventories"
}
```

## Get inventory

Retrieve the details of a single inventory by its numeric identifier.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /inventories/{id_inventory}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_inventory` | Yes | int | Numeric identifier of the inventory to retrieve. You can obtain it from [List inventories](#list-inventories). |

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
  "/inventories/1"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_inventory": 1,
    "inventory_name": "Main",
    "inventory_identifier": "inv-main",
    "inventory_description": "Primary devices"
  },
  "context": {},
  "instance": "/inventories/1"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_inventory` | int | Numeric identifier of the inventory. |
| `inventory_name` | string | Human-readable inventory name. |
| `inventory_identifier` | string | Unique identifier used to reference the inventory. |
| `inventory_description` | string | Optional description of the inventory. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Inventory retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to view this inventory. |
| `404` | Inventory not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Inventory 999 was not found",
  "data": null,
  "context": {
    "headers": {
      "Account": "<ID_ACCOUNT>"
    }
  },
  "instance": "/inventories/999"
}
```

## Create inventory

Create a new inventory available to the authenticated account.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
POST /inventories
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `inventory_name` | Yes | string | No | Human-readable inventory name. |
| `inventory_description` | No | string | No | Optional description for the inventory. |

The `inventory_identifier` is generated automatically and is not accepted in the request payload.

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
    "inventory_name": "Warehouse",
    "inventory_description": "Devices stored in the main warehouse"
  }' \
  /inventories
```

### Sample response (201)
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {
    "id_inventory": 15,
    "inventory_name": "Warehouse",
    "inventory_identifier": "inv-warehouse",
    "inventory_description": "Devices stored in the main warehouse"
  },
  "context": {
    "body": {
      "inventory_name": "Warehouse",
      "inventory_description": "Devices stored in the main warehouse"
    }
  },
  "instance": "/inventories"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `201` | Inventory created successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to create inventories. |
| `500` | Unexpected server error. |

### Error response (400)
```json
{
  "status": "error",
  "message": "inventory_name field required",
  "data": null,
  "context": {
    "body": {
      "inventory_name": "Warehouse"
    }
  },
  "instance": "/inventories"
}
```

## Update inventory

Modify an existing inventory. Omitted fields keep their current value.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
PUT /inventories/{id_inventory}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_inventory` | Yes | int | Numeric identifier of the inventory to update. Retrieve it from [List inventories](#list-inventories). |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `inventory_name` | No | string | No | Updated inventory name. |
| `inventory_description` | No | string | No | Updated description. |

The `inventory_identifier` returned in responses is read-only and cannot be modified.

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
    "inventory_description": "Devices stored in the refurbished warehouse"
  }' \
  /inventories/15
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {
    "id_inventory": 15,
    "inventory_name": "Warehouse",
    "inventory_identifier": "inv-warehouse",
    "inventory_description": "Devices stored in the refurbished warehouse"
  },
  "context": {
    "body": {
      "inventory_description": "Devices stored in the refurbished warehouse"
    },
    "path": {
      "id_inventory": "15"
    }
  },
  "instance": "/inventories/15"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Inventory updated successfully. |
| `400` | Validation failed for the provided payload. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to update inventories. |
| `404` | Inventory not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Inventory 15 was not found",
  "data": null,
  "context": {
    "path": {
      "id_inventory": "15"
    }
  },
  "instance": "/inventories/15"
}
```

## Delete inventory

Remove an inventory from the authenticated account.

Clearance level 2 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
DELETE /inventories/{id_inventory}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_inventory` | Yes | int | Numeric identifier of the inventory to delete. Retrieve it from [List inventories](#list-inventories). |

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
  /inventories/15
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element removed successfully",
  "data": null,
  "context": {
    "path": {
      "id_inventory": "15"
    }
  },
  "instance": "/inventories/15"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Inventory removed successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to delete inventories. |
| `404` | Inventory not found for the provided identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Inventory 15 was not found",
  "data": null,
  "context": {
    "path": {
      "id_inventory": "15"
    }
  },
  "instance": "/inventories/15"
}
```

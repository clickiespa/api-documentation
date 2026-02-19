# Device Models

## Endpoints
- [List device models](#list-device-models)
- [Get device model](#get-device-model)
- [List device model types](#list-device-model-types)
- [List device model manufacturers](#list-device-model-manufacturers)

Descriptions of supported device models that can be assigned to devices.

**Clearance requirements:** create requires clearance level 2 or lower, read requires clearance level 4 or lower, update requires clearance level 2 or lower and delete requires clearance level 2 or lower.

## List device models

Retrieve device models visible to the authenticated account, with optional filters for archival status, model type, manufacturer and name.

Clearance level 4 or lower is required to read device models through this endpoint.

### Endpoint
```
GET /device_models
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
| `archived` | No | bool | No | Filter by archival status. When omitted, both archived and active models are returned. |
| `id_device_model_type` | No | int | No | Filter models by the related type identifier configured for the account. See [Related catalogs](#related-catalogs). |
| `id_device_model_manufacturer` | No | int | No | Filter models by the manufacturer identifier associated with the account. See [Related catalogs](#related-catalogs). |
| `model_name` | No | string | No | Filter models using a case-insensitive match on the model name. |

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
  "/device_models?archived=false&limit=25"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Device models retrieved successfully",
  "data": [
    {
      "id_device_model": 3,
      "id_device_model_type": 2,
      "id_device_model_manufacturer": 4,
      "id_file_picture": 18,
      "id_configuration": null,
      "model_name": "Gateway v2",
      "model_description": "LTE-enabled indoor gateway",
      "model_observations": null,
      "model_sends_data": true,
      "model_is_node": false,
      "model_trace_by_quantity": false,
      "model_archived": false
    }
  ],
  "context": {},
  "instance": "/device_models"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_model` | int | Numeric identifier of the device model. |
| `id_device_model_type` | int | Identifier of the model type configured for the account. See [Related catalogs](#related-catalogs). |
| `id_device_model_manufacturer` | int | Identifier of the manufacturer associated with the model. See [Related catalogs](#related-catalogs). |
| `id_file_picture` | int | Identifier of the picture file illustrating the model. |
| `id_configuration` | int | Identifier of the default configuration applied to devices of this model. |
| `model_name` | string | Human-readable name of the device model. |
| `model_description` | string | Narrative description of the model. |
| `model_observations` | string | Internal notes about the model. |
| `model_sends_data` | bool | Indicates whether devices of this model publish telemetry. |
| `model_is_node` | bool | Specifies if the model represents a node device. |
| `model_trace_by_quantity` | bool | Determines if stock is tracked by quantity for the model. |
| `model_archived` | bool | Indicates whether the model is archived. |

### Related catalogs

Use the following endpoints to obtain identifiers referenced by device models:

- [`GET /types/device_models`](#list-device-model-types) returns available model types.
- [`GET /device_model_manufacturers`](#list-device-model-manufacturers) lists registered manufacturers.

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device models retrieved successfully. |
| `400` | Pagination or filter validation failed. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to list device models. |
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
  "instance": "/device_models"
}
```

## Get device model

Retrieve details for a single device model.

Clearance level 4 or lower is required to read device models through this endpoint.

### Endpoint
```
GET /device_models/{id_device_model}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_device_model` | Yes | int | Numeric identifier of the device model to retrieve. |

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
  /device_models/3
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_device_model": 3,
    "id_device_model_type": 2,
    "id_device_model_manufacturer": 4,
    "id_file_picture": 18,
    "id_configuration": null,
    "model_name": "Gateway v2",
    "model_description": "LTE-enabled indoor gateway",
    "model_observations": null,
    "model_sends_data": true,
    "model_is_node": false,
    "model_trace_by_quantity": false,
    "model_archived": false
  },
  "context": {},
  "instance": "/device_models/3"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_model` | int | Numeric identifier of the device model. |
| `id_device_model_type` | int | Identifier of the model type configured for the account. See [Related catalogs](#related-catalogs). |
| `id_device_model_manufacturer` | int | Identifier of the manufacturer associated with the model. See [Related catalogs](#related-catalogs). |
| `id_file_picture` | int | Identifier of the picture file illustrating the model. |
| `id_configuration` | int | Identifier of the default configuration applied to devices of this model. |
| `model_name` | string | Human-readable name of the device model. |
| `model_description` | string | Narrative description of the model. |
| `model_observations` | string | Internal notes about the model. |
| `model_sends_data` | bool | Indicates whether devices of this model publish telemetry. |
| `model_is_node` | bool | Specifies if the model represents a node device. |
| `model_trace_by_quantity` | bool | Determines if stock is tracked by quantity for the model. |
| `model_archived` | bool | Indicates whether the model is archived. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device model retrieved successfully. |
| `400` | Validation failed for the provided identifier. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to read device models. |
| `404` | Device model not found for the supplied identifier. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Device model not found",
  "data": null,
  "context": {
    "params": {
      "id_device_model": 99999
    }
  },
  "instance": "/device_models/99999"
}
```

## List device model types

Retrieve device model types configured for the authenticated account.

Clearance level 4 or lower is required to access this endpoint.

### Endpoint
```
GET /types/device_models
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
| `archived` | No | bool | No | Filter by archival status. When omitted, both archived and active types are returned. |

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
  "/types/device_models?archived=false"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Device model types retrieved successfully",
  "data": [
    {
      "id_device_model_type": 2,
      "type_name": "Gateway",
      "type_description": "Gateway devices that forward data",
      "type_archived": false
    }
  ],
  "context": {},
  "instance": "/types/device_models"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_model_type` | int | Identifier of the device model type. |
| `type_name` | string | Name of the model type. |
| `type_description` | string | Description of the type. |
| `type_archived` | bool | Indicates whether the type is archived. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device model types retrieved successfully. |
| `400` | Pagination or filter validation failed. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to list model types. |
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
  "instance": "/types/device_models"
}
```

## List device model manufacturers

Retrieve device model manufacturers configured for the authenticated account.

Clearance level 4 or lower is required to access this endpoint.

### Endpoint
```
GET /device_model_manufacturers
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
| `archived` | No | bool | No | Filter by archival status. When omitted, both archived and active manufacturers are returned. |

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
  "/device_model_manufacturers?archived=false"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Device model manufacturers retrieved successfully",
  "data": [
    {
      "id_device_model_manufacturer": 4,
      "id_file_picture": 18,
      "manufacturer_name": "Acme Devices",
      "manufacturer_description": "Industrial-grade sensors",
      "manufacturer_observations": null,
      "manufacturer_archived": false
    }
  ],
  "context": {},
  "instance": "/device_model_manufacturers"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_model_manufacturer` | int | Identifier of the manufacturer. |
| `id_file_picture` | int | Identifier of the picture file associated with the manufacturer. |
| `manufacturer_name` | string | Name of the manufacturer. |
| `manufacturer_description` | string | Description of the manufacturer. |
| `manufacturer_observations` | string | Internal notes about the manufacturer. |
| `manufacturer_archived` | bool | Indicates whether the manufacturer is archived. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device model manufacturers retrieved successfully. |
| `400` | Pagination or filter validation failed. |
| `401` | Authentication failed. |
| `403` | The authenticated user is not authorized to list model manufacturers. |
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
  "instance": "/device_model_manufacturers"
}
```

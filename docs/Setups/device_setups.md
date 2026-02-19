# Device Setups

Historical relationship between physical devices and the setups they implement.

## Endpoints
- [Devices by setup](#devices-by-setup)
- [Setups by device](#setups-by-device)

## Devices by setup

List every assignment of a device to the requested setup.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /setups/{id_setup}/devices
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_setup` | Yes | int | Numeric identifier of the setup whose device history you want to inspect. Retrieve it from [Get setup](./setups.md#get-setup). |

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
  "/setups/5/devices"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Devices retrieved",
  "data": [
    {
      "id_device_setup": 42,
      "id_device": 10,
      "id_setup": 5,
      "setup_install_date": "2023-07-01T12:00:00Z",
      "setup_install_observations": "Initial installation",
      "setup_uninstall_date": null,
      "setup_uninstall_observations": null
    }
  ],
  "context": {},
  "instance": "/setups/5/devices"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_setup` | int | Numeric identifier of the device-setup relationship. |
| `id_device` | int | Device identifier. Retrieve it from [Get device](../Devices/devices.md#get-device). |
| `id_setup` | int | Setup identifier linked to the assignment. See [Get setup](./setups.md#get-setup). |
| `setup_install_date` | string | Date and time when the device was linked to the setup (ISO 8601). |
| `setup_install_observations` | string | Optional notes recorded during installation. |
| `setup_uninstall_date` | string | Date and time when the device was removed from the setup. `null` while installed. |
| `setup_uninstall_observations` | string | Optional notes recorded during removal. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Device history retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to inspect device setups. |
| `404` | The referenced setup does not exist. |
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
  "instance": "/setups/5/devices"
}
```

## Setups by device

List every setup that has been assigned to the requested device.

Clearance level 6 or lower (level 1 is admin) is required to use this endpoint.

### Endpoint
```
GET /devices/{id_device}/setups
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | Yes | API key generated from your profile | string |
| `Account` | Yes | Target account ID | int |

### Path parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_device` | Yes | int | Numeric identifier of the device whose setup history you want to inspect. Retrieve it from [Get device](../Devices/devices.md#get-device). |

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
  "/devices/10/setups"
```

### Sample response (200)
```json
{
  "status": "success",
  "message": "Setups retrieved",
  "data": [
    {
      "id_device_setup": 42,
      "id_device": 10,
      "id_setup": 5,
      "setup_install_date": "2023-07-01T12:00:00Z",
      "setup_install_observations": "Initial installation",
      "setup_uninstall_date": null,
      "setup_uninstall_observations": null
    }
  ],
  "context": {},
  "instance": "/devices/10/setups"
}
```

### Response data attributes

| Field | Type | Description |
| --- | --- | --- |
| `id_device_setup` | int | Numeric identifier of the device-setup relationship. |
| `id_device` | int | Device identifier. Retrieve it from [Get device](../Devices/devices.md#get-device). |
| `id_setup` | int | Setup identifier linked to the assignment. See [Get setup](./setups.md#get-setup). |
| `setup_install_date` | string | Date and time when the device was linked to the setup (ISO 8601). |
| `setup_install_observations` | string | Optional notes recorded during installation. |
| `setup_uninstall_date` | string | Date and time when the device was removed from the setup. `null` while installed. |
| `setup_uninstall_observations` | string | Optional notes recorded during removal. |

### Status codes

| Status | Description |
| --- | --- |
| `200` | Setup history retrieved successfully. |
| `401` | Authentication failed. |
| `403` | The authenticated user lacks clearance to inspect device setups. |
| `404` | The referenced device does not exist. |
| `500` | Unexpected server error. |

### Error response (404)
```json
{
  "status": "error",
  "message": "Device 999 was not found",
  "data": null,
  "context": {
    "headers": {
      "Account": "<ID_ACCOUNT>"
    }
  },
  "instance": "/devices/999/setups"
}
```

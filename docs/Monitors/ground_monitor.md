# Ground Monitor

## Endpoints
- [Fetch data](#fetch-data)

Endpoint returning data about the ground monitoring process.

## Fetch data
Retrieve the current ground monitoring status.

### Endpoint
```
GET /ground_monitor
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Account ID of the requester | int |
| `account-id` | yes | Account ID to retrieve information from | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" \
     -H "Account: <ID_ACCOUNT>" \
     -H "account-id: <TARGET_ACCOUNT_ID>" \
     /ground_monitor
```

### Sample response
```json
{
  "status": "success",
  "message": "130 elements obtained successfully",
  "data": [
    {
      "id_asset": 1,
      "asset_name": "Sample asset",
      "asset_archived": 0,
      "total_monitored_devices": 3,
      "disconnected_devices": "0",
      "na_devices": "0",
      "percentage_disconnected": "0.00"
    }
  ],
  "context": {},
  "instance": "/ground_monitor"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Successful operation |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Not found |
| `422` | Validation error |
| `500` | Internal server error |

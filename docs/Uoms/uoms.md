# Units of Measure (UOMs)

## Endpoints
- [List UOMs](#list-uoms)
- [Create UOM](#create-uom)
- [Get UOM](#get-uom)
- [Update UOM](#update-uom)
- [Delete UOM](#delete-uom)

A uom is a unit of measure and is used to define magnitude of a quantity in a metric. There are global uoms that are used between all accounts and local uoms that belong to an account.

## List UOMs

### Endpoint
```
GET /uoms
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
| `owned` | no | bool | Only show account UOMs |
| `archived` | no | bool | Include archived UOMs |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /uoms?skip=0&limit=10
```

### Sample response
```json
{
  "status": "success",
  "message": "UOMs retrieved",
  "data": [{"id_uom": 1, "uom_name": "Percentage"}],
  "context": null,
  "instance": "/uoms"
}
```

## Create UOM

### Endpoint
```
POST /uoms
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
| `id_aggregation` | yes | int | Aggregation method identifier |
| `id_interpolation` | yes | int | Interpolation method identifier |
| `uom_name` | yes | string | Display name |
| `uom_description` | no | string | Optional description |
| `uom_unit` | yes | string | Unit symbol |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"id_aggregation":1,"id_interpolation":1,"uom_name":"Degree","uom_unit":"°C"}' \
  /uoms
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_uom": 2, "uom_name": "Degree"},
  "context": null,
  "instance": "/uoms"
}
```

## Get UOM

### Endpoint
```
GET /uoms/{id_uom}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_uom}` | UOM numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /uoms/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_uom": 2, "uom_name": "Degree"},
  "context": null,
  "instance": "/uoms/2"
}
```

## Update UOM

### Endpoint
```
PUT /uoms/{id_uom}
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
| `{id_uom}` | UOM numeric identifier | int |

### Request body
At least one field from the create table.

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: 1" \
  -H "Content-Type: application/json" \
  -d '{"uom_description":"Updated"}' \
  /uoms/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_uom": 2, "uom_name": "Degree"},
  "context": null,
  "instance": "/uoms/2"
}
```

## Delete UOM

### Endpoint
```
DELETE /uoms/{id_uom}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_uom}` | UOM numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: 1" /uoms/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/uoms/2"
}
```

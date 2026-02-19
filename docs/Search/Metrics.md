## Search metrics

Filter metrics by linked assets or intrinsic metadata using a flexible POST body.

Clearance level 7 or lower with read permission over metrics is required to use this endpoint.

### Endpoint
```
POST /search/metrics
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | Must be `application/json` | string |

### Request body
| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `resource_filter` | no | object | No | Scope results to metrics linked through `resource_relationships` when provided. |
| `param_filters` | no | array | No | List of field-level filters that accept the same names allowed in `return_fields` (except `id_resource`). |
| `return_fields` | no | array | No | Subset of fields to return. When a `resource_filter` is present, `id_resource` is always included in the response even if omitted from this list. |
| `limit` | no | int | `100` | Maximum number of records to return. |
| `skip` | no | int | `0` | Offset for pagination. |

#### `resource_filter`
| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `resource_type` | yes | string | No | Must be `asset`. |
| `direction` | no | string | `child` | Relationship side to traverse: `parent` to return metrics as parents of the listed resources, `child` (default) to return metrics as children. The relationship join only occurs when `resource_filter` is provided. |
| `operator` | yes | string | No | Comparison operator limited to `in` or `not_in`. |
| `resource_ids` | yes | array | No | IDs applied to the `resource_type` column using the selected operator. The returned `id_resource` matches the values supplied here. |

#### `param_filters` items
| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `type` | yes | string | No | Metric attribute to filter (any valid `return_fields` value except `id_resource`). |
| `operator` | yes | string | No | Comparison operator: `eq`, `not_eq`, `gt`, `gte`, `lt`, `lte`, `in`, `not_in`, `contains`, `like`, `contains_all`, `like_all`, `not_contains`, or `not_like`. |
| `value` | yes | any | No | Literal or list used with the selected operator. |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  /search/metrics \
  -d '{
    "resource_filter": {
      "resource_type": "asset",
      "direction": "child",
      "operator": "in",
      "resource_ids": [10, 12]
    },
    "param_filters": [
      {"type": "metric_tags", "operator": "contains", "value": "temperature"}
    ],
    "return_fields": ["id_resource", "id_metric", "metric_name", "metric_tags"],
    "limit": 25,
    "skip": 0
  }'
```

### Sample response
```json
{
  "status": "success",
  "message": "Metrics filtered successfully",
  "data": [
    {
      "id_resource": 10,
      "id_metric": 42,
      "id_metric_source": 4,
      "source_name": "API",
      "id_uom": 3,
      "uom_name": "Celsius",
      "uom_unit": "°C",
      "uom_description": "Temperature in Celsius",
      "id_aggregation": 1,
      "aggregation_name": "mean",
      "aggregation_description": "Average",
      "aggregation_function": "mean",
      "id_interpolation": 2,
      "interpolation_name": "linear",
      "interpolation_description": "Linear interpolation",
      "interpolation_function": "linear",
      "metric_name": "Ambient temperature",
      "metric_identifier": "<metric_uuid>",
      "metric_description": "Temperature sensor",
      "metric_tags": "temperature,indoor",
      "metric_formula": null,
      "metric_force_availability": false,
      "metric_impacts_health": true,
      "created_at": "2025-01-10T12:00:00.000Z"
    }
  ],
  "context": {},
  "instance": "/search/metrics"
}
```

### Status codes
- `200` Success
- `400` Invalid filter or payload validation error
- `401` Unauthorized
- `403` Forbidden
- `404` Resource not found
- `500` Internal server error

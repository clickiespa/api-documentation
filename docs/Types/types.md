# Types

## Endpoints
- [List available type groups](#list-available-type-groups)
- [Monitor types](#monitor-types)
- [Communication types](#communication-types)
- [Monitor trigger types](#monitor-trigger-types)
- [Monitor statuses](#monitor-statuses)
- [Metric sources](#metric-sources)
- [Widget types](#widget-types)
- [Form input types](#form-input-types)

Utility endpoints returning lists of predefined types.

A type is a particular kind of entity, which is used in creation or modification of other entities to categorize them. There exist an entity type for some entities, most of them follows the same naming pattern:
for entity “entity_a” its type entity is called “entity_a_type”. For example, for assets their entity type is called asset_types and so on.

## List available type groups

### Endpoint
```
GET /types
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types
```

### Sample response
```json
["monitors","communications","monitor_triggers","monitor_statuses","metric_sources","widgets","form_inputs","metric_events","assets"]
```

## Monitor types

### Endpoint
```
GET /types/monitors
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/monitors
```

### Sample response
```json
["status","threshold","anomaly"]
```

## Communication types

### Endpoint
```
GET /types/communications
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/communications
```

### Sample response
```json
["email","webhook","sms"]
```

## Monitor trigger types

### Endpoint
```
GET /types/monitor_triggers
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/monitor_triggers
```

### Sample response
```json
["above","below","deadman"]
```

## Monitor statuses

### Endpoint
```
GET /types/monitor_statuses
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/monitor_statuses
```

### Sample response
```json
["enabled","disabled","snoozed"]
```

## Metric sources

### Endpoint
```
GET /types/metric_sources
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/metric_sources
```

### Sample response
```json
["device","asset"]
```

## Widget types

### Endpoint
```
GET /types/widgets
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/widgets
```

### Sample response
```json
["chart","number","table"]
```

## Form input types

### Endpoint
```
GET /types/form_inputs
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key generated from your profile | string |
| `Account` | yes | Target account ID | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /types/form_inputs
```

### Sample response
```json
["text","number","select"]
```

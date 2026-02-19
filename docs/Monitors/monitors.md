# Monitors

## Endpoints
- [List monitors](#list-monitors)
- [Create monitor](#create-monitor)
- [Get monitor](#get-monitor)
- [Update monitor](#update-monitor)
- [Delete monitor](#delete-monitor)
- [Monitor triggers](#monitor-triggers)
- [Monitor templates](#monitor-templates)
- [Monitor rules](#monitor-rules)
- [Monitor history](#monitor-history)

A monitor observes one or more metrics with a certain frequency, compares values with limits or ranges configured by rules, and detects those values that comply with the rules. The monitoring can be defined as a status that changes according to the status of its rules. Each monitor exists only within a specific account.

## List monitors

Retrieve the monitors configured for the current account.

Clearance level 7 or any lower clearance is required to use this endpoint.

### Endpoint
```
GET /monitors
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

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors?skip=0&limit=100
```

### Sample response
```json
{
  "status": "success",
  "message": "Monitors retrieved",
  "data": [{"id_monitor": 1, "monitor_name": "Temperature Monitor"}],
  "context": null,
  "instance": "/monitors"
}
```

## Create monitor

Create a new monitor to observe metrics within the account.

Clearance level 5 or any lower clearance is required to use this endpoint.

### Endpoint
```
POST /monitors
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
| `monitor_name` | yes | string | No | Display name |
| `id_monitor_type` | no | int | 6 | Monitor type. Available values can be retrieved with [GET /types/monitors](../Types/types.md#monitor-types) |
| `id_monitor_status` | no | int | 1 | Id value of the status that will be communicated. Available status ids can be retrieved by endpoint [GET /types/monitor_statuses](../Types/types.md#monitor-statuses) |
| `monitor_description` | no | string | null | Description |
| `monitor_sampling_window` | no | int | 15 | Minutes to average |
| `monitor_sampling_frequency` | no | int | 15 | Check frequency |

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"monitor_name":"Temp","id_monitor_type":1,"id_monitor_status":1}' \
  /monitors
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_monitor": 2, "monitor_name": "Temp"},
  "context": null,
  "instance": "/monitors"
}
```

## Get monitor

Retrieve the details of a specific monitor.

Clearance level 7 or any lower clearance is required to use this endpoint.

### Endpoint
```
GET /monitors/{id_monitor}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_monitor": 2, "monitor_name": "Temp"},
  "context": null,
  "instance": "/monitors/2"
}
```

## Update monitor

Update the configuration of an existing monitor.

Clearance level 6 or any lower clearance is required to use this endpoint.

### Endpoint
```
PUT /monitors/{id_monitor}
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
| `{id_monitor}` | Monitor numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_monitor_type` | no | int | null | Monitor type. Available values can be retrieved with [GET /types/monitors](../Types/types.md#monitor-types) |
| `id_monitor_status` | no | int | null | Id value of the status that will be communicated. Available status ids can be retrieved by endpoint [GET /types/monitor_statuses](../Types/types.md#monitor-statuses) |
| `monitor_name` | no | string | null | Display name |
| `monitor_description` | no | string | null | Description |
| `monitor_sampling_window` | no | int | null | Minutes to average |
| `monitor_sampling_frequency` | no | int | null | Check frequency |

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"monitor_name":"Updated"}' \
  /monitors/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_monitor": 2, "monitor_name": "Updated"},
  "context": null,
  "instance": "/monitors/2"
}
```

## Delete monitor

Delete a monitor that is no longer needed.

Clearance level 5 or any lower clearance is required to use this endpoint.

### Endpoint
```
DELETE /monitors/{id_monitor}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/monitors/2"
}
```

## Monitor triggers

Monitor triggers define how a monitor reacts when one of its rules changes status. They usually dispatch communications, run integrations, or schedule follow-up actions based on the configured trigger type.

### Response envelope
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {},
  "context": {},
  "instance": "/endpoint/path"
}
```

### List triggers

Retrieve the triggers configured on a specific monitor.

Clearance level 7 or any lower clearance is required to use this endpoint.


### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Endpoint
```
GET /monitors/{id_monitor}/triggers
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination |
| `limit` | no | int | Max records to return |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/triggers?skip=0&limit=100
```

### Sample response
```json
{
  "status": "success",
  "message": "Monitor triggers retrieved",
  "data": [
    {
      "id_monitor_trigger": 2,
      "id_trigger_type": 1,
      "time_pattern": "08,18|1,5|*|*",
      "trigger_observations": "Notify when the monitor changes to warning or critical status.",
      "trigger_on": "2,3",
      "trigger_enabled": true,
      "trigger_parameters": {
        "id_users": [
          "2045"
        ],
        "notif_title": "Temperature alert",
        "notif_preview": "Sensor A exceeded threshold",
        "notif_content": "The temperature has exceeded the configured threshold for over 10 minutes.",
        "notif_template": "1"
      }
    }
  ],
  "context": null,
  "instance": "/monitors/1/triggers"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Status codes
- `200` — Monitor triggers retrieved successfully.
- `400` — Invalid pagination parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Trigger or monitor not found.
- `500` — Internal server error.

### Error responses

| Status | Description |
| --- | --- |
| `404` | Returned when a trigger references a time pattern that was deleted. Update the trigger schedule or recreate it with a valid pattern. |

```json
{
  "status": "error",
  "message": "Monitor trigger references a missing time pattern. Update the trigger to point to a valid pattern (id_time_pattern=99).",
  "data": null,
  "context": {
    "path": {
      "id_monitor": "1"
    }
  },
  "instance": "/monitors/1/triggers"
}
```

### Get trigger

Fetch details for a single trigger that belongs to the selected monitor.

Clearance level 7 or any lower clearance is required to use this endpoint.


### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Endpoint
```
GET /monitors/{id_monitor}/triggers/{id_monitor_trigger}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_trigger}` | Trigger numeric identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/triggers/2
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_monitor_trigger": 2,
    "id_trigger_type": 1,
    "time_pattern": "08,18|1,5|*|*",
    "trigger_observations": "Notify when the monitor changes to warning or critical status.",
    "trigger_on": "2,3",
    "trigger_enabled": true,
    "trigger_parameters": {
      "id_users": [
        "2045"
      ],
      "notif_title": "Temperature alert",
      "notif_preview": "Sensor A exceeded threshold",
      "notif_content": "The temperature has exceeded the configured threshold for over 10 minutes.",
      "notif_template": "1"
    }
  },
  "context": null,
  "instance": "/monitors/1/triggers/2"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Element not found",
  "data": null,
  "context": {
    "path": {
      "id_monitor": "1",
      "id_monitor_trigger": "999"
    }
  },
  "instance": "/monitors/1/triggers/999"
}
```

### Status codes
- `200` — Trigger retrieved successfully.
- `400` — Invalid path parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Trigger not found.
- `500` — Internal server error.

### Create trigger

Create a new trigger for the monitor to deliver notifications or automate follow-up actions.

Clearance level 5 or any lower clearance is required to use this endpoint.


### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>",
  "Content-Type": "application/json"
}
```

### Endpoint
```
POST /monitors/{id_monitor}/triggers
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
| `{id_monitor}` | Monitor numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_trigger_type` | yes | int | No | Trigger type identifier. Available values can be retrieved with [GET /types/monitor_triggers](../Types/types.md#monitor-trigger-types) |
| `time_pattern` | no | string | null | Time pattern for scheduled execution. Use `{hours}|{dayweek}|{daymonth}|{months}` where each segment accepts `*`, a single value, or comma-separated values within the ranges hours `00-59`, dayweek `1-7`, daymonth `1-31`, months `1-12`. Example: `00,12|1,3|*|*` runs at 00:00 and 12:00 on Mondays and Wednesdays. |
| `trigger_observations` | no | string | null | Trigger observations |
| `trigger_on` | no | string | '1,3' | Id values of the status that will be communicated. Available status IDs can be retrieved by endpoint [GET /types/monitor_statuses](../Types/types.md#monitor-statuses) |
| `trigger_enabled` | no | bool | true | Whether the trigger is enabled |
| `trigger_parameters` | no | object | null | Additional configuration for the trigger. See [Trigger parameters](#trigger-parameters) for supported keys. |

### Trigger parameters

| Parameter | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_users` | no | array[string] | null | List of user identifiers that receive the notification. |
| `notif_title` | no | string | null | Notification title shown in recipients' inboxes. |
| `notif_preview` | no | string | null | Short preview text for the notification. |
| `notif_content` | no | string | null | Full notification content delivered to recipients. |
| `notif_template` | no | string | 1 | Notification template identifier for predefined copy. |

### Time pattern format

The `time_pattern` attribute describes when the trigger should run. Split the string in four sections using the `{hours}|{dayweek}|{daymonth}|{months}` format:

1. `hours`: values from `00` to `59`. Write `*` to run every minute or `00,12` to run at 00:00 and 12:00.
2. `dayweek`: values from `1` (Monday) to `7` (Sunday). Use comma-separated values when the trigger runs on multiple days.
3. `daymonth`: values from `1` to `31`. Use `*` to run every day of the month.
4. `months`: values from `1` (January) to `12` (December).

Keep each section free of spaces and zero-pad hour values so the system can store and return the pattern without validation errors.

### Sample request body
```json
{
  "id_trigger_type": 1,
  "trigger_on": "2,3",
  "trigger_enabled": true,
  "trigger_parameters": {
    "id_users": [
      "2045"
    ],
    "notif_title": "Temperature alert",
    "notif_preview": "Sensor A exceeded threshold",
    "notif_content": "The temperature has exceeded the configured threshold for over 10 minutes.",
    "notif_template": "1"
  }
}
```

### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"id_trigger_type":1,"trigger_on":"2,3","trigger_enabled":true,"trigger_parameters":{"id_users":["2045"],"notif_title":"Temperature alert","notif_preview":"Sensor A exceeded threshold","notif_content":"The temperature has exceeded the configured threshold for over 10 minutes.","notif_template":"1"}}' \
  /monitors/1/triggers
```

### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_monitor_trigger": 3},
  "context": null,
  "instance": "/monitors/1/triggers/3"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Invalid trigger payload",
  "data": null,
  "context": {
    "body": {
      "trigger_on": "1,8"
    }
  },
  "instance": "/monitors/1/triggers"
}
```

### Status codes
- `201` — Trigger created successfully.
- `400` — Invalid payload or trigger parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Monitor, trigger type, or time pattern not found.
- `500` — Internal server error.

### Update trigger

Update the configuration of an existing trigger.

Clearance level 6 or any lower clearance is required to use this endpoint.


### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>",
  "Content-Type": "application/json"
}
```

### Endpoint
```
PUT /monitors/{id_monitor}/triggers/{id_monitor_trigger}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_trigger}` | Trigger numeric identifier | int |

### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_trigger_type` | no | int | null | Trigger type identifier. Available values can be retrieved with [GET /types/monitor_triggers](../Types/types.md#monitor-trigger-types) |
| `time_pattern` | no | string | null | Time pattern for scheduled execution. Use `{hours}|{dayweek}|{daymonth}|{months}` where each segment accepts `*`, a single value, or comma-separated values within the ranges hours `00-59`, dayweek `1-7`, daymonth `1-31`, months `1-12`. Example: `0,12|1,3|*|*` runs at 00:00 and 12:00 on Mondays and Wednesdays. |
| `trigger_observations` | no | string | null | Trigger observations |
| `trigger_on` | no | string | null | Comma-separated event codes |
| `trigger_enabled` | no | bool | null | Whether the trigger is enabled |
| `trigger_parameters` | no | object | null | Additional configuration for the trigger. See [Trigger parameters](#trigger-parameters) for supported keys. |

### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"id_trigger_type":1,"trigger_on":"2,3","trigger_enabled":true,"trigger_parameters":{"id_users":["2045"],"notif_title":"Temperature alert","notif_preview":"Sensor A exceeded threshold","notif_content":"The temperature has exceeded the configured threshold for over 10 minutes.","notif_template":"1"}}' \
  /monitors/1/triggers/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_monitor_trigger": 3},
  "context": null,
  "instance": "/monitors/1/triggers/3"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Element not found",
  "data": null,
  "context": {
    "path": {
      "id_monitor": "1",
      "id_monitor_trigger": "999"
    }
  },
  "instance": "/monitors/1/triggers/999"
}
```

### Status codes
- `200` — Trigger updated successfully.
- `400` — Invalid payload or trigger parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Trigger not found.
- `500` — Internal server error.

### Delete trigger

Remove a trigger so it no longer runs for the monitor.

Clearance level 5 or any lower clearance is required to use this endpoint.


### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Endpoint
```
DELETE /monitors/{id_monitor}/triggers/{id_monitor_trigger}
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_trigger}` | Trigger numeric identifier | int |

### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/triggers/3
```

### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/monitors/1/triggers/3"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Error responses
```json
{
  "status": "error",
  "message": "Element not found",
  "data": null,
  "context": {
    "path": {
      "id_monitor": "1",
      "id_monitor_trigger": "999"
    }
  },
  "instance": "/monitors/1/triggers/999"
}
```

### Status codes
- `200` — Trigger deleted successfully.
- `400` — Invalid path parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — Trigger not found.
- `500` — Internal server error.

## Monitor templates

Monitor templates provide predefined copy that can be reused when configuring monitor notifications.

Clearance level 2 or any lower clearance is required to use this endpoint.


### Endpoint
```
GET /monitor_templates
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

### Sample headers
```json
{
  "Authorization": "<API_KEY>",
  "Account": "<ID_ACCOUNT>"
}
```

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitor_templates?skip=0&limit=100
```

### Sample response
```json
{
  "status": "success",
  "message": "Monitor templates retrieved",
  "data": [
    {
      "id_monitor_template": 1,
      "template_name": "Default notification template",
      "template_description": "Standard notification copy for monitor alerts."
    }
  ],
  "context": null,
  "instance": "/monitor_templates"
}
```

### Sample response headers
```json
{
  "Content-Type": "application/json"
}
```

### Status codes
- `200` — Monitor templates retrieved successfully.
- `400` — Invalid pagination parameters.
- `401` — Missing or invalid credentials.
- `403` — Clearance level is insufficient.
- `404` — No templates were found.
- `500` — Internal server error.

### Error responses
```json
{
  "status": "error",
  "message": "Monitor templates not available",
  "data": [],
  "context": {
    "errors": [
      "No templates were found for the requested account"
    ]
  },
  "instance": "/monitor_templates"
}
```

## Monitor rules

Monitor rules evaluate incoming metric data and determine the monitor status. Each rule targets a metric, compares values against thresholds, and emits a status that can trigger downstream actions.

### List rules

Retrieve the rules configured on the selected monitor.

Clearance level 7 or any lower clearance is required to use this endpoint.

#### Endpoint
```
GET /monitors/{id_monitor}/rules
```

#### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

#### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

#### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination |
| `limit` | no | int | Max records to return |

#### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/rules?skip=0&limit=100
```

#### Sample response
```json
{
  "status": "success",
  "message": "Monitor rules retrieved",
  "data": [],
  "context": null,
  "instance": "/monitors/1/rules"
}
```

### Get rule

Fetch the configuration of a single monitor rule.

Clearance level 7 or any lower clearance is required to use this endpoint.

#### Endpoint
```
GET /monitors/{id_monitor}/rules/{id_monitor_rule}
```

#### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

#### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_rule}` | Rule numeric identifier | int |

#### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/rules/2
```

#### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {"id_monitor_rule": 2},
  "context": null,
  "instance": "/monitors/1/rules/2"
}
```

### Create rule

Create a new rule to evaluate metric data for the monitor.

Clearance level 5 or any lower clearance is required to use this endpoint.

#### Endpoint
```
POST /monitors/{id_monitor}/rules
```

#### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

#### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

#### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_metric` | yes | int | No | Metric identifier |
| `id_monitor_status` | no | int | 1 | Id value of the status that will be communicated. Available status ids can be retrieved by endpoint [GET /types/monitor_statuses](../Types/types.md#monitor-statuses) |
| `rule_name` | yes | string | No | Name of the rule |
| `rule_description` | no | string | null | Description |
| `rule_method` | yes | int | No | Method to detect values that comply with the rule. Valid ids: `1` greater than, `2` lower than, `3` in range, `4` out of range |
| `rule_lower_limit` | no | number | null | Lower bound |
| `rule_upper_limit` | no | number | null | Upper bound |
| `rule_threshold` | no | int | 100 | Threshold value |

#### Sample request
```bash
curl -X POST \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"id_metric":1,"rule_name":"High value","rule_method":1,"id_monitor_status":1,"rule_threshold":100}' \
  /monitors/1/rules
```

#### Sample response
```json
{
  "status": "success",
  "message": "Element created successfully",
  "data": {"id_monitor_rule": 3},
  "context": null,
  "instance": "/monitors/1/rules/3"
}
```

### Update rule

Update an existing monitor rule to adjust thresholds or status mappings.

Clearance level 6 or any lower clearance is required to use this endpoint.

#### Endpoint
```
PUT /monitors/{id_monitor}/rules/{id_monitor_rule}
```

#### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |
| `Content-Type` | yes | `application/json` | string |

#### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_rule}` | Rule numeric identifier | int |

#### Request body

| Field | Required | Type | Default | Description |
| --- | --- | --- | --- | --- |
| `id_metric` | no | int | null | Metric identifier |
| `id_monitor_status` | no | int | null | Id value of the status that will be communicated. Available status ids can be retrieved by endpoint [GET /types/monitor_statuses](../Types/types.md#monitor-statuses) |
| `rule_name` | no | string | null | Name of the rule |
| `rule_description` | no | string | null | Description |
| `rule_method` | no | int | null | Method to detect values that comply with the rule. Valid ids: `1` greater than, `2` lower than, `3` in range, `4` out of range |
| `rule_lower_limit` | no | number | null | Lower bound |
| `rule_upper_limit` | no | number | null | Upper bound |
| `rule_threshold` | no | int | null | Threshold value |
| `rule_enabled` | no | bool | null | Whether the rule is enabled |

#### Sample request
```bash
curl -X PUT \
  -H "Authorization: <API_KEY>" \
  -H "Account: <ID_ACCOUNT>" \
  -H "Content-Type: application/json" \
  -d '{"rule_enabled":true}' \
  /monitors/1/rules/3
```

#### Sample response
```json
{
  "status": "success",
  "message": "Element updated successfully",
  "data": {"id_monitor_rule": 3},
  "context": null,
  "instance": "/monitors/1/rules/3"
}
```

### Delete rule

Remove a rule so it no longer affects the monitor status.

Clearance level 5 or any lower clearance is required to use this endpoint.

#### Endpoint
```
DELETE /monitors/{id_monitor}/rules/{id_monitor_rule}
```

#### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

#### Path parameters

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |
| `{id_monitor_rule}` | Rule numeric identifier | int |

#### Sample request
```bash
curl -X DELETE -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" /monitors/1/rules/3
```

#### Sample response
```json
{
  "status": "success",
  "message": "Element deleted successfully",
  "data": null,
  "context": null,
  "instance": "/monitors/1/rules/3"
}
```

## Monitor history

Retrieve the historical status changes for a monitor.

Clearance level 7 or any lower clearance is required to use this endpoint.

Monitor history entries are read-only; they cannot be created, updated, or deleted through the API.

If no timestamps are provided, the API returns the last seven days of activity by default. When the `from_timestamp` is greater than the `to_timestamp`, the service automatically swaps them to build a valid range.

### Endpoint
```
GET /monitors/{id_monitor}/history
```

### Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter

| Parameter | Description | Type |
| --- | --- | --- |
| `{id_monitor}` | Monitor numeric identifier | int |

### Query parameters

| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `from_timestamp` | no | int | Inclusive start of the range in UNIX seconds. Defaults to 7 days before the current time when omitted. |
| `to_timestamp` | no | int | Inclusive end of the range in UNIX seconds. Defaults to the current time when omitted. |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: <ID_ACCOUNT>" \
  /monitors/1/history?from_timestamp=1713312000&to_timestamp=1713916800
```

### Sample response
```json
{
  "status": "success",
  "message": "Monitor history retrieved",
  "data": {
    "id_monitor": 1,
    "from_timestamp": 1713312000,
    "to_timestamp": 1713916800,
    "history": [
      {
        "history_timestamp": 1713400000,
        "history_event": "status change"
      }
    ]
  },
  "context": null,
  "instance": "/monitors/1/history"
}
```

### Status codes

| Status | Description |
| --- | --- |
| `200` | Monitor history retrieved successfully. |
| `400` | Invalid timestamp format provided. |
| `401` | Missing or invalid authentication headers. |
| `403` | The authenticated user lacks the required clearance. |
| `404` | Monitor not found for the provided identifier. |
| `500` | Unexpected error while retrieving monitor history. |

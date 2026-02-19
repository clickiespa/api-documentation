# Forms

Index of endpoints that expose form catalogs and input definitions.

- [List forms](#list-forms)
- [Get form](#get-form)
- [List form inputs](#list-form-inputs)
- [Get form input](#get-form-input)
- [List form input types](#list-form-input-types)

## List forms
Return paginated forms available to the authenticated account. Clearance level 7 is required.

### Endpoint
```
GET /forms
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters
| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination. Default: 0 |
| `limit` | no | int | Page size. Default: 100 |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /forms?skip=0&limit=20
```

### Sample response
```json
{
  "status": "success",
  "message": "2 elements obtained successfully",
  "data": [
    {
      "id_form": 5,
      "id_environment": null,
      "id_account": 1,
      "form_name": "Widget config",
      "form_description": "Base form for widget configuration"
    }
  ],
  "context": {"query": {"skip": "0", "limit": "20"}},
  "instance": "/forms"
}
```

## Get form
Retrieve a single form definition. Clearance level 7 is required.

### Endpoint
```
GET /forms/{id_form}
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter
| Parameter | Description | Type |
| --- | --- | --- |
| `{id_form}` | Form identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /forms/5
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_form": 5,
    "id_environment": null,
    "id_account": 1,
    "form_name": "Widget config",
    "form_description": "Base form for widget configuration"
  },
  "context": {"path": {"id_form": "5"}},
  "instance": "/forms/5"
}
```

## List form inputs
List inputs that belong to a given form. Clearance level 7 is required.

### Endpoint
```
GET /form_inputs
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters
| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `id_form` | no | int | Limit the result to a specific form |
| `skip` | no | int | Offset for pagination. Default: 0 |
| `limit` | no | int | Page size. Default: 100 |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" \
  "/form_inputs?id_form=5&skip=0&limit=50"
```

### Sample response
```json
{
  "status": "success",
  "message": "3 elements obtained successfully",
  "data": [
    {
      "id_form_input": 14,
      "id_form": 5,
      "id_input_type": 1,
      "input_name": "title",
      "input_description": "Widget title",
      "resource_type": null,
      "input_label": "Title",
      "input_default": null,
      "input_placeholder": "Line chart title",
      "input_select_options": null,
      "input_multiple": false,
      "input_attributes": null,
      "input_class": null,
      "input_order": 10,
      "input_required": true
    }
  ],
  "context": {"query": {"id_form": "5", "skip": "0", "limit": "50"}},
  "instance": "/form_inputs"
}
```

## Get form input
Fetch metadata for a single form input. Clearance level 7 is required.

### Endpoint
```
GET /form_inputs/{id_form_input}
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Path parameter
| Parameter | Description | Type |
| --- | --- | --- |
| `{id_form_input}` | Form input identifier | int |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /form_inputs/14
```

### Sample response
```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {
    "id_form_input": 14,
    "id_form": 5,
    "id_input_type": 1,
    "input_name": "title",
    "input_description": "Widget title",
    "resource_type": null,
    "input_label": "Title",
    "input_default": null,
    "input_placeholder": "Line chart title",
    "input_select_options": null,
    "input_multiple": false,
    "input_attributes": null,
    "input_class": null,
    "input_order": 10,
    "input_required": true
  },
  "context": {"path": {"id_form_input": "14"}},
  "instance": "/form_inputs/14"
}
```

## List form input types
Return the catalog of available input types. Clearance level 7 is required.

### Endpoint
```
GET /form_input_types
```

### Headers
| Header | Required | Description | Type |
| --- | --- | --- | --- |
| `Authorization` | yes | API key from your profile | string |
| `Account` | yes | Target account ID | int |

### Query parameters
| Parameter | Required | Type | Description |
| --- | --- | --- | --- |
| `skip` | no | int | Offset for pagination. Default: 0 |
| `limit` | no | int | Page size. Default: 100 |

### Sample request
```bash
curl -H "Authorization: <API_KEY>" -H "Account: 1" /form_input_types?skip=0&limit=100
```

### Sample response
```json
{
  "status": "success",
  "message": "11 elements obtained successfully",
  "data": [
    {"id_input_type": 1, "type_name": "text"},
    {"id_input_type": 2, "type_name": "select"}
  ],
  "context": {"query": {"skip": "0", "limit": "100"}},
  "instance": "/form_input_types"
}
```

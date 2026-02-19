# Getting Started

This guide introduces the basics of the Clickie API v4.

## Host

```
https://api.clickie.io/v4/
```

Use this base URL with the relative paths shown throughout the documentation.
All `curl` examples omit the host, so prepend this base to each path.

## HTTP Methods

| Method | Description |
| --- | --- |
| `GET` | Retrieves resources |
| `POST` | Creates resources |
| `PUT` | Updates resources |
| `DELETE` | Deletes resources |

## Authentication

### Authorization header

Use an API key in the `Authorization` header. Keys can be generated from your profile: https://my.clickie.io/profile/api-keys

### Account header

Most endpoints, including the `/accounts` endpoint, require an `Account` header with the ID of the account you want to operate on. If you don't know your account ID, you can find it in the Clickie web app: https://my.clickie.io/home.

Once you know your account ID, you can list your accounts using:

```
GET /accounts
```

### How account scoping is applied on creation

The API Gateway authorizer injects the `Account` header value into the session (`id_account`), and the data access layer automatically assigns that `id_account` to new records that include an `id_account` field. If a payload explicitly provides an `id_account`, it is used instead. This defaulting only applies to models other than `Account` and `UserAccount`.

### API Key

API keys authenticate a collaborator and define permissions.

## Common Headers

| Header | Required | Description | Type |
| --- | --- | --- | --- |
| Authorization | yes | API key generated from your profile | string |
| Account | yes | Target account ID | int |

## Formatting a request

### Path parameters

Resource identifiers appear in the URI when requesting a specific record.

```
GET /monitors/{id_monitor}/triggers
```

### Request body

`POST` and `PUT` requests use JSON payloads.

```
POST /uoms
```

```json
{
  "id_aggregation": 1,
  "id_interpolation": 1,
  "uom_name": "string",
  "uom_description": "string",
  "uom_unit": "string"
}
```

## Responses

Responses follow a common JSON structure:

```json
{
  "status": "success",
  "message": "Element obtained successfully",
  "data": {},
  "context": {},
  "instance": "/endpoint/path"
}
```

### Status Codes

| Code | Description |
| --- | --- |
| `200` | Success |
| `201` | Created |
| `400` | Bad request |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Not Found |
| `500` | Internal Server Error |

### Successful request

| Method | Response |
| --- | --- |
| `GET` | Returns resource data |
| `POST` | Returns created object |
| `PUT` | Returns updated object |
| `DELETE` | Returns deletion confirmation |

## API Tools

### Docs endpoint

Interactive documentation is accessible at:

```
/docs
```

To use it, follow the next steps:

1. Click [here](https://api.clickie.io/v4/docs#).
2. Click the Authorize button at the top right of the page and manually add the Account  and Authorization headers.
3. Click the endpoint you want to use, fill the required fields, and click the “Try it out” button to make the request.
4. The server's response will be displayed below.

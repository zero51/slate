---
title: 1MT Authentication API

language_tabs:
  - http: HTTP
  - shell: cURL

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - base
  - users
  - subscriptions
  - accounts
  - notifications

search: true
---

# Introduction

Welcome to the 1MT Authentication API. Use this API to manage the syncing of user authentication data from your MT platform to the 1MT authentication service.

<aside class="notice">
The service can be found at: <code>https://id.markettrack.com</code>.
</aside>

# Authentication

All calls to the API require a valid access token. Once granted (provided they are not revoked), tokens are valid for 12 hours.

<aside class="notice">
Calls authenticated with invalid credentials will result in a <code>401</code> error.
</aside>

## Requesting access tokens

> Request an access token:

```http
POST /auth/token HTTP/1.1
Accept: application/json
Authorization: Basic Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "grant_type": "client_credentials"
}
```

```shell
curl /auth/token 
  -H "Content-Type: application/json" 
  -X POST 
  -u identifier:secret
  -d '{"grant_type": "client_credentials"}'
```

> A successful response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4
```

```json
{
	"access_token": "216fee3f-404d-4663-b32a-dcb765020125",
	"expires_at": "2017-02-01T16:59:20.112Z",
	"token_type": "Bearer"
}
```

Access tokens are granted at the end of an [OAuth2 client-credentials](https://tools.ietf.org/html/rfc6749#section-4.4) handshake. To request a new token, use [HTTP basic auth](https://tools.ietf.org/html/rfc2617#section-2) with your platform ID and secret to POST to the `/auth/token` endpoint. The `grant_type` body parameter should be set to 'client_credentials'.

`"grant_type": "client_credentials"`

If the token request is valid and authorised, the access token is returned in the JSON body of the response, along with details of its expiry.

Invalid token requests result in a `401` error. If encountered, check the platform credentials that were supplied.

**A note on case**  
In order to conform to OAuth2 specifications, all request/response parameters specific to authentication are in `snake_case`. This is different to the wider 1MT API convention, where all such parameters are presented in `camelCase`.

### HTTP request

`POST /auth/token`

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
grant_type | string

## Using access tokens

> Authenticate a request:

```http
GET /api/v1 HTTP/1.1
Accept: application/json
Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1 
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
```

In possession of a valid access token, you can make requests to the API. Credentials should be passed as [bearer tokens](https://tools.ietf.org/html/rfc6750#section-2.1) in the request header...

`Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125`

<aside class="notice">
Replace <code>216fee3f-404d-4663-b32a-dcb765020125</code> with your access code.
</aside>

Recall that invalid or expired tokens will result in a `401: Bad Request` error.

# Using the API

## Status codes

The 1MT Authentication API uses the following status codes:

Code | Description
---- | -----------
[200](https://httpstatuses.com/200) | OK: the request succeeded
[201](https://httpstatuses.com/201) | Created: one or more resources created
[204](https://httpstatuses.com/204) | No Content: success, but nothing to return
[400](https://httpstatuses.com/400) | Bad Request: malformed or invalid request
[401](https://httpstatuses.com/401) | Unauthorized: the access token is invalid
[403](https://httpstatuses.com/403) | Forbidden: insufficient permissions
[404](https://httpstatuses.com/404) | Not Found: the requested resource could not be found
[406](https://httpstatuses.com/406) | Not Acceptable: a format was requested that isn't JSON
[409](https://httpstatuses.com/409) | Conflict: internal resource state conflict
[410](https://httpstatuses.com/410) | Gone: the requested resource has been removed
[415](https://httpstatuses.com/415) | Unsupported Media Type: the request's payload is not JSON
[422](https://httpstatuses.com/422) | Unprocessable Entity: validation failed
[500](https://httpstatuses.com/500) | Internal Server Error: unexpected API error
[503](https://httpstatuses.com/503) | Service Unavailable: API temporarily offline

## Making requests

All requests to the API should be over HTTPS.

<aside class="notice">
The API can be found at: <code>https://id.markettrack.com/api/v1</code>.
</aside>

### HTTP verbs

The API uses RESTful design patterns. As such, it implements the following HTTP verbs:

Verb | Description
---- | -----------
GET | Read resource(s)
POST | Create new resource
PATCH | Modify existing resource
DELETE | Remove resource

### Content type

When making requests, ensure that the `Accept` header is set to `application/json` and, when relevant, make sure that the `Content-Type` is also set to `application/json`.

### User agent

Set the user agent according to the platform name and current version.

`User-Agent: PriceVision/1.0.0`

## Responses

> A successful response:

```json
{
  "succeeded": true,
  "message": "User created: me@here.com",
  "data": {
    "name": "Me",
    "email": "me@here.com"
  }
}
```

> An unsuccessful response:

```json 
{
  "succeeded": false,
  "message": "RAML validation error",
  "data": {
    "fields": [{
      "field": "firstName",
      "value": "null",
      "error": "invalid json (required, true)"
    }]
  }
}
```

Outside of [authentication](#authentication), all API responses have a similar JSON payload. 

Key | Type | Description
--- | ---- | -----------
succeeded | boolean | Whether or not the request was successful
message | string | A short description of the response
data | object | An optional object describing the result in detail

### Errors

Unsuccessful responses can include details on the error in the `data` object. For example, a validation error would include each invalid field and describe why the validation failed.

### Request ID

Each request to the API is given a unique request ID. This is passed back in the response via the `X-Request-Id` header...

`x-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4`

If you want to pass on your own request ID as part of a communication chain, then supply a similar header in the request. When such a header is identified, the API will not create a new ID, but will instead pass back the supplied ID in the response.

## Fields

### IDs

All resource IDs are represented in [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) format...

`"id": "4a8e9a36-c902-42c5-bbd6-19d222acf178"`

### Timestamps

All timestamps are returned in [ISO8601](https://www.w3.org/TR/NOTE-datetime) format, in UTC, with fields ending in the postfix **_at** or **At**...

`"expires_at": "2016-12-01T00:55:47Z"`

`"createdAt": "2016-12-01T00:55:47Z"`

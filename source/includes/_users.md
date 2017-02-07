# Users API

Available at `/api/v1/users`.

## User resource

> Example user payload:

```json
{
  "id": "da2d3e9a-e84e-11e6-bf01-fe55135034f3",
  "firstName": "Fred",
  "lastName": "Flintstone",
  "email": "fred@bedrock.com",
  "status": "active",
  "internal": false,
  "createdAt": "2016-12-01T00:55:47Z",
  "updatedAt": "2016-12-01T00:55:47Z",
  "subscription": {
    "id": "f5e9dc78-e84f-11e6-bf01-fe55135034f3",
    "status": "active",
    "default": false,
    "createdAt": "2016-12-01T00:55:47Z",
    "updatedAt": "2016-12-01T00:55:47Z",
    "accounts": [{
      "id": "f9a5b170-e84f-11e6-bf01-fe55135034f3",
      "identifier": "fred",
      "name": "Fred's account",
      "status": "active",
      "default": false,
      "createdAt": "2016-12-01T00:55:47Z",
      "updatedAt": "2016-12-01T00:55:47Z"
    }]
  }
}
```

Attribute | Type | Description
--------- | ---- | -----------
id | string | User ID
firstName | string
lastName | string
email | string | /[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}/
status | string | One of: active, inactive, orphaned, pending
internal | boolean | Internal user?
createdAt | datetime
updatedAt | datetime
subscription | [Subscription](#subscription-resource) | User's subscription to current platform

### User status

A user can have one of four states.

Status | Description
------ | -----------
active | The user is allowed to authenticate
inactive | The user has been disabled and cannot authenticate
orphaned | The user has been addedm but has no subscriptions
pending | The user has been added, but has yet to validate their email address

## Create user

> Create user:

```http
POST /api/v1/users HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "firstName": "Fred",
  "lastName": "Flintstone",
  "email": "fred@bedrock.com"
}
```

```shell
curl /api/v1/users
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X POST 
  -d '{"firstName": "Fred", "lastName": "Flintstone", "email": "fred@bedrock.com"}'
```

> A successful response:

```http
HTTP/1.1 201 OK
Content-Type: application/json
X-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4
```

```json
{
  "succeeded": true,
  "message": "User created: da2d3e9a-e84e-11e6-bf01-fe55135034f3",
  "data": {
    "user": "payload"
  }
}
```

Create a new user (and optionally a subscription and account). 

**Note**  
If an account identifier is given, a subscription and an accompanying account is also created.

### HTTP request

`POST /api/v1/users`

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
firstName | string
lastName | string
email | string
internal? | boolean | false
account.identifier? | string | null

## Get user

> Get user:

```http
GET /api/v1/users/fred@bedrock.com HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```http
GET /api/v1/users/da2d3e9a-e84e-11e6-bf01-fe55135034f3 HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
```

```shell
curl /api/v1/users/da2d3e9a-e84e-11e6-bf01-fe55135034f3
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
```

> A successful response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4
```

```json
{
  "succeeded": true,
  "message": "User found: fred@bedrock.com",
  "data": {
    "user": "payload"
  }
}
```

Get details of a specific user.

### HTTP request

`GET /api/v1/users/:userIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

## Update user

> Update user:

```http
PATCH /api/v1/users/fred@bedrock.com HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "firstName": "Wilma"
}
```

```shell
curl /api/v1/users/fred@bedrock.com
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X PATCH
  -d '{"firstName": "Wilma"}'
```

> A successful response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4
```

```json
{
  "succeeded": true,
  "message": "User updated: fred@bedrock.com",
  "data": {
    "user": "payload"
  }
}
```

Update an existing user. 

**Note**  
At least one body parameter is required.

### HTTP request

`PATCH /api/v1/users/:userIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
firstName? | string | null
lastName? | string | null

## Delete user

> Delete user:

```http
DELETE /api/v1/users/fred@bedrock.com HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -X DELETE
```

> A successful response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-Id: ea49faf8-f7ad-48fa-b72a-e659db0f16e4
```

```json
{
  "succeeded": true,
  "message": "User deleted: da2d3e9a-e84e-11e6-bf01-fe55135034f3"
}
```

Delete an existing user.

**Note**  
The operation will fail with a `409` error if the user is subscribed to other platforms.

### HTTP request

`DELETE /api/v1/users/:userIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

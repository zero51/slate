# Users API

Available at `/api/v1/users`.

## User resource

> Example user payload:

```json
{
  "id": "da2d3e9a-e84e-11e6-bf01-fe55135034f3",
  "first_name": "First",
  "last_name": "Last",
  "email": "me@here.com",
  "status": "active",
  "internal": false,
  "created_at": "2016-12-01T00:55:47Z",
  "updated_at": "2016-12-01T00:55:47Z",
  "subscription": {
    "id": "f5e9dc78-e84f-11e6-bf01-fe55135034f3",
    "status": "active",
    "created_at": "2016-12-01T00:55:47Z",
    "updated_at": "2016-12-01T00:55:47Z",
    "accounts": [{
      "id": "f9a5b170-e84f-11e6-bf01-fe55135034f3",
      "identifier": "me@here.com",
      "status": "active",
      "created_at": "2016-12-01T00:55:47Z",
      "updated_at": "2016-12-01T00:55:47Z"
    }]
  }
}
```

Attribute | Type | Description
--------- | ---- | -----------
id | string | User ID
first_name | string
last_name | string
email | string | /[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}/
status | string | One of: active, inactive, pending
internal | boolean | Internal user?
created_at | datetime
updated_at | datetime
subscription | [Subscription](#subscription-resource) | User's subscription to current platform

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
  "first_name": "First",
  "last_name": "Last",
  "email": "me@here.com"
}
```

```shell
curl /api/v1/users
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X POST 
  -d '{"first_name": "First", "last_name": "Last", "email": "me@here.com"}'
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

**Note.** New users are initially in a 'pending' state.

**Note.** If an account identifier is given, a subscription is also created.

### HTTP request

`POST https://id.markettrack.com/api/v1/users`

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
first_name | string
last_name | string
email | string
internal? | boolean | false
account.identifier? | string | null

## Get user

> Get user:

```http
GET /api/v1/users/me@here.com HTTP/1.1
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
curl /api/v1/users/me@here.com
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
  "message": "User found: me@here.com",
  "data": {
    "user": "payload"
  }
}
```

Get details of a specific user.

### HTTP request

`GET https://id.markettrack.com/api/v1/users/:identifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
identifier | string | Email or ID of user

## Update user

Update an existing user. 

**Note.** Only 'safe' user fields such as first and last name can be updated.

**Note.** At least one body parameter is required.

### HTTP request

`PATCH https://id.markettrack.com/api/v1/users/:identifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
identifier | string | Email or ID of user

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
first_name? | string | null
last_name? | string | null

## Delete user

Delete an existing user.

**Note.** The operation will fail if the user is subscribed to other platforms.

### HTTP request

`DELETE https://id.markettrack.com/api/v1/users/:identifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
identifier | string | Email or ID of user

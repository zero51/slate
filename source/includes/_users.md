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
      "id": "",
      "identifier": "f9a5b170-e84f-11e6-bf01-fe55135034f3",
      "status": "active",
      "created_at": "2016-12-01T00:55:47Z",
      "updated_at": "2016-12-01T00:55:47Z"
    }]
  }
}
```

Attribute | Type | Description
--------- | ---- | -----------
first_name | string
last_name | string
email | string | /[a-z0-9._%+-]+@[a-z0-9.-]+\\.[a-z]{2,}/
status | string | One of: active, inactive, pending
internal | boolean | Internal user
created_at | datetime
updated_at | datetime
subscription? | Subscription | Included if `extended` param truthy

## Get user

> Get user:

```http
GET /api/v1/users/me@here.com HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: accounts.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/me@here.com
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
    "as": "above"
  }
}
```

Get details of specific user.

### HTTP request

`GET https://accounts.markettrack.com/api/v1/users/:identifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
identifier | string | Email or ID of user

### Query parameters

Parameter | Type | Default | Description
--------- | ---- | ------- | -----------
extended | boolean | true | If truthy, include subscription details


## Create user

## Update user

## Delete user

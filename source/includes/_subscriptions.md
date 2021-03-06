# Subscriptions API

A subscription defines a user's ability to access a platform. A user can have multiple subscriptions, but can only have one to each platform. If a user has more than one subscription, one of them can be flagged as being the default one.

Available at `/api/v1/users/:userIdentifier/subscription`.

## Subscription resource

> Example subscription payload:

```json
{
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
```

Attribute | Type | Description
--------- | ---- | -----------
id | string | Subscription ID
status | string | One of: active, inactive
default | boolean | Default subscription?
createdAt | datetime
updatedAt | datetime
accounts | [Account](#account-resource)[] | Accounts associated to the subscription

<aside class="notice">
A user can only have one subscription per platform.
</aside>

## Create subscription

> Create subscription:

```http
POST /api/v1/users/fred@bedrock.com/subscription HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{}
```

```shell
curl /api/v1/users/fred@bedrock.com/subscription
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X POST 
  -d '{}'
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
  "message": "Subscription created: f5e9dc78-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "subscription": "payload"
  }
}
```

Create a new subscription (and optionally an associated account). 

**Notes**  
If an account identifier is given, an accompanying account is also created.  
A user can only have one default subscription.

### HTTP request

`POST /api/v1/users/:userIdentifier/subscription`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
default | boolean | false
account.identifier? | string | null

## Get subscription

> Get subscription:

```http
GET /api/v1/users/fred@bedrock.com/subscription HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com/subscription
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
  "message": "Subscription found: f5e9dc78-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "subscription": "payload"
  }
}
```

Get details of the subscription associated to the given user and platform.

### HTTP request

`GET /api/v1/users/:userIdentifier/subscription`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

## Update subscription

> Update subscription:

```http
PATCH /api/v1/users/fred@bedrock.com/subscription HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "status": "inactive"
}
```

```shell
curl /api/v1/users/fred@bedrock.com/subscription
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X PATCH
  -d '{"status": "inactive"}'
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
  "message": "Subscription updated: f5e9dc78-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "subscription": "payload"
  }
}
```

Update an existing subscription. 

**Notes**  
At least one body parameter is required.  
A user can only have one default subscription.

### HTTP request

`PATCH /api/v1/users/:userIdentifier/subscription`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
status? | string | null
default? | boolean | null

## Delete subscription

> Delete subscription:

```http
DELETE /api/v1/users/fred@bedrock.com/subscription HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com/subscription
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
  "message": "Subscription deleted: f5e9dc78-e84f-11e6-bf01-fe55135034f3"
}
```

Delete an existing subscription.

### HTTP request

`DELETE /api/v1/users/:userIdentifier/subscription`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

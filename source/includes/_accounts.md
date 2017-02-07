# Accounts API

Available at `/api/v1/users/:userIdentifier/accounts`.

## Account resource

> Example account payload:

```json
{
	"id": "f9a5b170-e84f-11e6-bf01-fe55135034f3",
	"identifier": "fred",
	"name": "Fred's account",
	"status": "active",
	"default": false,
	"createdAt": "2016-12-01T00:55:47Z",
	"updatedAt": "2016-12-01T00:55:47Z"
}
```

Attribute | Type | Description
--------- | ---- | -----------
id | string | Subscription ID
identifier | string | Unique account identifier (e.g. username)
status | string | One of: active, inactive
default | boolean | Default account?
createdAt | datetime
updatedAt | datetime

## Create account

> Create account:

```http
POST /api/v1/users/fred@bedrock.com/accounts HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "identifier": "fred"
}
```

```shell
curl /api/v1/users/fred@bedrock.com/accounts
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X POST 
  -d '{"identifier": "fred"}'
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
  "message": "Account created: f9a5b170-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "account": "payload"
  }
}
```

Create a new account. 

**Note**  
A subscription can only have one default account.

### HTTP request

`POST /api/v1/users/:userIdentifier/accounts`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
identifier | string
name? | string 
default? | boolean | false

## Get accounts

> Get accounts:

```http
GET /api/v1/users/fred@bedrock.com/accounts HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com/accounts
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
  "message": "Accounts found: 2",
  "data": [{
    "account": "payload"
  }, {
    "account": "payload"
  }]
}
```

Get details of all accounts belonging to the given subscription.

### HTTP request

`GET /api/v1/users/:userIdentifier/accounts`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user

## Get account

> Get account:

```http
GET /api/v1/users/fred@bedrock.com/accounts/fred HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com/accounts/fred
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
  "message": "Account found: f9a5b170-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "account": "payload"
  }
}
```

Get details of given account.

### HTTP request

`GET /api/v1/users/:userIdentifier/accounts/:accountIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user
accountIdentifier | string | Identifier or ID of account

## Update account

> Update account:

```http
PATCH /api/v1/users/fred@bedrock.com/accounts/fred HTTP/1.1
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
curl /api/v1/users/fred@bedrock.com/accounts/fred
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
  "message": "Account updated: f5e9dc78-e84f-11e6-bf01-fe55135034f3",
  "data": {
    "account": "payload"
  }
}
```

Update an existing account. 

**Notes**  
At least one body parameter is required.  
A subscription can only have one default account.

### HTTP request

`PATCH /api/v1/users/:userIdentifier/accounts/:accountIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user
accountIdentifier | string | Identifier or ID of account

### Body parameters

Parameter | Type | Default
--------- | ---- | -------
identifier? | string
name? | string
status? | string
default? | boolean

## Delete account

> Delete account:

```http
DELETE /api/v1/users/fred@bedrock.com/accounts/fred HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1/users/fred@bedrock.com/accounts/fred
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
  "message": "Account deleted: f5e9dc78-e84f-11e6-bf01-fe55135034f3"
}
```

Delete an existing account.

### HTTP request

`DELETE /api/v1/users/:userIdentifier/accounts/:accountIdentifier`

### URL parameters

Parameter | Type | Description
--------- | ---- | -----------
userIdentifier | string | Email or ID of user
accountIdentifier | string | Identifier or ID of account

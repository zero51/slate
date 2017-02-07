# Notifications API

Available at `/api/v1/notifications`.

## Notification resource

> Example notification payload:

```json
{
  "id": "e6750448-0481-4ec4-b48d-a87fa7187cf5",
  "type": "welcome",
  "sentAt": "2016-12-01T00:55:47Z",
  "recipient": {
    "id": "da2d3e9a-e84e-11e6-bf01-fe55135034f3",
    "firstName": "Fred",
    "lastName": "Flintstone",
    "email": "fred@bedrock.com",
    "status": "active",
    "internal": false,
    "createdAt": "2016-12-01T00:55:47Z",
    "updatedAt": "2016-12-01T00:55:47Z"
  }
}
```

Attribute | Type | Description
--------- | ---- | -----------
id | string | Message ID
type | string | Only 'welcome' currently supported
sentAt | datetime
recipient | [User](#user-resource) | The recipient of the notification

### Notification type

Type | Description
---- | -----------
welcome | Welcomes user to a platform, with password link if applicable

## Create notification

> Create account:

```http
POST /api/v1/notifications HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Content-Type: application/json
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0

{
  "user": "fred@bedrock.com",
  "type": "welcome"
}
```

```shell
curl /api/v1/notifications
  -H "Authorization: Bearer 216fee3f-404d-4663-b32a-dcb765020125"
  -H "Content-Type: application/json"   
  -X POST 
  -d '{"user": "fred@bedrock.com", "type": "welcome"}'
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
  "message": "Notification sent: fred@bedrock.com",
  "data": {
    "notification": "payload"
  }
}
```

Create a new notification and queue for delivery. 

### HTTP request

`POST /api/v1/notifications`

### Body parameters

Parameter | Type | Description
--------- | ---- | -----------
user | string | Email or ID of user
type | string | Only 'welcome' currently supported

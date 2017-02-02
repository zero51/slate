# Base API

Available at `/api/v1`.

## Hello

> Check API status:

```http
GET /api/v1 HTTP/1.1
Accept: application/json
Authorization: Bearer Y3ItZWNvbW06YTljZTFhMWYtOWI1ZS00ZDllLTkzYjctN2VmMTBmZWRiZDYz
Host: id.markettrack.com
User-Agent: PriceVision/1.0.0
```

```shell
curl /api/v1
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
  "message": "Hello from 1MT resource server"
}
```

Check status of API.

### HTTP request

`GET https://id.markettrack.com/api/v1`

# `GET /pivot/logs?`

This endpoint will return the logs for a specific call. This is raw diagnostic data useful for troubleshooting when a call fails.

- Path :: `/pivot/logs?requestId=1609224d-374f-4570-8078-02255e7b19b3`
- Method :: `GET`
- Querystring
  - `requestId` - *(String, optional)* The UUID returned in the response
  - `echo` - *(String, optional)* The echo value returned in the response
  - **One of `requestId` OR `echo` is required**

## Example

**Request**
```bash
curl \
  'https://api-test.paxiq.com/pivot/logs?requestId=1609224d-374f-4570-8078-02255e7b19b3'
```

**Response**
```json
[
  {
    "_id": "5b1032826219af55f1b49324",
    "event": "request",
    "timestamp": 1527788160905,
    "tags": [
      "1609224d-374f-4570-8078-02255e7b19b3",
      "foobar"
    ],
    "data": "sending command :: *tjcbxt~x",
    "pid": 21544,
    "id": "1527788160904:app01:21544:jhutcxkp:10001",
    "method": "get",
    "path": "/pivot/pnr",
    "config": {},
    "host": "app01",
    "app": "pivot"
  },
  ...
]
```

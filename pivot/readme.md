# Pivot API

The Pivot API serves as a translation and standardization layer between the Videcom command-line API and client applications.

Pivot provides REST endpoints for some of the more common interactions with Videcom that are typically conducted via constructing complex strings of VARS commands as well as a standardized data models across all endpoints.

## Authentication

Currently, Pivot uses a pass-through model for authentication with Videcom. Each request sent to Pivot must contain two headers:
- `Videcom-Token`
- `Videcom-Airline`

These headers are mapped into the request that is sent to Videcom along with the constructed command.

**To use the Pivot API, you must authorize the `35.197.78.251` with Videcom!**

## Versioning

The Pivot API makes use of an optional `Pivot-Version` header for each endpoint. If omitted, the most current version is used by default.

Pivot versions each endpoint independently based on the need to change the data models returned.

## Call Tracing

Each request to Pivot can contain a `Pivot-Echo` header that *should* be a unique string that client applications can use to help troubleshoot interactions with Pivot. The value in the `Pivot-Echo` header is returned in the response as `echo`.

Each response will also include a `requestId` property which is necessary for diagnosing & troubleshooting individual calls. The `requestId` value is unique and generated automatically with each call;

# Endpoints

## `POST /pivot/command`

This endpoint provides an interface directly to the Videcom command-line. You can construct your own commands and submit them via this endpoint.

- Path :: `/pivot/command`
- Method :: `POST`
- Payload
  - `command` - *(String, required)* The standard Videcom command-line command you want to submit.

### Example

**Request**
```bash
curl \
  -X POST \
  -H "Videcom-Token: <<TOKEN>>" \
  -H "Videcom-Airline: <<AIRLINE>>" \
  -H "Pivot-Version: 1.0" \
  -H "Pivot-Echo: foo" \
  -H "Content-Type: application/json" \
  -d '{"command":"*abc123"}' \
  'https://api-test.paxiq.com/pivot/command'
```

**Response**
```json
{
  "requestId": "78dc987e-beed-4136-99ea-f11d8e9fad34",
  "echo": "foo",
  "version": "1.0",
  "result": {
    "command": "*abc123",
    "raw": "<?xml version=\"1.0\" encoding=\"utf-8\"?><soap:Envelope xmlns:soap=\"http://www.w3.org/2003/05/soap-envelope\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\"><soap:Body><RunVRSCommandResult xmlns=\"http://videcom.com/\">ERROR - RECORD NOT FOUND - ABC123</RunVRSCommandResult></soap:Body></soap:Envelope>",
    "body": "ERROR - RECORD NOT FOUND - ABC123",
    "json": null
  }
}
```

## `GET /pivot/pnr`

This endpoint will return the PNR from Videcom. The response payload

- Path :: `/pnr?rloc=abc123`
- Method :: `GET`
- Querystring
  - `rloc` - *(String, required)* The standard 6-character code for the PNR that you want to read.


### Example

**Request**
```bash
curl \
  -X GET \
  -H "Videcom-Token: <<TOKEN>>" \
  -H "Videcom-Airline: <<AIRLINE>>" \
  -H "Pivot-Version: 1.0" \
  -H "Pivot-Echo: foobar" \
  'https://api-test.paxiq.com/pivot/pnr?rloc=abc123'
```

**Response**
```json
{
  "requestId": "5c86a508-dd28-4588-a87a-0c954d2b4b16",
  "echo": "foobar",
  "version": "1.0",
  "result": {
    "command": "*abc123~x",
    "raw": "<PNR RLOC=\"ABC123\" PNRLocked=\"False\" ...>...</PNR>",
    "pnr": {
      "rloc": "ABC123",
      "locked": false,
      ... // omitted for brevity
    }
  }
}
```

## `GET /pivot/availability`

This endpoint will return the flight availability data for the specified querystring values.

- Path :: `/pivot/availability`
- Method :: `GET`
- Querystring
  - `date` - *(String, required)* The date that you want to look for an availability. Format can be any supported by Moment (https://momentjs.com/docs/#/parsing/string/)
  - `depart` - *(String, required)* IATA code for the point of departure.
  - `arrive` - *(String, required)* IATA code for the point of arrival.
  - `passengers` - *(Number, optional, default = 1)* The number of ticketed passengers.

### Example

**Request**
```bash
curl \
  -X GET \
  -H "Videcom-Token: <<TOKEN>>" \
  -H "Videcom-Airline: <<AIRLINE>>" \
  -H "Pivot-Version: 1.0" \
  -H "Pivot-Echo: bar" \
  'https://api-test.paxiq.com/pivot/availability?date=2018-05-05&depart=pdx&arrive=sea'
```

**Response**
```json
{
  "requestId": "bed85caa-a0e0-44c8-b80b-b220357ca202",
  "echo": "bar",
  "version": "1.0",
  "result": {
    "command": "A05MAYPDXSEA[SALESCITY=PDX,VARS=TRUE,CLASSBANDS=TRUE,STARTCITY=PDX,SINGLESEG=S,FGNOAV=TRUE,QTYSEATS=1]",
    "_raw": "<xml><classbands><band .../></classbands><itin ...></itin><cal><day .../></cal></xml>",
    "availability": [
      ...
    ]
  }
}
```

# Feedback

Please submit feedback to [Ben](mailto:ben@paxiq.com).

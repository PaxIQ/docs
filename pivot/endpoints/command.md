# `POST /pivot/command`

This endpoint provides an interface directly to the Videcom command-line. You can construct your own commands and submit them via this endpoint.

- Path :: `/pivot/command`
- Method :: `POST`
- Payload
  - `command` - *(String, required)* The standard Videcom command-line command you want to submit.

## Example

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

## Data Modeling

Since the `/pivot/command` endpoint exposes the standard Videcom command line interface, we have to use a generic data model for returning responses.

## Raw

The `result.raw` value is just that, the raw, untouched response from Videcom.

## Body

The `result.body` value is what we find between the `<RunVRSCommandResult>` elements in the `result.raw`.

## JSON

If the `result.body` is XML, Pivot will parse it into JSON using the `pixl-xml` library with a `{ forceArrays: true }` option.

# Command

This endpoint provides an interface directly to the Videcom command-line. You can construct your own commands and submit them via this endpoint.

- Path :: `/pivot/1.0/command`
- Method :: `POST`
- Payload
  - `command` - *(String, required)* The standard Videcom command-line command you want to submit.

## Sample Request

```bash
curl \
  -X POST \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  -H "Content-Type: application/json" \
  -d '{"command":"*ABC123~x"}' \
  https://api-test.paxiq.com/pivot/1.0/command
```

## Sample Response

```js
{
  "requestId": "6edff1f2-bf56-45ca-af34-5d60edf252bc",
  "echo": null,
  "result": {
    "command": "*AAH5TV~x",
    "raw": "<PNR  ... </PNR>",
    "json": {
      "RLOC": "AAH5TV",
      ...
    }
  }
}

```

## Data Modeling

Since the `/pivot/command` endpoint exposes the standard Videcom command line interface, we have to use a generic data model for returning responses.

## Command

This property shows the command that was submitted directly to Videcom.

## Raw

The `result.raw` value is just that, the raw, untouched response from Videcom. For many commands, appending `~x` will cause Videcom to return an XML formatted response.

## JSON

Pivot will attempt to convert the raw response from XML to JSON. If the raw response is not in XML format, this property will be `null`.

# SSRP

This endpoint will return the results of an SSRP query.

- SSRP
  - [Read](#ssrp-read)

## SSRP Read

The SSRP endpoint uses dynamic URL parameters. The first parameter after `ssrp/` is the name of the report you wish to run. Subsequent parameters are applied directly to the report interface.  For example, if you were to run a query like `ssrpFoo/bar/baz` at the command line, the URL parameters would look like `ssrp/foo/bar/baz`. If `ssrpFoo` takes no parameters, the URL becomes `ssrp/foo`.

There is an optional `format` querystring parameter that allows you to specify the response format of the data. Omitting this parameter will return whatever the report has set as it's default. Valid options are:
- `json`
- `xml`
- `csv`
- `html`

### Example Request

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  'https://api-test.paxiq.com/pivot/1.0/ssrp/tkt/01AUG/01SEP?format=json'
```

## Example Response

```json
{
  "requestId": "914645a2-f042-4c50-8265-a00ed44157fb",
  "echo": null,
  "result": {
    "command": "ssrptkt/01AUG/01SEP[x/]",
    "format": "json",
    "data": [
      {
        "Depart": "ABC",
        "Arrive": "DEF",
        "DateofIssue": "2018-08-15T20:08:03",
        "TicketNumber": "123 4567890987/1",
        "RLOC": "ABC123",
        "Name": "CASH/JOHNMR",
        "MFCI": "M",
        "Flight": "ZZ1234",
        "FlightDate": "2018-08-24T00:00:00",
        "VidStatus": "ETKT",
        "IATAStatus": "O",
        "FareCurr": "USD",
        "Fare": "739.4400",
        "Com": ".0000",
        "OthTax": "59.5600",
        "c19": "2",
        "c20": "0"
      },
      {
        ...
      }
    ]
  }
}
```

## Data Modeling

The `result.data` array is the the content of the response from the SSRP. If you specify `json` as the format, we submit the call and specify XML as the response type and then parse the XML into JSON.

# `GET /pivot/availability`

This endpoint will return the flight availability data for the specified querystring values.

- Path :: `/pivot/availability`
- Method :: `GET`
- Querystring
  - `date` - *(String, required)* The date that you want to look for an availability. Format can be any supported by Moment (https://momentjs.com/docs/#/parsing/string/)
  - `depart` - *(String, required)* IATA code for the point of departure.
  - `arrive` - *(String, required)* IATA code for the point of arrival.
  - `passengers` - *(Number, optional, default = 1)* The number of ticketed passengers.

## Example

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

## Data Modeling

The `result.availability` array is the standardized JSON version of the Videcom availability response (which is still accessible via the `result.raw`).

We've standardized a number of child data models, have converted specific values from `String` to the JavaScript type that is appropriate, and composed the `raw` response into a reasonable data structure.

We use strict data validation practices to verify that the data we read from Videcom meets expected standards. If this validation fails, it will throw an error in the response.

## Availability

This is an array of composed `Itinerary` elements

## Itinerary

- `line` - Number
- `airlineId` - String, the 2-character identifier for the airline
- `flightNumber` - String, the 4-characters idntifying the flight, can have leading `0`
- `flightId` - String, the joining of `${airlineId}${flightNumber}`
- `equipmentType` - String
- `stops` - Number
- `depart` - Object
  - `airport` - String, 3-letter IATA code
  - `date` - String, the date in `YYYY-MM-DD` format
  - `time` - String, the time in `HH:MM:SS` format
  - `timezone` - String, the timzone for this airport, e.g.: `America/New_York`
  - `moment` - String, the date/time in JSON format
- `arrive` - Object
  - `airport` - String, 3-letter IATA code
  - `date` - String, the date in `YYYY-MM-DD` format
  - `time` - String, the time in `HH:MM:SS` format
  - `timezone` - String, the timzone for this airport, e.g.: `America/New_York`
  - `moment` - String, the date/time in JSON format
- `duration` - Number, duration of flight in minutes
- `classes` - Array of Class

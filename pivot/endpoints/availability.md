# Availability

This endpoint will return the flight availability data for the specified querystring values.

- Availability
  - [Read](#availability-read)

## Availability Read

The querystring parameters for this endpoint are:
- `date` - *(String, required)* The date that you want to look for an availability. Format can be any supported by Moment (https://momentjs.com/docs/#/parsing/string/)
- `depart` - *(String, required)* IATA code for the point of departure.
- `arrive` - *(String, required)* IATA code for the point of arrival.
- `passengers` - *(Number, optional, default = 1)* The number of ticketed passengers.

### Example Request

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  'https://api-test.paxiq.com/pivot/1.0/availability?date=2018-07-24&depart=pit&arrive=aoo'
```

## Example Response

```json
{
  "requestId": "62f9e6c3-8f88-4cb7-bf6c-ae473dc0adfb",
  "echo": null,
  "result": {
    "command": "A24JULPITAOO[SALESCITY=PIT,VARS=TRUE,CLASSBANDS=TRUE,STARTCITY=PIT,SINGLESEG=S,FGNOAV=TRUE,QTYSEATS=1]",
    "raw": "<xml>...</xml>",
    "availability": [
      {
        "line": 1,
        "airlineId": "9X",
        "flightNumber": "0356",
        "flightId": "9X0356",
        "equipmentType": "CNC",
        "stops": 0,
        "depart": {
          "airport": "PIT",
          "date": "2018-07-24",
          "time": "13:00:00",
          "timezone": "America/New_York",
          "moment": "2018-07-24T17:00:00.000Z"
        },
        "arrive": {
          "airport": "AOO",
          "date": "2018-07-24",
          "time": "13:50:00",
          "timezone": "America/New_York",
          "moment": "2018-07-24T17:50:00.000Z"
        },
        "duration": 50,
        "classes": [
          {
            "classband": 1,
            "class": "Y",
            "available": 1,
            "currency": "USD",
            "CurInf": "2,0.01,0.01",
            "price": 45.77,
            "tax": 13.23,
            "fav": 1,
            "miles": 0,
            "awards": 0,
            "fid": 229,
            "finf": "0,0,1",
            "name": "Flexible",
            "cabin": "Y",
            "total": 59
          },
          {
            "classband": 2,
            "class": "",
            "available": 0,
            "currency": "USD",
            "CurInf": "2,0.01,0.01",
            "price": 0,
            "tax": 0,
            "fav": 0,
            "miles": 0,
            "awards": 0,
            "fid": 0,
            "finf": "",
            "name": "Flexible",
            "cabin": "Y",
            "total": 0
          },
          {
            "classband": 3,
            "class": "",
            "available": 0,
            "currency": "USD",
            "CurInf": "2,0.01,0.01",
            "price": 0,
            "tax": 0,
            "fav": 0,
            "miles": 0,
            "awards": 0,
            "fid": 0,
            "finf": "",
            "name": "Flexible",
            "cabin": "Y",
            "total": 0
          }
        ]
      },
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

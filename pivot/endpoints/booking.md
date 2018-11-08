# Booking

This endpoint will allow developers to create, read, update, and delete (cancel) bookings.

A booking is the fundamental data model that binds together a list of passengers, flight segments, tickets, and other data elements.

- Booking
  - [Data Model](./booking-model.md)
  - [Create](#booking-create)
  - [Read](#booking-read)
  - [Remarks](#remarks)
    - [Create](#remarks-create)
    - [Read](#remarks-read)
    - [Delete](#remarks-delete)
  - [Passengers](#passengers)
    - Create - **TODO**
    - [Read](#passengers-read)
    - Delete - **TODO**
  - [Tickets](#tickets)
    - Create - **TODO**
    - [Read](#tickets-read)
    - Delete - **TODO**
  - [Segments](#segments)
    - Create - **TODO**
    - [Read](#segments-read)
    - Delete - **TODO**

## Booking Create

To create a booking, you must POST a JSON payload to the booking endpoint with the following structure:

```json
{
  "passengers": [Passenger],
  "segments": [Segment],
  "payment": Payment
}
```

### Passengers

The `passengers` array must contain at least one Passenger object.

```json
{
  "name": {
    "first": "John",
    "last": "Doe",
    "title": "Mr"
  },
  "email": "jdoe@example.com",
  "phone": "5558679305",
  "dob": "1981-01-01",
  "type": "ADULT",
  "age": 37,
  "gender": "Male",
  "weight": 240,
  "ktNumber": "//K/AB12CD345///USA"
}
```

- `type` - *(String, optional)* Valid options are `ADULT`, `CHILD`, or `INFANT` (case-insensitive). If undefined, the `type` will be calculated based on the `dob` where 18 years or older is `ADULT` and 2 years or older is `CHILD` and under 2 is `INFANT`.
- `age` - *(String, Number, optional)* For adults & children, age should be in years. For an infant, age should be in months, e.g.: `13 MTHS`. If undefined, `age` is calculated based on the `dob`
- `name` - *(Object, required)*
  - `first` - *(String, required)*
  - `last` - *(String, required)*
  - `title` - *(String, optional)*
- `email` - *(String, optional)*
- `phone` - *(String, optional)*
- `dob` - *(String, required)* Format can be any supported by Moment (https://momentjs.com/docs/#/parsing/string/)
- `gender` - *(String, required)* Valid options are `male` or `female` (case-insensitive)
- `weight` - *(Number, required)* Passenger weight in pounds
- `ktNumber` - *(String, optional)* The passenger's Known Traveler Number, if applicable.

### Segments

The `segments` array in the payload must contain at least one Segment object.

```json
{
  "flightId": "AB0100",
  "class": "R",
  "date": "2018-06-05",
  "depart": "ABC",
  "arrive": "XYZ",
  "direction": "OUTBOUND"
}
```

- `flightId` *(String, required)* The two-character airline identifier and the four-character flight number
- `class` *(String, required)* The one-character class identifier for the segment
- `date` *(String, required)* Format can be any supported by Moment (https://momentjs.com/docs/#/parsing/string/)
- `depart` *(String, required)* The IATA code for the point of departure
- `arrive` *(String, required)* The IATA code for the point of arrival
- `direction` *(String, required)* Valid options are `outbound` or `inbound` (case-insensitive)

### Payment

The `payment` object defines how to handle the payment for the booking. You can specify `cash`, `invoice`, or `credit` as a method of payment.

**Cash**
```json
{
  "method": "cash"
}
```

**Invoice**
```json
{
  "method": "invoice"
}
```

**Credit**
```json
{
  "method": "credit",
  "total": 292.21,
  "currency": "USD",
  "ccNumber": "4242424242424242",
  "ccExpiration": "0120"
}
```

- `method` *(String, required)* Valid options are `cash` or `credit` (case-insensitive).
- `total` *(Number, required if `method === "credit"`)*
- `currency` *(String, required if `method === "credit"`)*
- `ccNumber` *(String, required if `method === "credit"`)*
- `ccExpiration` *(String, required if `method === "credit"`)*

### Sample Payload

```js
// payload.json
{
  "passengers": [{
    "name": {
      "first": "Jane",
      "last": "Doe",
      "title": "Mr"
    },
    "email": "jdoe@example.com",
    "dob": "1981-09-26",
    "gender": "Female",
    "weight": 240
  }],
  "segments": [{
    "flightId": "XX0101",
    "class": "R",
    "date": "2018-06-24",
    "depart": "LAX",
    "arrive": "PDX",
    "direction": "OUTBOUND"
  }],
  "payment": {
    "method": "cash"
  }
}
```

### Sample Request

```bash
curl \
  -X POST \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  -H "Content-Type: application/json" \
  -d @payload.json \
  https://api-test.paxiq.com/pivot/1.0/booking
```

### Sample Response

```js
{
  "requestId": "eab5c5e3-9d85-4b68-8a19-571cc21e9f58",
  "echo": null,
  "result": {
    "command": "-DOE/MRSJANE#^3-1FDOB 26SEP81^3-1FGNDR FEMALE^3-1FPWGT 240^9-1E*JDOE@EXAMPLE.COM^09X0356Y24JULLAXPDXNN1^FG^FS1^*R^MM^EZT*R^EZRE^*R~X",
    "raw": "<PNR RLOC=\"ABC123\" ... ></PNR>",
    "booking": {
      "rloc": "ABC123",
      ... omitted for brevity, see the booking-model.md
    }
  }
}
```

## Booking Read

Bookings are referenced by a "Record LOCator" (RLOC) which is a unique 6-character identifier composed of letters A-Z and numbers 0-9. The uniqueness of the RLOC is limited to the airline.

### Sample Request

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123
```

### Sample Response

See the [Booking data model](./booking-model.md)

## Remarks

Remarks are free-text notes on a booking. Some applications use the remarks as a way to store structured data with a booking, but this practice isn't recommended as notes can easily be deleted by anyone with write access to the booking.

Additionally, remarks are referenced by their line number, but this number can change if a lower-numbered remark is deleted. For example, if there are 3 remarks and remark #1 & #2 are deleted, the third remark becomes remark #1.

Remarks are always converted to UPPERCASE.

## Remarks Create

### Sample Payload

```js
// payload.json
{ "value": "this is remarkable" }
```

### Sample Request

```bash
curl \
  -X POST \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  -H "Content-Type: application/json" \
  -d @payload.json \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/remarks
```

### Sample Response

```js
{
  "requestId": "995d36b7-e7aa-40b9-9cf7-3fe2a7c792f4",
  "result": {
    "command": "*ABC123^5**this is remarkable^E~x",
    "raw": "OK Locator ABC123",
    "success": true
  }
}
```

## Remarks Read

Read all remarks

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/remarks
```

```js
{
  "requestId": "2b5e2cb3-d6b4-422f-99b5-ff3c930be481",
  "echo": null,
  "result": {
    "remarks": [
      {
        "line": 1,
        "rmkid": "",
        "value": "THIS IS REMARKABLE PDXAAAP09JUL"
      }
    ]
  }
}
```

Read remark at line 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/remarks/1
```

```js
{
  "requestId": "70a083c6-db3c-4f03-a847-c9a01046d6cf",
  "echo": null,
  "result": {
    "remark": {
      "line": 1,
      "rmkid": "",
      "value": "THIS IS REMARKABLE PDXAAAP09JUL"
    }
  }
}
```

## Remarks Delete

Delete remark at line 2

```bash
curl \
  -X DELETE \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/remarks/1
```

```js
{
  "requestId": "88b865d2-c1b2-4982-9533-9601d3273483",
  "echo": null,
  "result": {
    "command": "*ABC123^5x1^E",
    "raw": "OK Locator ABC123",
    "success": true
  }
}
```

## Passengers Create

**TODO**

## Passengers Read

Read all passengers on a booking

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/passengers
```

```js
{
  "requestId": "b6850652-0a6f-4dda-b3fd-f90132598ad3",
  "echo": null,
  "result": {
    "passengers": [
      {
        "passengerNumber": 1,
        "groupNumber": 1,
        "groupPaxNumber": 1,
        "name": {
          "title": "",
          "first": "MRSJUNE",
          "last": "DOE"
        },
        "type": "adult",
        "age": "",
        "awards": "0"
      }
    ]
  }
}
```

Read passenger 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/passengers/1
```

```js
{
  "requestId": "0002ecf5-6819-4bc0-be92-4d7e3dfb33c0",
  "echo": null,
  "result": {
    "passenger": {
      "passengerNumber": 1,
      "groupNumber": 1,
      "groupPaxNumber": 1,
      "name": {
        "title": "",
        "first": "MRSJUNE",
        "last": "DOE"
      },
      "type": "adult",
      "age": "",
      "awards": "0"
    }
  }
}
```

## Passengers Delete

**TODO**

## Tickets

Tickets bind passengers to segments and can be referenced by `passengerNumber` **and/or** `segmentNumber`.

## Tickets Create

**TODO**

## Tickets Read

Read all tickets

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/tickets
```

```js
{
  "requestId": "f0df60a0-78a9-4db7-9933-d1224726463b",
  "echo": null,
  "result": {
    "tickets": [
      {
        "passengerNumber": 1,
        "segmentNumber": 1,
        "ticketType": "ETKT",
        "ticketNumber": "504 2300127131",
        "coupon": 1,
        "airlineId": "9X",
        "flightNumber": "0356",
        "flightId": "9X0356",
        "class": "Y",
        "depart": {
          "airport": "PIT",
          "date": "24JUL2018",
          "timezone": "America/New_York",
          "moment": "2018-07-24T04:00:00.000Z"
        },
        "arrive": {
          "airport": "AOO",
          "timezone": "America/New_York"
        },
        "issueDate": "06JUL2018",
        "issueMoment": "2018-07-06T04:00:00.000Z",
        "status": "O",
        "name": {
          "title": "",
          "last": "DOE"
        },
        "ticketFor": ""
      }
    ]
  }
}
```

Read all tickets for passenger 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  'https://api-test.paxiq.com/pivot/1.0/booking/ABC123/tickets?passengerNumber=1'
```

```js
{
  "requestId": "67f80179-864b-4680-87f2-a8ae4b10d798",
  "echo": null,
  "result": {
    "tickets": [
      {
        "passengerNumber": 1,
        "segmentNumber": 1,
        "ticketType": "ETKT",
        "ticketNumber": "504 2300127131",
        "coupon": 1,
        "airlineId": "9X",
        "flightNumber": "0356",
        "flightId": "9X0356",
        "class": "Y",
        "depart": {
          "airport": "PIT",
          "date": "24JUL2018",
          "timezone": "America/New_York",
          "moment": "2018-07-24T04:00:00.000Z"
        },
        "arrive": {
          "airport": "AOO",
          "timezone": "America/New_York"
        },
        "issueDate": "06JUL2018",
        "issueMoment": "2018-07-06T04:00:00.000Z",
        "status": "O",
        "name": {
          "title": "",
          "last": "DOE"
        },
        "ticketFor": ""
      }
    ]
  }
}
```

Read all tickets for segment 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  'https://api-test.paxiq.com/pivot/1.0/booking/ABC123/tickets?segmentNumber=1'
```

```js
{
  "requestId": "3a451e47-bfbc-4fdf-aad6-2bf69c96de79",
  "echo": null,
  "result": {
    "tickets": [
      {
        "passengerNumber": 1,
        "segmentNumber": 1,
        "ticketType": "ETKT",
        "ticketNumber": "504 2300127131",
        "coupon": 1,
        "airlineId": "9X",
        "flightNumber": "0356",
        "flightId": "9X0356",
        "class": "Y",
        "depart": {
          "airport": "PIT",
          "date": "24JUL2018",
          "timezone": "America/New_York",
          "moment": "2018-07-24T04:00:00.000Z"
        },
        "arrive": {
          "airport": "AOO",
          "timezone": "America/New_York"
        },
        "issueDate": "06JUL2018",
        "issueMoment": "2018-07-06T04:00:00.000Z",
        "status": "O",
        "name": {
          "title": "",
          "last": "DOE"
        },
        "ticketFor": ""
      }
    ]
  }
}
```

Read ticket for passenger 1 on segment 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  'https://api-test.paxiq.com/pivot/1.0/booking/ABC123/tickets?passengerNumber=1&segmentNumber=1'
```

```js
{
  "requestId": "7c0f3916-4711-47ac-871c-bafbccb186f4",
  "echo": null,
  "result": {
    "ticket": {
      "passengerNumber": 1,
      "segmentNumber": 1,
      "ticketType": "ETKT",
      "ticketNumber": "504 2300127131",
      "coupon": 1,
      "airlineId": "9X",
      "flightNumber": "0356",
      "flightId": "9X0356",
      "class": "Y",
      "depart": {
        "airport": "PIT",
        "date": "24JUL2018",
        "timezone": "America/New_York",
        "moment": "2018-07-24T04:00:00.000Z"
      },
      "arrive": {
        "airport": "AOO",
        "timezone": "America/New_York"
      },
      "issueDate": "06JUL2018",
      "issueMoment": "2018-07-06T04:00:00.000Z",
      "status": "O",
      "name": {
        "title": "",
        "last": "DOE"
      },
      "ticketFor": ""
    }
  }
}
```

## Tickets Delete

**TODO**

## Segments

## Segments Create

**TODO**

## Segments Read

Read all segments

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/segments
```

```js
{
  "requestId": "a8c47f1c-92da-431d-a5ae-57dd598bdc07",
  "echo": null,
  "result": {
    "segments": [
      {
        "segmentNumber": 1,
        "airlineId": "9X",
        "flightNumber": "0356",
        "flightId": "9X0356",
        "class": "Y",
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
        "status": "HK",
        "passengers": 1,
        "stops": 0,
        "cabin": "Y"
      }
    ]
  }
}
```

Read segment 1

```bash
curl \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  https://api-test.paxiq.com/pivot/1.0/booking/ABC123/segments/1
```

```js
{
  "requestId": "ac3b3b20-7680-44f5-bd85-dc0fbf908c6c",
  "echo": null,
  "result": {
    "segment": {
      "segmentNumber": 1,
      "airlineId": "9X",
      "flightNumber": "0356",
      "flightId": "9X0356",
      "class": "Y",
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
      "status": "HK",
      "passengers": 1,
      "stops": 0,
      "cabin": "Y"
    }
  }
}
```

## Segments Delete

**TODO**

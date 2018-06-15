# `POST /pivot/booking`

This endpoint will create a booking.

- Path :: `/pivot/booking`
- Method :: `POST`
- Payload
  - `passengers` - *(Array of Passenger, required)*[Passenger](#passenger)
  - `segments` - *(Array of Segment, required)* [Segment](#segment)
  - `payment` - *(Object, required)* [Payment](#payment)

## Passenger

The `passengers` array in the payload must contain at least one Passenger object.

**Example**
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

## Segment

The `segments` array in the payload must contain at least one Segment object.

**Example**
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

## Payment

The `payment` object defines how to handle the payment for the booking.

**Example** - Cash
```json
{
  "method": "cash"
}
```

**Example** - Credit
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


## Example

**Request**
```js
// booking.json
{
  "passengers": [{
    "name": {
      "first": "John",
      "last": "Doe",
      "title": "Mr"
    },
    "email": "jdoe@example.com",
    "dob": "1981-01-01",
    "gender": "Male",
    "weight": 240
  }],
  "segments": [{
    "flightId": "AB0100",
    "class": "R",
    "date": "2018-06-05",
    "depart": "ABC",
    "arrive": "XYZ",
    "direction": "OUTBOUND"
  }],
  "payment": {
    "method": "credit",
    "total": 292.21,
    "currency": "USD",
    "ccNumber": "4242424242424242",
    "ccExpiration": "0120"
  }
}
```

```bash
curl \
  -X GET \
  -H "Videcom-Token: <<TOKEN>>" \
  -H "Videcom-Airline: <<AIRLINE>>" \
  -H "Pivot-Version: 1.0" \
  -H "Pivot-Echo: bar" \
  -d @booking.json \
  'https://api-test.paxiq.com/pivot/booking'
```

**Response**
```json
{
  "requestId": "27bcd679-8f14-42b0-a49c-0a108f99dfeb",
  "echo": "bar",
  "version": "1.0",
  "result": {
    "command": "-MRJOHN/DOE#^3-1FDOB 26SEP81^3-1FGNDR MALE^3-1FPWGT 240^9-1E*JDOE@EXAMPLE.COM^0TJ0105R24JUNSBHSJUNN1^FG^FS1^*R^MKUSD292.21/4242424242424242**0120^EZT*R^EZRE^R*~X",
    "raw": "<?xml version=\"1.0\" encoding=\"utf-8\"?><soap:Envelope xmlns:soap=\"http://www.w3.org/2003/05/soap-envelope\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:xsd=\"http://www.w3.org/2001/XMLSchema\"><soap:Body><RunVRSCommandResult xmlns=\"http://videcom.com/\">&lt;VrsServerResponse&gt;&lt;PaymentResult&gt;\n&lt;PaymentState&gt;PaymentError&lt;/PaymentState&gt;\n&lt;AuthCode/&gt;\n&lt;Description&gt;E00003 The 'AnetApi/xml/v1/schema/AnetApiSchema.xsd:cardCode' element is invalid - The value XX is invalid according to its datatype 'AnetApi/xml/v1/schema/AnetApiSchema.xsd:cardCode' - The Pattern constraint failed.&lt;/Description&gt;\n&lt;BankLogId/&gt;\n&lt;PaymentCompleted&gt;false&lt;/PaymentCompleted&gt;\n&lt;/PaymentResult&gt;&lt;/VrsServerResponse&gt;</RunVRSCommandResult></soap:Body></soap:Envelope>",
    "booking": {
      "paymentResult": {
        "state": "PaymentError",
        "authCode": "",
        "description": "E00003 The 'AnetApi/xml/v1/schema/AnetApiSchema.xsd:cardCode' element is invalid - The value XX is invalid according to its datatype 'AnetApi/xml/v1/schema/AnetApiSchema.xsd:cardCode' - The Pattern constraint failed.",
        "bankLogId": "",
        "paymentComplete": false
      }
    }
  }
}
```

## Data Modeling

The `result.booking` object shows the response back from Videcom. The example above shows the response when there is a problem with payment. If, however, payment is successful, the new [PNR](./pnr.md) is returned as `result.booking.pnr`.

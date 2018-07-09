# Booking Model

- `rloc` - String
- `locked` - Boolean
- `lockedReason` - String
- `secureFlight` - Boolean
- `sfpddob` - String
- `sfpdgndr` - String
- `showFares` - Boolean
- `editFlights` - Boolean
- `editProducts` - Boolean
- `passengers` - Array of [Passenger](#passenger)
- `segments` - Array of [Segment](#segment)
- `mps` - Array of [MP](#mp)
- `contacts` - Array of [Contact](#contact)
- `apfax` - Array of [AFX](#afx)
- `genfax` - Array of [GFX](#gfx)
- `fareQuote` - Object [FareQuote](#farequote)
- `payments` - Array of [Payment](#payment)
- `timeLimits` - Array of [TimeLimit](#timelimit)
- `tickets` - Array of [Ticket](#ticket)
- `remarks` - Array of [Remark](#remark)
- `rle` - Object [RLE](#rle)
- `basket` - Object [Basket](#basket)
- `zPay` - Object [ZPay](#zpay)
- `pnrFields` - Object [PNRFields](#pnrfields)
- `fqFields` - Array of [FQField](#fqfield)

## Passenger

- `passengerNumber` - Number, unique identifier for this passenger within this PNR
- `groupNumber` - Number
- `groupPaxNumber` - Number
- `name` - Object
  - `title` - String
  - `first` - String
  - `last` - String
- `type` - String, enumeration: `[ "adult", "child", "infant" ]`
- `age` - String, infant ages are recorded as "13 MTHS"
- `awards` - String

## Segment

- `segmentNumber` - Number, unique identifier for this itinerary/segment within this PNR
- `airlineId` - String, the 2-character identifier for the airline
- `flightNumber` - String, the 4-characters idntifying the flight, can have leading `0`
- `flightId` - String, the joining of `${airlineId}${flightNumber}`
- `class` - String, single letter indicating seating class
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
- `duration` - Number, the number of minutes that the flight is expected to take
- `status` - String
- `passengers` - Number, the number of passengers requiring a seat
- `stops` - Number, the number of stops
- `cabin` - String, single letter indicating the cabin class

## MP

- `line` - Number
- `mpid` - String
- `passengerNumber` - Number
- `segmentNumber` - Number
- `currency` - String
- `amount` - Number
- `mpsId` - Number
- `value` - String

## Contact

- `line` - Number
- `ctcid` - String
- `type` - String, enumeration: `[ "business", "agency", "home", "mobile", "other", "physical", "credit", "email" ]`
- `passengerNumber` - Number
- `value` - String

## AFX

- `line` - Number
- `afxid` - String
- `type` - String, enumeration: `[ "secure", "agency", "timelimit", "ticket" ]`
- `passengerNumber` - Number
- `segmentNumber` - Number
- `value` - String

## GFX

- `line` - Number
- `gfxid` - String
- `passengerNumber` - Number
- `segmentNumber` - Number
- `value` - String

## FareQuote

- `fqItins` - Array of [FQItin](#fqitin)
- `fareStores` - Array of [FareStore](#farestore)
- `fareTax` - Object
  - `paxTax` - Array of [PaxTax](#paxtax)

### FQItin

- `segmentNumber` - Number
- `currency` - String
- `curInf` - String
- `fqi` - String
- `fqb` - String
- `total` - Number
- `fare` - Number
- `tax1` - Number
- `tax2` - Number
- `tax3` - Number
- `miles` - Number

### FareStore

- `fsid` = String
- `passengerNumber` = Number
- `currency` = String
- `curInf` = String
- `total` = Number
- `segmentFS` = Array of [SegmentFS](#segmentfs)

#### SegmentFS

 - `segFSID` - String
 - `segmentNumber` - Number
 - `fare` - Number
 - `tax1` - Number
 - `tax2` - Number
 - `tax3` - Number
 - `miles` - Number
 - `disc` - Number

### PaxTax

- `segmentNumber` - Number
- `passengerNumber` - Number
- `code` - String
- `currency` - String
- `amount` - Number
- `curInf` - String

## Payment

- `line` - Number
- `fopId` - String
- `currency` - String
- `amount` - Number
- `reference` - String
- `pnrCurrency` - String
- `pnrAmount` - Number
- `pnrExchangeRate` - String
- `date` - String

## TimeLimit

- id - String
- city - String
- qNumber - String
- time - String
- date - String
- moment - String
- timezone - String
- agCity - String
- sineCode - String
- sineType - String
- resDate - String

## Ticket

- `passengerNumber` - Number
- `segmentNumber` - Number
- `ticketType` - String
- `ticketNumber` - String
- `coupon` - Number
- `airlineId` - String
- `flightNumber` - String
- `flightId` - String
- `class` - String
- `depart` - Object
  - airport: String
  - date: String
  - timezone: String,
  - moment: String, JSON date
- `arrive` - Object
  - airport: String
  - timezone: String
- `issueDate` - String
- `issueMoment` - String, JSON date
- `status` - String
- `name` - Object
  - title: String
  - first: String
  - last: String
- `ticketFor` - String

## Remark

- `line` - Number
- `rmkid` - String
- `value` - String

## RLE

- `rloc` - String
- `airlineId` - String
- `issueOfferCode` - String
- `city` - String
- `currency` - String
- `curInf` - String
- `sineCode` - String
- `rleDate` - String
- `timezone` - String
- `rleMoment` - String
- `issueAgentId` - String
- `issueAgentTicket` - String
- `gdsRef` - String

## Basket

- `outstanding` - Object
  - `currency` - String
  - `curInf` - String
  - `amount` - Number
  - `info` - String
- `outstandingAirMiles` - Object
  - `currency` - String
  - `curInf` - String
  - `amount` - Number
  - `info` - String
  - `airmiles` - Number

## ZPay

- `scheme` - String
- `rloc` - String
- `info` - String
- `amount` - Number
- `currency` - String
- `totalFare` - Number
- `totalTax` - Number
- `ttl` - String

## PNRFields

This property is poorly defined and there are too few examples to reverse-engineer it. Instead we simply pass-through the raw JSON data.

## FQField

- `line` - Number
- `fareId` - String
- `fareInf` - String

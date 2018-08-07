# Inventory

This endpoint allows updating inventory.

- Inventory
  - [Update](#availability-update)

## Inventory Update

### Request Data Model

The request data model for this endpoint is:
- `flightId` - *(String, required)* The two-character `airlineId` and the four-character `flightNumber`
- `depart` - *(String, required)* IATA code for the point of departure.
- `arrive` - *(String, required)* IATA code for the point of arrival.
- `start` - *(String, required)* The start date for flights to which to apply the update. Format can be any supported by [Moment](https://momentjs.com/docs/#/parsing/string/)
- `end` - *(String, required)* The end date for flights to which to apply the update. Format can be any supported by [Moment](https://momentjs.com/docs/#/parsing/string/)
- `days` - *(Object, required)* Setting the value of the `days.xx` child properties to `true` (or any Truthy value) will cause the update to apply to each weekday within the `start` and `end` dates. Omitting a day from `days` is the same as setting it to `false` (or any Falsey value).
  - `mo` - *(Truthy/Falsey, optional)* Monday
  - `tu` - *(Truthy/Falsey, optional)* Tuesday
  - `we` - *(Truthy/Falsey, optional)* Wednesday
  - `th` - *(Truthy/Falsey, optional)* Thursday
  - `fr` - *(Truthy/Falsey, optional)* Friday
  - `sa` - *(Truthy/Falsey, optional)* Saturday
  - `su` - *(Truthy/Falsey, optional)* Sunday
- `classes` - *(Array, required)* An array of seating classes & their associated inventory
  - `class` - *(String, required)* A single letter representing the seating class
  - `inventory` - *(Number, required)* The number of seats assigned to the seating class

#### Truthy & Falsey

In JavaScript & Node.js, [Truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) and [Falsey](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) are terms that refer to a set of values that indicate `true` or `false`.

```js
const truthy = [ true, 1, -1, `foo`, {}, [], new Date() ];

const falsey = [ false, null, undefined, 0, NaN, `` ];
```

### Response Data Model

The response data model uses the standard Pivot response model:

- `requestId` - The UUID identifying this request.
- `echo` - The optional `Pivot-Echo` header provided by the client.
- `result` - If succesful
  - `success` - Boolean `true`
- `error` - If there is an error
  - `statusCode` - Number
  - `error` - The error type
  - `message` - The error message

## Example

### Request

```js
// payload.json
{
  "flightId": "XX1234",
  "depart": "PDX",
  "arrive": "LAX",
  "start": "2019-01-01",
  "end": "2019-01-01",
  "days": {
    "mo": true,
    "tu": true,
    "we": true,
    "th": true,
    "fr": true,
    "sa": true,
    "su": true
  },
  "classes": [
    { "class": "Y", "inventory": 10 },
    { "class": "S", "inventory": 5 },
    { "class": "K", "inventory": 2 }
  ]
}
```

```bash
curl \
  -X POST \
  -H "Videcom-Token: {TOKEN}" \
  -H "Videcom-Airline: {AIRLINE}" \
  -d @payload.json \
  'https://api-test.paxiq.com/pivot/1.0/inventory'
```

## Response

```json
{
  "requestId": "a959ea46-25f4-4ed8-94f7-23d37eb88534",
  "echo": null,
  "result": {
    "success": true
  }
}
```

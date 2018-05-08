# Booking a Flight

## Sample Concatenated Command

```
-Doe/John#^9-1E*johndoe@example.com^9-1M*555-867-5309^3-1FDOB 17NOV44^3-1FGNDR Male^-Doe/Jane#^3-2FDOB 21DEC46^3-2FGNDR Female^09X0157M27JulMGWPITNN2^FG^FS1^3-1fpwgt 172^4-1FDOCO//K/987654320///USA^3-2fpwgt 135^4-2FDOCO//K/987654321///USA^*R^MKUSD88.00/123456xxxxxx7890**0120^EZT*R^EZRE^*R~x
```

## Command breakdown

### Creating a passenger

```
-Doe/John#
```

This is passenger #1 because it was the first passenger identified. Template is:

```
-${lastName}/${firstName}#
```

### Adding passenger contact information

```
9-1E*johndoe@example.com
```
This is passenger #1's email contact information. The template for this command is:

```
9-${passengerNumber}E*${passengerEmail}
```

```
9-1M*555-867-5309
```

This is passenger #1's mobile number. The template is:

```
9-${passengerNumber}M*${passengerPhoneNumber}
```

Valid options for type of contact information include:

- `B` - Business contact number
- `T` - Travel agency information
- `H` - Home phone number
- `M` - Mobile phone number
- `P` - Other phone number
- `A` - Home address
- `C` - Credit card address
- `E` - Email address

### General passenger information

```
3-1FDOB 17NOV44
```
This is passenger #1's date of birth. Template is:

```
3-${passengerNumber}FDOB ${passengerDoB}
```

Valid options include:

- `FGNDR` - Passenger gender: `Male`, `Female`
- `FPWGT` - Passenger weight (in pounds): `172`
- `FDOB` - Passenger date of birth: `24MAY81`

### Flight information

```
09X0157M27JulMGWPITNN2
```

Breakdown:

- `0` - Standard prefix that tells Videcom that you want to sell seats on the flight that you are about to specify.
- `9X0157` - Flight identifier, `9X` = the airline prefix, `0157` the flight number.
- `M` - Seating class identifier
- `27Jul` - The date of departure (`DDMMM`).
- `MGWPIT` - The IATA code for departure (`MGW`) and arrival (`PIT`).
- `NN` - Actually sell, don't just quote the price (`QQ`)
- `2` - The number of ticketed passengers

### Airport information

```
4-1FDOCO//K/987654321///USA
```

This example is the "US Known Traveler Number". Template is:

```
4-${passengerNumber}FDOCO//${docType}/${docDetails}
```

Valid `${docType}` and `${docDetails}` include:

- *DHS Redress Number*
  - Document type = `R`
  - Document details = `12345///USA` (`/I` for infant)
- *US Known Traveler Number*
  - Document type = `K`
  - Document details = `12345///USA` (`/I` for infant)
- *Visa information*
  - Document type = `V`
  - Document details = `9891555/LONDON/14MAR06/USA`
    - `9891555` = Visa number
    - `LONDON` = Place of issue
    - `14MAR06` = Date of issue
    - `USA` = Country where visa applies

### Fare Commands

```
FG
```

This is the *Fare Quote* command. It essentially creates a list of fares from which you can select one to sell by issuing an `FS${number}` command.

```
FS1
```

This is the *Fare Store* command. Issuing this command immediately after the `FG` command will sell from the *Fare Quote* list whichever fare is specified by the `${number}`.

### Diaplay Commands

```
*R
```

This command will "save" the information you've entered so far and and return the PNR context making it possible to assign tickets to the PNR.

### Payment Information

```
MKUSD88.00/123456xxxxxx7890**0120
```

This command tells Videcom how to process the payment for the booking. Template is:

- `MK` - Prefix identifying payment information.
- `USD` - Currency identifier.
- `88.00` - The cost of the tickets in whatever currency is specified.
- `/` - separator
- `123456xxxxxx7890` - The credit card number,
- `**` - separator
- `0120` - Credit card expiration date (`MMYY`).

### Ticketing

```
EZT*R
```

This command tells Vidcom to issue the E-Ticket for all pax on all segments, finalize the transaction and return the PNR context (`*R`).

### Email confirmation

```
EZRE
```

This command tells Videcom to send confirmation emails to the address specified in the CTCE field (`9-${passengerNumber}E*${passengerEmail}`).

### Return format

```
~x
```

Appending this to the command tells Videcom that you expect a response in XML format.

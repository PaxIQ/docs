# PNR

> Passenger Name Record

The PNR is a model that represents the association of passengers, itineraries, tickets, and fares. The command to request a PNR is:

## Plain Text

```
*X4AAH9
```

This command will return all the relevant information for that record in plain-text format. This can be useful for humans to interpret, but it is difficult for a computer to parse.

```
1.1DOE/JOHN

1  X4 0506 Y  FR 25MAY18SGY JNU HK1   1130 1215   CAB Y

2  AS 0076 L  FR 25MAY18JNU SEA PK1   1316 1635   CAB L

3  AS 0712 L  FR 25MAY18SEA STL PK1   1805 2357   CAB L

01 CTCP   MIA 800 321 6666  A CARNIVAL TOURS

02 CTCP   MIA 877 885 4856 CARNIVAL TOURS AFTER HOURS

03 CTCA

01 AFXS-1 DOCS ////14MAR62/M//DOE/JOHN JOE[-HK]

02 AFXS-1 DOCA R/US/CARNIVAL LEGEND/SKAGWAY/US/99840[-HK]

03 AFXS   ADTK TO X4 BY 08/05/2018 07:25:00 SGY OTHERWISE WILL BE XLD[X4]

04 AFX1-1 TKNE 0420123456789/1[-HK]

FARE QUOTE -MANUAL- *USD.00 Y/.00+.00/.00+.00/.00+.00 Manual Fare Entry

01 FQ01   *USD0/.00+.00 [ ]

FQC 1 USD       0.00 /0.00+0.00/0.00+0.00/0.00+0.00

Total USD        .00

TICKET ISSUED 042 0123456789/01 ETKT 1 DOE/JOHN

01 RMKS   CKIN YY HK1 SEAMAN PSGR CARNIVAL CRUISE CREW

02 RMKS   OTHS YY MARINE FARE CHECK SEAMAN DOCUMENTS AT CHECK IN

03 RMKS   YY CARNIVAL CRUISES BOOKING  PLEASE DO NOT CANCEL

04 RMKS   YY SEAMAN TRAVELER PLS ALLOT REQD BAGGAGE ALLOWANCE

05 RMKS   YY CARNIVAL TOURS 3055990210 MIAMI IATA ARC 10585363

06 RMKS   TYPE-A GDS BOOKING

07 RMKS   BOOKED BY IATA NO: 10585555 PCC: MIA GDSID: 1S

08 RMKS   TICKETED BY IATA NO: 10585555 PCC:  GDSID: 1S

09 RMKS   TFID KALININ AVIATION LLC-2104555 JNUAAAP08MAY

JNUX4 X4AAH9/GDS001/JNU/X4///USD/GD 30/04/2018 23:03:10 [1S/FLCEFT]

X4AAH9

```

Appending `~x` will cause the response to be formatted in XML-ish:

```
*X4AAH9~x
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
  <soap:Body>
    <RunVRSCommandResult xmlns="http://videcom.com/">&lt;PNR RLOC="X4AAH9" PNRLocked="False" PNRLockedReason="" SecureFlight="False" sfpddob="N" sfpdgndr="N" showFares="True" editFlights="False" editProducts="True"&gt;
  &lt;Names&gt;
    &lt;PAX GrpNo="1" GrpPaxNo="1" PaxNo="1" Title="" FirstName="JOHN" Surname="DOE" PaxType="AD" Age="" awards="0"/&gt;
  &lt;/Names&gt;
  &lt;Itinerary&gt;
    &lt;Itin Line="1" AirID="X4" FltNo="0506" Class="Y" DepDate="2018-05-25" Depart="SGY" Arrive="JNU" Status="HK" PaxQty="1" DepTime="11:30:00" ArrTime="12:15:00" ArrOfst=" " ddaygmt="2018-05-25" dtimgmt="19:30:00" adaygmt="2018-05-25" atimgmt="20:15:00" Stops="0" Cabin="Y" GDSID="" GDSRLoc="" DMap="" AMap="" SecID="" SecRLoc="" MSL="" Hosted="1" nostop="" cnx="" ClassBand="" onlineCheckin="False" OperatedBy="" OAWebsite="" SelectSeat="True" MMBSelectSeat="True"/&gt;
    &lt;Itin Line="2" AirID="AS" FltNo="0076" Class="L" DepDate="2018-05-25" Depart="JNU" Arrive="SEA" Status="PK" PaxQty="1" DepTime="13:16:00" ArrTime="16:35:00" ArrOfst=" " ddaygmt="2018-05-25" dtimgmt="21:16:00" adaygmt="2018-05-25" atimgmt="23:35:00" Stops="0" Cabin="L" GDSID="" GDSRLoc="" DMap="" AMap="" SecID="" SecRLoc="" MSL="" Hosted="0" nostop="" cnx="" ClassBand="" onlineCheckin="False" OperatedBy="" OAWebsite="" SelectSeat="True" MMBSelectSeat="True"/&gt;
    &lt;Itin Line="3" AirID="AS" FltNo="0712" Class="L" DepDate="2018-05-25" Depart="SEA" Arrive="STL" Status="PK" PaxQty="1" DepTime="18:05:00" ArrTime="23:57:00" ArrOfst=" " ddaygmt="2018-05-26" dtimgmt="01:05:00" adaygmt="2018-05-26" atimgmt="04:57:00" Stops="0" Cabin="L" GDSID="" GDSRLoc="" DMap="" AMap="" SecID="" SecRLoc="" MSL="" Hosted="0" nostop="" cnx="" ClassBand="" onlineCheckin="False" OperatedBy="" OAWebsite="" SelectSeat="True" MMBSelectSeat="True"/&gt;
  &lt;/Itinerary&gt;
  &lt;MPS/&gt;
  &lt;Contacts&gt;
    &lt;CTC Line="1" CTCID="P" Pax=""&gt;MIA 800 321 6666  A CARNIVAL TOURS&lt;/CTC&gt;
    &lt;CTC Line="2" CTCID="P" Pax=""&gt;MIA 877 885 4856 CARNIVAL TOURS AFTER HOURS&lt;/CTC&gt;
    &lt;CTC Line="3" CTCID="A" Pax=""&gt;&lt;/CTC&gt;
  &lt;/Contacts&gt;
  &lt;APFAX&gt;
    &lt;AFX Line="1" AFXID="DOCS" Pax="1" Seg="S"&gt;////14MAR62/M//DOE/JOHN JOE&lt;/AFX&gt;
    &lt;AFX Line="2" AFXID="DOCA" Pax="1" Seg="S"&gt;R/US/CARNIVAL LEGEND/SKAGWAY/US/99840&lt;/AFX&gt;
    &lt;AFX Line="3" AFXID="ADTK" Pax="" Seg="S"&gt;TO X4 BY 08/05/2018 07:25:00 SGY OTHERWISE WILL BE XLD&lt;/AFX&gt;
    &lt;AFX Line="4" AFXID="TKNE" Pax="1" Seg="1"&gt;0427143804378/1&lt;/AFX&gt;
  &lt;/APFAX&gt;
  &lt;GenFax/&gt;
  &lt;FareQuote&gt;
    &lt;FQItin Seg="1" Cur="USD" CurInf="2,.01,.01" FQI="" FQB="" Total="0" Fare=".00" Tax1=".00" Tax2="0" Tax3="0" miles="0"/&gt;
    &lt;FareStore FSID="FQC" Pax="1" Cur="USD" CurInf="2,.01,.01" Total="0.00"&gt;
      &lt;SegmentFS SegFSID="" Seg="1" Fare="0.00" Tax1="0.00" Tax2="0" Tax3="0" miles="0" disc="0"/&gt;
      &lt;SegmentFS SegFSID="" Seg="2" Fare="0.00" Tax1="0.00" Tax2="0" Tax3="0" miles="0" disc="0"/&gt;
      &lt;SegmentFS SegFSID="" Seg="3" Fare="0.00" Tax1="0.00" Tax2="0" Tax3="0" miles="0" disc="0"/&gt;
    &lt;/FareStore&gt;
    &lt;FareStore FSID="Total" Pax="" Cur="USD" CurInf="2,.01,.01" Total=".00"/&gt;
    &lt;FareTax/&gt;
  &lt;/FareQuote&gt;
  &lt;Payments/&gt;
  &lt;TimeLimits/&gt;
  &lt;Tickets&gt;
    &lt;TKT Pax="1" TKTID="ETKT" TktNo="042 7143804378" Coupon="01" TktFltDate="25MAY2018" TktFltNo="X40506" TktDepart="SGY" TktArrive="JNU" TktBClass="Y" IssueDate="08MAY2018" Status="O" SegNo="01" Title="" Firstname="JOHN" Surname="DOE" TktFor=""/&gt;
  &lt;/Tickets&gt;
  &lt;Remarks&gt;
    &lt;RMK Line="1" RMKID=""&gt;CKIN YY HK1 SEAMAN PSGR CARNIVAL CRUISE CREW&lt;/RMK&gt;
    &lt;RMK Line="2" RMKID=""&gt;OTHS YY MARINE FARE CHECK SEAMAN DOCUMENTS AT CHECK IN&lt;/RMK&gt;
    &lt;RMK Line="3" RMKID=""&gt;YY CARNIVAL CRUISES BOOKING  PLEASE DO NOT CANCEL&lt;/RMK&gt;
    &lt;RMK Line="4" RMKID=""&gt;YY SEAMAN TRAVELER PLS ALLOT REQD BAGGAGE ALLOWANCE&lt;/RMK&gt;
    &lt;RMK Line="5" RMKID=""&gt;YY CARNIVAL TOURS 3055992600 MIAMI IATA ARC 10585363&lt;/RMK&gt;
    &lt;RMK Line="6" RMKID=""&gt;TYPE-A GDS BOOKING&lt;/RMK&gt;
    &lt;RMK Line="7" RMKID=""&gt;BOOKED BY IATA NO: 10585363 PCC: MIA GDSID: 1S&lt;/RMK&gt;
    &lt;RMK Line="8" RMKID=""&gt;TICKETED BY IATA NO: 10585363 PCC:  GDSID: 1S&lt;/RMK&gt;
    &lt;RMK Line="9" RMKID=""&gt;TFID KALININ AVIATION LLC-2104108 JNUAAAP08MAY&lt;/RMK&gt;
  &lt;/Remarks&gt;
  &lt;TourOp/&gt;
  &lt;RLE RLOC="X4AAH9" AirID="X4" IssOffCode="GDS001" City="JNU" Cur="USD" CurInf="2,.01,.01" SineCode="GD" RLEDate="2018-04-30T23:03:10" issagtidpnr="1SGDS003" issagtidtkt="" GDSRef="1S/FLCEFT"/&gt;
  &lt;Basket&gt;
    &lt;Outstanding cur="USD" CurInf="2,.01,.01" amount="0" info=""/&gt;
    &lt;Outstandingairmiles cur="USD" CurInf="2,.01,.01" amount="0.00" info="Outstanding Currency and Airmiles" airmiles="0"/&gt;
  &lt;/Basket&gt;
  &lt;zpay/&gt;
  &lt;pnrfields originissoffcode="GDS001"/&gt;
  &lt;fqfields&gt;
    &lt;fqfield line="1" fareid="" finf="0,0,0"/&gt;
  &lt;/fqfields&gt;
&lt;/PNR&gt;</RunVRSCommandResult>
  </soap:Body>
</soap:Envelope>
```

Clearly, this is "XML-ish" as the `>` and `<` characters have been HTML encoded as `&gt;` & `&lt;`, respectively. In order to parse this as XML, you first need to clean it up. In JavaScript, it looks like this:

```javascript
// pnr is the string of data returned in the <RunVRSCommandResult> element

const pnrXml = pnr
  .replace(/&gt;/g, `>`)
  .replace(/&lt;/g, `<``);
```

There are some JavaScript/NodeJS libraries that I have found useful in working with this data:

- [`pixl-xml`](https://www.npmjs.com/package/pixl-xml) - Good for parsing the XML into JSON

```javascript
const pixl = require('pixl-xml');
const pnrJson = pixl.parse(pnrXml, { forceArrays: true });
```

- [`has-deep`](https://www.npmjs.com/package/has-deep) - Makes navigating the JSON a bit less painful

```javascript
const has = require('has-deep');
const airlineId = has(pnrJson[0], 'RLE[0].AirID');

// instead of

const airlineId = (pnrJson && pnrJson[0] && pnrJson[0].RLE && pnrJson[0].RLE[0]) ? pnrJson[0].RLE[0].AirID : undefined;
```

## Record Locator (`rloc`)

The RLOC is the unique 6-character string used to identify a PNR. In the example we use here, the RLOC is `X4AAH9`. The RegExp pattern for matching the RLOC is `/[A-Z0-9]{6}/.`

## Passengers

A PNR has one or many passengers. Individual passengers are listed as `<PAX>` elements in the `<Names>` element.

The `PAX.PaxNo` property is the same as the line number in the plain-text PNR and is how the other data in the PNR is associated with a passenger.

The `PaxNo` number *can* change if passengers with a lower number are removed.

## Itineraries

A PNR *should* have at least one `Itinerary.Itin` element. If it does not, then the PNR has been canceled and the contents of the `<Itinerary>` element *should* be `NO Itinerary`.

The `Itin.Line` property is the unique identifier by which the segment/itinerary is referenced elsewhere in the PNR.

The `Line` number *can* change if segments w/ a lower number are removed.

## Contacts

The `Contacts.CTC` elements contain the contact information for the PNR. Sometimes, as in the example PNR, this information reflects the travel agency that booked the trip. If there is a `CTC.Pax` number provided, it should reflect the `PAX.PaxNo`.

## APFAX

- TODO

## GenFax

- TODO

## FareQuote

- TODO

## Payments

- TODO

## Tickets

The `Tickets.TKT` element references both a passenger & an itinerary.

```
TKT.Pax === PAX.PaxNo
TKT.SegNo === Itin.Line
```

## Remarks

The `Remarks.RMK` elements are free-text fields with notes for the PNR. The `RMK.Line` number is how the remark is referenced and it *can* change if other remarks with a lower `Line` number are removed.

## TourOp

- TODO

## RLE

- TODO

# Headers

Pivot accepts several HTTP headers:

- `Videcom-Token` _(required)_ This is the token assigned by Videcom.
- `Videcom-Airline` _(required)_ This is the airline name as defined by Videcom.
- `Videcom-Timeout` _(optional)_ When Pivot communicates with Videcom, a default timeout is set at 30 seconds (30000ms). With this optional header, you can specify an alternative timeout. *The value must be in milliseconds.*

[Main](./readme.md)

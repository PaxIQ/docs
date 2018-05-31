# Authentication

Currently, Pivot uses a pass-through model for authentication with Videcom. Each request sent to Pivot must contain two headers:
- `Videcom-Token`
- `Videcom-Airline`

These headers are mapped into the request that is sent to Videcom along with the constructed command.

**To use the Pivot API, you must authorize the `35.197.78.251` with Videcom!**

[Main](./readme.md)

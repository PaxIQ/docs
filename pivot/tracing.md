# Call Tracing

Each request to Pivot can contain a `Pivot-Echo` header that *should* be a unique string that client applications can use to help troubleshoot interactions with Pivot. The value in the `Pivot-Echo` header is returned in the response as `echo`.

Each response will also include a `requestId` property which is necessary for diagnosing & troubleshooting individual calls. The `requestId` value is unique and generated automatically with each call.

See [Logs](./endpoints/logs.md) for more detail on pulling logs.

[Main](./readme.md)

# Versioning

Endpoint versioning is tracked in the URL path. We use strict validation of responses from Videcom to verify the integrity of the data and our ability to populate our data models. As a result, even small changes to the Videcom data structure can result in errors in Pivot. Because each endpoint is versioned independently, however, we're able to develop and deploy fixes quickly and without interfering with other endpoints.

Adding new properties to our models will result in revving the **minor** version number (1.1 instead of 1.0).

Removing or altering model structure will result in revving the **major** version number (2.0 instead of 1.1).

[Main](./readme.md)

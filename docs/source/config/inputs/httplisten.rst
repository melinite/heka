
HttpListenInput
===============

.. versionadded:: 0.5

HttpListenInput plugins start a webserver listening on the specified address
and port. If no decoder is specified data in the request body will be populated
as the message payload. Messages will be populated as follows:

- Uuid: Type 4 (random) UUID generated by Heka.
- Timestamp: Time HTTP request is handled.
- Type: `heka.httpdata.request`
- Hostname: The remote network address of requester.
- Payload: Entire contents of the HTTP response body.
- Severity: 6
- Logger: HttpListenInput
- Fields["UserAgent"] (string): Request User-Agent header (e.g. "GitHub Hookshot dd0772a").
- Fields["ContentType"] (string): Request Content-Type header (e.g. "application/x-www-form-urlencoded").
- Fields["Protocol"] (string): HTTP protocol used for the request (e.g.
                               "HTTP/1.0")

.. versionadded:: 0.6

All query parameters are added as fields. For example, a request to
"127.0.0.1:8325?user=bob" will create a field "user" with the value
"bob".

Config:

- address (string):
    An IP address:port on which this plugin will expose a HTTP server.
    Defaults to "127.0.0.1:8325".
- decoder (string):
    The name of the decoder used to further transform the request body text
    into a structured hekad message. No default decoder is specified.

.. versionadded:: 0.7

- headers (subsection, optional):
    It is possible to inject arbitrary HTTP headers into each outgoing response
    by adding a TOML subsection entitled "headers" to you HttpOutput config
    section. All entries in the subsection must be a list of string values.

.. versionadded:: 0.9

- unescape_body (bool):
    Specifies whether or not the received request body will be URL unescaped
    before being written to the message payload. Defaults to true.

- request_headers ([]string):
    Add additional request headers as message fields. Defaults to empty list.

Example:

.. code-block:: ini

    [HttpListenInput]
    address = "0.0.0.0:8325"

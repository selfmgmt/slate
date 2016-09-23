# Error Messages

> If you attempt to initiate a request that does not contain a supported verb for the endpoint, you will get a message as follows:

```json
{
  "errors": [
   {
      "code": 405,
      "reason": "method_not_allowed",
      "detail": "allowed_methods: GET"
    }
  ]
}
```

> All other errors should be self-explanatory as seen from the example below where an invalid token was sent to the endpoint:

```json
{
  "errors": [
    {
      "code": 400,
      "reason": "bad_request",
      "detail": "invalid_token_error"
    }
  ]
}
```

All messages are returned in a `JSON` array with with the keyword `errors` being the top level node. Contents include an error code, a reason and a detail. The http status code is also returned in the http response.

Error Code | Meaning
---------- | -------
400 | Bad Request
401 | Unauthorized
403 | Forbidden
404 | Not Found
405 | Method Not Allowed
500 | Internal Server Error -- We had a problem with our server. Please try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

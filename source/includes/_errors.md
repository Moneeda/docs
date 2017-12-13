# Errors

The Moneeda API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks.
401 | Unauthorized -- Your API key is wrong or you didn't provide it.
403 | Forbidden -- Unsupported endpoint.
404 | Not Found -- The specified endpoint is not found or the exchange doesn't support this endpoint.
405 | Method Not Allowed -- You tried to access a kitten with an invalid method.
429 | Too Many Requests -- You're requesting too many times! Slow down!
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.

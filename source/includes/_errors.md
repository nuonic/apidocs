# Errors

Nuonic APIs use the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is not valid
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- You are not authorized to access this resource
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarially offline for maintanance. Please try again later.

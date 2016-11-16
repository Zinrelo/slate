# Errors

Our API returns standard HTTP success or error status codes. For errors, we will also include extra information about what went wrong encoded in the response as JSON. The various HTTP status codes we might return are listed below.

## HTTP Status codes


Code | Title | Description
-----| ------- | --------
200 | OK | The request was successful.
400 | Bad Request | Bad request.
401 | Unauthorized | Your API key is invalid.
404 | Not Found | The resource does not exist.
50X | Internal Server Error | An error occured with our API.


## Error types

> Example error response

```
{
    "error": {
      "success": false,
      "reason": "missing parameters"
    }
}
```

All errors are returned in the form of JSON containing the error reason.

Reason | Description
------ | -----------
authentication missing | When the authentication fields are missing from the header of the API request.
authentication failure | When the authentication fails due to a wrong combination of partner-id and api-key.
missing parameters | When the required parameters in a API request are missing.
unexpected date format | When the date format in the parameters for the API request is not in the required format.
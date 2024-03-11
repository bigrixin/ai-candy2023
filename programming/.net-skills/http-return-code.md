---
description: Return HTTP Status Code from ASP.NET Core Methods
---

# HTTP Return code

1xx – Informational \
2xx – Successful \
3xx – Redirection \
4xx – Client Error \
5xx – Server Error



// Http status code **200** \
Standard response for successful HTTP requests\
**return Ok();**

// Http status code **201**\
The request has been fulfilled, resulting in the creation of a new resource. return Created();\
**return CreatedAtAction();**\
**return CreatedAtRoute();**

// Http status code **202**\
**return Accepted();** \
**return AcceptedAtAction();**\
**return AcceptedAtRoute();**

// Http status code **301**\
Redirecting permanently then the status code will be 301. \
**return LocalRedirectPermanent("\~/api/employees");**

// Http status code **302**\
Redirecting temporary then the status code will be 302. \
**return LocalRedirect("\~/api/employees");**

// Http status code **204**\
The server successfully processed the request, and is not returning any content. return **NoContent();**

// Http status code **400**\
The server cannot or will not process the request due to an apparent client error. return **BadRequest();**

// Http status code **401** \
Similar to 403 Forbidden, but specifically for use when authentication is required and has failed or has not yet been provided.\
**return Unauthorized();**

// Http status code **403**\
The request contained valid data and was understood by the server, but the server is refusing action. \
**return Forbid();**

// Http status code **404** \
The requested resource could not be found but may be available in the future. Subsequent requests by the client are permissible.\
**return NotFound();**

[`More return code`](https://en.wikipedia.org/wiki/List\_of\_HTTP\_status\_codes)



#### [`more return code`](https://learn.microsoft.com/en-us/dotnet/api/system.net.httpstatuscode?view=net-7.0)`for .NET 7`

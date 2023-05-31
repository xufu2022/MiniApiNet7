# Handling Exceptions and Logging

Handling exceptions is a common task for any ASP.NET Core application
- It’s not that different for minimal APIs

## Developer Exception Page Middleware && Exception Handler Middleware

- Exposes stack traces for unhandled exceptions
    - Useful during development
    - Should NOT be used outside of a development environment

- Enabled by default when 
    - Running in the development environment
    - AND the app is set up with a call into WebApplication.CreateBuilder

- Produces an error payload without exposing stack traces
    - Useful when NOT working in a development environment
    - Handles & logs exceptions

- NOT enabled by default 
    - Enable with a call into app.UseExceptionHandler

## Improving Error Responses with Problem Details

```cs
// enable
builder.Services.AddProblemDetails();
```

Provided via **IProblemDetailsService**
Default implementation is included with ASP.NET Core

## Effects of Enabling the Default Problem Details Service

- The developer exception page middleware will automatically generate a problem details response 
- The exception handler middleware will automatically generate a problem details response
- The status code page middleware can be configured to generate problem details responses for empty bodies (e.g.: 400, 404, …)

## Summary

Developer exception page middleware
- Returns stack traces, for use during development
Exception handler middleware
- Handles and logs exceptions but doesn’t return stack traces, for use when not in development

If possible, return a problem details payload in the error response body
- Standardized response for API errors
Log like you would log in “regular” ASP.NET Core
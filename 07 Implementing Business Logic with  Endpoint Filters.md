# Implementing Business Logic with Endpoint Filters

> ASP.NET Core MVC filter pipeline

A pipeline of filters that runs after MVC has selected the action to execute. These filters allow you to run code before or after specific stages in the request processing pipeline. 

                        
                                Model Binding   Action 
Authorization filters => Resource filters => Action Filters => Exception Filters=> Result Filter => Result Executes

There is **no comparable filter pipeline** for minimal APIs
- As an alternative, endpoint filters are supported

## Endpoint Filters Enable

- Running code before and after the endpoint handler
- Inspecting and modifying parameters provided during endpoint handler invocation
- Intercepting the response behavior of an endpoint handler

## Common Scenarios for Filters

- Logging request/response information
- Transforming the request
- Transforming the response
- (Dis)allowing a request
- Validating the incoming request

A request travels down the list of endpoint filters and then back up again in reverse order
- The order in which they are added is important!
- Filters can short-circuit the pipeline

## Summary

Create an endpoint filter
- IEndpointFilter
Apply to an endpoint
- AddEndpointFilter

The order in which endpoint filters are added matters 
Endpoint filters can short-circuit the pipeline
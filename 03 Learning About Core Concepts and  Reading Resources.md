# Learning About Core Concepts and Reading Resources

## Inversion of Control (IoC)

The IoC pattern delegates the function of selecting a concrete implementation type for a class's dependencies to an external component

## Dependency Injection in Minimal APIs

The handler’s dependencies are provided by an external component
- Don’t new them up

The DI pattern uses an object - the container - to initialize other objects, manage their lifetime and provide the required dependencies to objects

```cs
builder.Services.AddDbContext<DishesDbContext>(o => o.UseSqlite(
    builder.Configuration["ConnectionStrings:DishesDBConnectionString"]));

app.MapGet("/dishes", (DishesDbContext dishesDbContext) =>
{
// … implementation
});
```

## Routing

The process of matching an HTTP method & URI to a specific route handler

Learning About Routing

### app.MapAction methods 
- Where Action = the HTTP method
- IEndpointRouteBuilder

| **HTTP method**    | **Endpoint route builder method** | **Request payload** | **Sample URI** | **Response payload** |
|:---------------------------------------------------|------------|------------|------------|------------|
|   GET     |   MapGet      |     -       | /dishes <br/> /dishes/{dishId}  | dish collection  <br/> single dish|
|   POST    |   MapPost     | single dish | /dishes                         | single dish|
|   PUT     |   MapPut      | single dish | /dishes/{dishId}                | single dish or empty|
|   DELETE  |   MapDelete   |     -       | /dishes/{dishId}                | - |

## Using HTTP methods as intended improves the overall reliability of your system

> Different components in your architecture will rely on correct use of the HTTP standard
- Don’t introduce potential inefficiencies
- Don’t introduce potential bugs
> For reference, RFC9110
- https://datatracker.ietf.org/doc/html/rfc9110

URI
https:/localhost:7070/dishes/dab23b28-df91-47e1-948e-76bf3e0c1546

## Route Parameters
Gather input via the URI with route parameters

> URI

    https:/localhost:7070/dishes/dab23b28-df91-47e1-948e-76bf3e0c1546

> Route pattern

    app.MapGet("/dishes/{dishId}", …);

In the route template/pattern, use accolades to signify a route parameter

> URI

    https:/localhost:7070/dishes/dab23b28-df91-47e1-948e-76bf3e0c1546

> Route pattern + handler

    app.MapGet("dishes/{dishId}", () => (Guid dishId) => { … });

Route parameters from the URI will be bound to same-name parameters in the handler signature

```cs
// Get dish by id: guid
app.MapGet("dishes/{dishId:guid}", () => (Guid dishId) => { … });
// Get dish by name: string (implied constraint)
app.MapGet("dishes/{dishName}", () => (string dishName) => { … });
```

## Route Constraints

Route constraints allow constraining the matching behavior of a route

## Routing Concepts

- Route builder methods on IEndpointRoute Builder
- Route templates or patterns
- Route parameters
- Route constraints

## Working with routing templates

**Shouldn’t Expose the Entity Model**

- Entity model : A means to represent database rows as objects (object graphs)
- DTO model: A means to represent the data that’s sent over the wire

## Parameter binding

The process of converting request data into strongly typed parameters that are expressed by route handlers

## Parameter Binding Sources

- Route values 
- Query string 
- Header
- Body (as JSON) 
- Services provided by DI 
- Custom

```cs
app.MapGet("/dishes/{dishId:guid}", 
    async ( DishesDbContext dishesDbContext, 
            IMapper mapper,
            Guid dishId) => { … }
```

Use binding attributes to be explicit about the binding source

```cs
app.MapGet(
    "/dishes/{dishId:guid}", 
        async ( [FromServices] DishesDbContext dishesDbContext, 
                [FromServices] IMapper mapper,
                [FromRoute] Guid dishId) => { … }
);
```

## Rules for Selecting the Binding Source

Explicit binding ==> Special types

### Special Types for Binding

- HttpContext
- HttpRequest (bound from HttpContext.Request)
- HttpResponse (bound from HttpContext.Response)
- ClaimsPrincipal (bound from HttpContext.User)
- CancellationToken (bound from HttpContext.RequestAborted)

### Special Types for Binding

- IFormFileCollection (bound from HttpContext.Request.Form.Files)
- IFormFile (bound from HttpContext.Request.Form.Files[paramName])
- Stream (bound from HttpContext.Request.Body)
- PipeReader (bound from HttpContext.Reqeuest.BodyReader)

### Rules for Selecting the Binding Source

Explicit binding =>  Special types => BindingAsync => string or TryParse => Services => Request body 

### Filtering

Limiting a collection resource, taking into account a predicate

> Modelling Common API Functionality

    Filter via the query string, using name/value combinations for fields to filter on

> Searching 

    Searching for matching items in a collection based on a predefined set of rules

Headers: X-Pagination :{"pageNumber": 2, "pageSize": 5, "totalCount": 24, … }


## Minimal API Endpoint Return Values

- string 
- Any type but string
- IResult-based types: 
    - Results.X, 
    - TypedResults.X





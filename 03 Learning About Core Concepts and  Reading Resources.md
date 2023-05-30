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



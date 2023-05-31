# Structuring Your Minimal API

## Options for Structuring Minimal APIs

- Using methods instead of inline handlers
- Separating handler methods out in classes
- Extending IEndpointRouteBuilder

## Using methods instead of inline handlers

```cs
dishesEndpoints.MapGet("", async Task<Ok<IEnumerable<DishDto>>> (
    DishesDbContext dishesDbContext, 
    IMapper mapper,
    ClaimsPrincipal claimsPrincipal,
    string? name) =>
{
    Console.WriteLine($"User authenticated? {claimsPrincipal.Identity?.IsAuthenticated}");
    return TypedResults.Ok(mapper.Map<IEnumerable<DishDto>>(await dishesDbContext.Dishes
    .Where(d => name == null || d.Name.Contains(name)).ToListAsync()));
});
```
Improves maintainability and testability

```cs
dishesEndpoints.MapGet("", GetDishesAsync);
async Task<Ok<IEnumerable<DishDto>>> GetDishesAsync(
 DishesDbContext dishesDbContext,
 IMapper mapper,
 ClaimsPrincipal claimsPrincipal,
 string? name) 
{
 Console.WriteLine($"User authenticated? claimsPrincipal.Identity?.IsAuthenticated}");
 return TypedResults.Ok(mapper.Map<IEnumerable<DishDto>>(await dishesDbContext.Dishes
 .Where(d => name == null || d.Name.Contains(name)).ToListAsync()));
}
```

## Separating Handler Methods Out in Classes

```cs
public static class DishesHandlers
{
 public static async Task<Ok<IEnumerable<DishDto>>> GetDishesAsync(
 DishesDbContext dishesDbContext,
 IMapper mapper,
 ClaimsPrincipal claimsPrincipal,
 string? name)
 {
 …
 }
}

dishesEndpoints.MapGet("", DishesHandlers.GetDishesAsync);
```

## Extending IEndpointRouteBuilder

```cs
public static class EndpointRouteBuilderExtensions
{ 
 public static void RegisterDishesEndpoints( this IEndpointRouteBuilder app)
 {
 var dishesEndpoints = app.MapGroup("/dishes");
 dishesEndpoints.MapGet("", async Task<Ok<IEnumerable<DishDto>>> GetDishesAsync(
 DishesDbContext dishesDbContext,
 IMapper mapper,
 ClaimsPrincipal claimsPrincipal,
 string? name)
 { … }); 
}
}

app.RegisterDishesEndpoints();
// other extension methods
app.RegisterIngredientsEndpoints();
```

## Combine the Previous Approaches

```cs
public static class EndpointRouteBuilderExtensions
{ 
 public static void RegisterDishesEndpoints(this IEndpointRouteBuilder app)
 {
 var dishesEndpoints = app.MapGroup("/dishes");
 var dishWithGuidIdEndpoints = dishesEndpoints.MapGroup("/{dishId:guid}");
 dishesEndpoints.MapGet("", DishesHandlers.GetDishesAsync);
 dishWithGuidIdEndpoints.MapGet("", DishesHandlers.GetDishByIdAsync);
 dishesEndpoints.MapGet("/{dishName}", DishesHandlers.GetDishByDishNameAsync);
}
}
```
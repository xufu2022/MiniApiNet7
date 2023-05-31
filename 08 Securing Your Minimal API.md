# Securing Your Minimal API

- High-level API security overview
- Token-based security
    - Specific to minimal APIs
    - Options for token generation
- Authorization and authorization policies

There’s no “one size fits all” approach when it comes to securing clients & APIs 

## High-level API Security Overview

- Application-level and/or infrastructure level?
- On the same domain or cross-domain?
- Local or centralized users & credentials?
- Authentication and/or authorization?

component that requests a token => component that generates and provides tokens => component that requires and validates a token

Tokens are often JWTs but don’t have to be


## Our API validates a token. It does not generate it. It does not provide it

### Token-based Security for Minimal APIs

Authentication: the token contains verifiable info on who or what is accessing it
Delegation: the token allows access on behalf of a user or application


```cs
builder.Services.AddAuthentication();
builder.Services.AddAuthentication().AddJwtBearer();
```

## Token-based Security for Minimal APIs

Add and configure authentication services
Tie authentication to the JWT bearer authentication handler
```cs
builder.Services.AddAuthorization();
```

## Token-based Security for Minimal APIs

- Add and configure authorization services
- Require authorization for a certain (group of) endpoint(s)


```cs
builder.Services.AddAuthorization();
...
var ingredientsEndpoints = endpointRouteBuilder.MapGroup("/dishes/{dishId:guid}/ingredients").RequireAuthorization();
```

## Requiring a bearer token

Manually generate a token at level of your API
- /login endpoint
- Good for simple use cases, not a good approach for most scenarios

## Generating a Token

OAuth2 and OpenID Connect are standardized protocols for token-based security
- “Token based security on steroids”
Centralized identity providers implement these & generate tokens
- Azure AD, IdentityServer, Auth0, …

(ASP).NET Core includes a built-in tool to generate tokens which can be used during development
- dotnet-user-jwts

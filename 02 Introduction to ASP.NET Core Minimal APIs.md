# Introduction to ASP.NET Core Minimal APIs

## Building APIs with ASP.NET Core MVC

- Improves separation of concerns
- Improves testability
- Promotes reuse

## Building APIs with ASP.NET Core

- ASP.NET Core MVC : Fast, full-featured, proven and time-tested
- ASP.NET Core Minimal API : A bit faster, lightweight, less full-featured, relativel new

## Building APIs with ASP.NET Core Minimal API

- Supports routing, parameter binding, basic serialization & basic filters
- Intended for lightweight scenarios without all the ceremony that comes with MVC
    - Microservice APIs, aggregate/proxy APIs, relatively small APIs, …

## Consider what you want to rely on for building your API

- Built-in approach tends to lead to MVC approach
- Own code or external packages tend to lead to minimal API approach

## Introducing the Demo Application

We’ll build an API that exposes dishes and their ingredients
- Start from scratch
- Dummy data is provided
- Database integration with Entity Framework Core

# Manipulating Resources

## Routing, Revisited

-   Make sure the URLs make sense
    - Don’t create a dish via /people
-   Use nouns in URLs, not verbs
-   Don’t mix plural and singular nouns

## Things to Keep in Mind When Creating a Resource

- When working with parent/child relationships, validate whether the parent exists
- Don’t use the same endpoint for creating one item & a collection of items. Create a new endpoint instead: /itemcollections

## Things to Keep in Mind When Updating a Resource

- Check if the resource exist, including hierarchical parents
- Be careful when enabling PUT for collection resources: this can be very destructive
- PUT is intended for FULL updates, PATCH is for partial upates

- Change sets for PATCH requests are often described as a list of operations: a JsonPatchDocument
- There is no support for this for minimal APIs…

## Grouping resources

## Content negotiation
The process of selecting the best representation for a given response when there are multiple representations available 

Content negotiation is not supported out of the box by minimal APIs (nor is it planned)
If you need it, consider ASP.NET Core MVC

## Validation in Minimal APIs

Input validation is a common requirement 
- Required fields, format, value & length restriction, cross-field rules, …

## A Common Approach to Validation: Annotations
In ASP.NET Core MVC
- Automatic validation of incoming request bodies
- Manual validation with (Try)ValidateModel(...).

No built-in support for model validation for minimal APIs

## Summary

Group endpoints with **MapGroup** to apply common behavior to them
Generate links with the built-in **LinkGenerator**
Minimal APIs **don’t support content negotiation or validation** out of the box
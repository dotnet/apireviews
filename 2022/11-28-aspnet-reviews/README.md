# API Review 11/28/2022

## [Analyzer] Warn on inconsistent constraints between route params and types

**Approved** | [#aspnetcore/45218](https://github.com/dotnet/aspnetcore/issues/45218#issuecomment-1329588106)

API Review Notes:

1. Usage and error makes sense. We're confident these endpoints will be completely broken when hit at runtime.
2. This only fires for routes that have constraints already.
3. Given that this "[s]hould also work on MVC actions and Razor pages." How do you read from a slug in a razor page? What would be invalid? A `datetime` constraint?
4. `app.MapGet("/{id:int}", (long id) => ...);` should not trigger the analyzer because the route is more constrained than the parameter. It should fire for the opposite where the route is less constrained.
5. Should we have a fixer? It would be easy to suggest removing the constraint, but developers probably added them for a reason. However suggesting an `int` constraint for `int` parameters seems redundant. That would just turn 400 results into 404. tl;dr: We're fine without a fixer for now.

API Approved!

```
Message: $"The constraint '{constraint}' on parameter '{parameter}' can't be used with type {typeName}."
Category: Usage
Severity Level: Error
```

## Enable Option to Use Content Negotiation for ProblemDetails

**NeedsWork** | [#aspnetcore/45159](https://github.com/dotnet/aspnetcore/issues/45159#issuecomment-1329610038)

API Review Notes

1. Where is this options used? When writing problem details via the new service for MVC, the error handler middleware, status page result, the developer exception page.
2. Is there another name for `UseContentNegotiation` that can default for false? `IgnoreAcceptHeader`?
3. Can we change the default and make a breaking change announcement? The developer exception page will try to render to HTML first, so it shouldn't break that. @commonsensesoftware Do you think it would be too breaking to change the default here?

We'll hold off on approving this until we're confident what we want the default to be. Ideally, we'll have whatever we name the option false by default.

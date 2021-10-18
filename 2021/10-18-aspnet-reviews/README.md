# API Review 10/18/2021

## Expose Use(Func<RequestDelegate, RequestDelegate> middleware) on WebApplication

**Approved** | [#aspnetcore/37346](https://github.com/dotnet/aspnetcore/issues/37346#issuecomment-946012521)

API review: We think it's worthwhile having this, although I personally don't think it adds a great deal of value. That said, we already have two additional overloads, so a 3rd one isn't over kill. Approved.

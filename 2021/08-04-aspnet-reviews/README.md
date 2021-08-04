# API Review 08/04/2021

## Improve support for OpenAPI in minimal actions

**Approved** | [#aspnetcore/34906](https://github.com/dotnet/aspnetcore/pull/34906)

## Makes razor views (.cshtml) files reloadable

**Approved** | [#aspnetcore/34248](https://github.com/dotnet/aspnetcore/issues/34248#issuecomment-892863156)

API feedback - let's generate RazorCompiledItemAttribute on the type + assembly rather than introduce a new type. The attribute is now allowed to be applied to classes as part of this change.
## Add ISupportsConfigureWebHost interface to hosting.

**Approved** | [#aspnetcore/34824](https://github.com/dotnet/aspnetcore/issues/34824#issuecomment-892867218)

API review feedback - we think the names are fine. Let's put this in a non-root namespace `Microsoft.AspNetCore.Hosting.Infrastructure` since we don't want casual users seeing this type.
## Support specifying host configuration via WebApplication.CreateBuilder 

**Approved** | [#aspnetcore/34837](https://github.com/dotnet/aspnetcore/issues/34837#issuecomment-892873169)

API feedback -> `WebApplicationOptions` -> `WebApplicationBuilderOptions`. Discussion about `args` array being preferred over explicitly set properties. The behavior should be documented as part of the property description.
## .NET to JS Streaming Interop API Proposal

**Approved** | [#aspnetcore/35009](https://github.com/dotnet/aspnetcore/issues/35009#issuecomment-892880239)

API feedback - we discussed making the properties on `DotNetStreamReference` non public, but they're used across assembly boundaries. Exposing an overload that accepts a `Stream` was also considered, but that adds more complexity particularly because we want to have a non-disposing stream.
## [SignalR TS] Pass timeout when invoking http requests

**Approved** | [#aspnetcore/34207](https://github.com/dotnet/aspnetcore/pull/34207#issuecomment-892887168)

API review:

```diff
-  httpTimeout?: number;
+ timeout?: number;
```

## Make HubConnection more testable

**Approved** | [#aspnetcore/34843](https://github.com/dotnet/aspnetcore/issues/34843#issuecomment-892890156)

API review - Let's make all of these APIs `virtual`. The names are somewhat unfortunate, but since they're already a public, we can't change them.
## [Routing] API review for pull/33712

**Approved** | [#aspnetcore/35042](https://github.com/dotnet/aspnetcore/issues/35042#issuecomment-892893848)

API review:

Let's put this in a subnamespace:  `Microsoft.AspNetCore.Routing.Matching`

```C#
namespace Microsoft.AspNetCore.Routing.Matching;
public interface IParameterLiteralNodeMatchingPolicy : IParameterPolicy
{
    bool MatchesLiteral(string parameterName, string literal);
}
```


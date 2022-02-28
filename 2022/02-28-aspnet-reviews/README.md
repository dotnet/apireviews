# API Review 02/28/2022

## Add Results.Empty

**Approved** | [#aspnetcore/40168](https://github.com/dotnet/aspnetcore/issues/40168#issuecomment-1054550507)

API review:

We'll use properties instead. Here's the approved API:

```diff
public static class Results
{
+   public static IResult Empty { get; } = new EmptyResult();
}

public class ControllerBase
{
+    public static EmptyResult Empty { get; } = new ();
}
```
## RequestDecompression middleware

**Approved** | [#aspnetcore/40080](https://github.com/dotnet/aspnetcore/issues/40080#issuecomment-1054578813)

API review:

Approved. Update the `IRequestDecompressionProvider` to return the Stream instead of the provider:

```diff
+ public interface IRequestDecompressionProvider
+ {
+     Stream? GetDecompressionStream(HttpContext context);
+ }
```

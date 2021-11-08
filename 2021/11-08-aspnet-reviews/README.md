# API Review 11/08/2021

## Add metadata for IFormFile usage with minimal actions

**Approved** | [#aspnetcore/35175](https://github.com/dotnet/aspnetcore/issues/35175#issuecomment-963475874)

API review: We think we might be able to get away re-using the existing `FromFormAttribute` and specialize the swagger based on if it's a `IFormFile` / `IFormFileCollection` instance.

```diff
+ public interface IFromFormMetadata
+ {
+     string? Name { get; }
+ }

- public class FromFormAttribute : Attribute, IBindingSourceMetadata, IModelNameProvider
+ public class FromFormAttribute : Attribute, BindingSourceMetadata, IModelNameProvider, IFromFormMetadata
```

Usage:

```C#
app.MapPost("/upload", async ([FromForm("file-name")] IFormFile file) => { ... });

app.MapPost("/upload", async ([FromForm] IFormFileCollection file) => { ... }); 

app.MapPost("/upload", async (IFormFile myFile) => { ... });

app.MapPost("/upload", async (IFormFileCollection myFiles) => { ... });

// Unsupported. Analyzer error. This can be done in a follow up
app.MapPost("/upload", async ([FromForm("myFiles")] IFormFileCollection file) => { ... }); 
```

For `IFormFileCollection`, we do not wanted based on the parameter name or `IFromFormAttribute.Name` (at least to start with). We can make the latter an error via an analyzer + a runtime check.



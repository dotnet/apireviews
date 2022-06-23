# API Review 06/23/2022

## Introduce package for rendering Blazor components with custom HTML elements

**Approved** | [#aspnetcore/42329](https://github.com/dotnet/aspnetcore/issues/42329#issuecomment-1164681562)

New package: Microsoft.AspNetCore.Componenets.CustomElements

```diff
+ namespace Microsoft.AspNetCore.Components.Web;

+ public static class CustomElementsJSComponentConfigurationExtensions
+ {
+     public static void RegisterCustomElement<TComponent>(this IJSComponentConfiguration configuration, string identifier) where TComponent : IComponent
+ }
```

API approved

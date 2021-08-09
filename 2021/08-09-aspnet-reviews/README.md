# API Review 08/09/2021

## [SignalR] Allow controlling maximum buffer size on client

**Approved** | [#aspnetcore/17797](https://github.com/dotnet/aspnetcore/issues/17797#issuecomment-895409881)

API review: We should document that setting this to  `0` will disable buffering (edit: `-1` is "default" not unlimited). In addition, we should use the `value` node because it exists. API review noted that the names were poor choices, but they match the server names and it's in our best interest to keep them consistent.
## Add method to create ConnectionContext from Socket

**Approved** | [#aspnetcore/34522](https://github.com/dotnet/aspnetcore/issues/34522)

## Querystring building APIs for Blazor

**Approved** | [#aspnetcore/34115](https://github.com/dotnet/aspnetcore/issues/34115#issuecomment-895423121)

API review:

* `UriWithQueryParameter` -> `GetUriWithQueryParameter`
* `UriWithQueryParameters` -> `GetUriWithQueryParameters`

We also decided to remove all the `IEnumerable<{DataType}>` overloads.

```diff
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<string?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<bool> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<bool?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<DateTime> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<DateTime?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<decimal> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<decimal?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<double> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<double?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<float> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<float?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<Guid> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<Guid?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<int> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<int?> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<long> values)
-        public static string UriWithQueryParameter(this NavigationManager navigationManager, string name, IEnumerable<long?> values)
```



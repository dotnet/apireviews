# Quick Reviews 6/2/2020

## Configuring request options in Browser WebAssembly

**Approved** | [#runtime/34168](https://github.com/dotnet/runtime/issues/34168#issuecomment-637712249) | [Video](https://www.youtube.com/watch?v=P1w3Tc7Oyqk&t=0h0m0s)

* It doesn't seem we need the `IHttpRequestOptions`
* We should combine the options with the `Properties` collection the message
* We don't believe we need the overloads on `HttpClient` that take options; it feels like an advanced scenario which people can achieve by constructing the message
* Users can store mismatched types via `IDictionary<string, object>`, but we should verify types on retrieval and throw an exception

```C#
namespace System.Net.Http
{
    public class HttpRequestOptions : IDictionary<string, object>
    {
        // Explicit interface implementation
        public bool TryGetValue<TValue>(HttpRequestOptionsKey<TValue> key, out TValue value);
        public void Set(HttpRequestOptionsKey<TValue> key, TValue value);
    }

    public class HttpRequestMessage : IDisposable
    {
        [Obsolete("Use Options instead.")]
        public IDictionary<string, object> Properties => Options;

        public HttpRequestOptions Options { get; set; }
    }
}
```
## ICastableObject interface

**Approved** | [#runtime/36654](https://github.com/dotnet/runtime/issues/36654#issuecomment-637742472) | [Video](https://www.youtube.com/watch?v=P1w3Tc7Oyqk&t=1h5m10s)

* Looks good, but we should make the type name less attractive, such as `IDynamicInterfaceCastable`
* The implementer of `IDynamicInterfaceCastable` has to abide to the contract of only throwing when `throwIfNotFound` is `true` and even then should only throw `InvalidCastException`. However, the runtime doesn't handle the throwing so that implementers can tweak the message
* However, it is valid for the implementer to return `default` and the let the runtime throw
* It would be nice if `throwIfNotFound` would be renamed to indicate whether it's an `is`/`as` or a hard cast

```C#
namespace System.Runtime.InteropServices
{
    public interface IDynamicInterfaceCastable
    {
        RuntimeTypeHandle GetInterfaceImplementation(RuntimeTypeHandle interfaceType, bool throwIfNotFound);
    }
    [AttributeUsage(AttributeTargets.Interface,
                    AllowMultiple = false,
                    Inherited = false)]
    public sealed class DynamicInterfaceCastableImplementationAttribute : Attribute
    {
    }
}
```

# Quick Reviews 4/2/2019

## Evolving EventCounter API

**Approved** | [#corefx/36129](https://github.com/dotnet/corefx/issues/36129#issuecomment-479128603) | [Video](https://www.youtube.com/watch?v=x5vbM3zB0_I&t=0h-52m-10s)

* MetaData -> `Metadata`
* `AddMetaData` to `SetMetaData
* Add a `DiagnosticCounter` abstract base
    - Have protected ctor that accepts the name and `EventSource`
    - Add `Name` get-only property
    - Add `EventSource` get-only property
    - Add `DisplayName` get/set property
    - Add abstract `SetMetadata(string key, string value) method
    - All counters, including the existing `EventCounter` should inherit from it
* `getMetricFunction` -> `metricProvider`
* `getCountFunction` -> `totalValueProvider`
* We should consider using `double` instead of `float`
## SSLStream Allow Configuration of CipherSuites

**Approved** | [#corefx/33809](https://github.com/dotnet/corefx/issues/33809#issuecomment-479132747) | [Video](https://www.youtube.com/watch?v=x5vbM3zB0_I&t=1h10m58s)

Going through the builder feels odd; especially because it doesn't itself represent a collection. It seems more natural to do something like:

```C#
public sealed class CipherSuitesPolicy
{
    public CipherSuitesPolicy(IEnumerable<TlsCipherSuite> allowedCipherSuites);

    public IEnumerable<TlsCipherSuite> AllowedCipherSuites { get; }
}
```

It was proposed to apply hide the `AllowedCipherSuites` property from other types; we don't believe that's the way to give guidance. Rather, we should document the API, provide relevant guidance developers are likely searching for (e.g. "How to properly configure SSL with TLS"). We should also invest in an analyzer.
## Simpler way to specify leaveOpen in StreamWriter constructor

**Approved** | [#corefx/8173](https://github.com/dotnet/corefx/issues/8173#issuecomment-479149933) | [Video](https://www.youtube.com/watch?v=x5vbM3zB0_I&t=1h24m49s)

We'd prefer the first option (i.e. default constructors), reason being:

1. It works for all settings, including the ones that we can't change after construction
2. We can apply it elsewhere, such as `FileStream`

The counter argument is that it might make it harder for folks to find bugs because we'd now accept `null`, but that's already the case in the framework. Hopefully, non-nullable reference types can help with that.

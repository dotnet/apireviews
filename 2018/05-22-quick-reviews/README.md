# Quick Reviews 5/22/2018

## Adding GetChunks which allow efficient scanning of a StringBuilder

**Approved** | [#corefx/29770](https://github.com/dotnet/corefx/issues/29770#issuecomment-391080101) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=0h0m0s)

Looks good, only change is that we think we should collapse the two types:

```csharp
class StringBuilder {
        public ChunkEnumerator GetChunks();

        public struct ChunkEnumerator
        {
            [EditorBrowsable(Never)] // Only here to make foreach work
            public ChunkEnumerator GetEnumerator();
            public bool MoveNext();
            public ReadOnlyMemory<char> Current { get; }
        }
}
```

* Potentially we can add `CurrentSpan` to make it cheaper to get the span
* We could also in the future expose a `CurrentSpanReadWrite` if we ever need a mutating version.
## Exception.HResult setter should be made public

**Approved** | [#corefx/29696](https://github.com/dotnet/corefx/issues/29696) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=0h37m47s)

## Add SpinWait.SpinOnce overload to specify or disable the Sleep(1) threshold

**Approved** | [#corefx/29623](https://github.com/dotnet/corefx/issues/29623#issuecomment-391085259) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=0h42m36s)

* Looks good but we should shorten the name.
* Are there any other thresholds that we need to expose?

```csharp
public struct SpinWait
{
    public void SpinOnce(int sleep1Threshold);
}
```
## Enable EnvelopedCms to work with an external private key

**Approved** | [#corefx/29327](https://github.com/dotnet/corefx/issues/29327#issuecomment-391088582) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=0h53m21s)

Looks good, after minor name change:

```csharp
public partial class EnvelopedCms
{
     public void Decrypt(RecipientInfo recipientInfo, AsymmetricAlgorithm privateKey);
}

public partial class SubjectIdentifier
{
     public bool MatchesCertificate(X509Certificate2 certificate);
}
```
## Add DecompressionMethods.All?

**Approved** | [#corefx/29243](https://github.com/dotnet/corefx/issues/29243#issuecomment-391095592) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=1h3m36s)

Seems reasonable.
## Implement portable support for TCP_KEEPCNT, TCP_KEEPIDLE and TCP_KEEPINTVL socket options

**Approved** | [#corefx/25040](https://github.com/dotnet/corefx/issues/25040#issuecomment-391097721) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=1h26m10s)

Looks good.

Can we double check what happens if options are no-ops or are unsupported. Do we throw? If so, should the name of the option indicate that things might be unsupported or are no-ops?
## [Cookie] CookieCollection should implement ICollection<Cookie>

**Approved** | [#corefx/26975](https://github.com/dotnet/corefx/issues/26975#issuecomment-391098529) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=1h30m40s)

Looks good but for consistency we should also implement `IReadOnlyCollection<>`.
## Support struct Enumerator for ConcurrentDictionary

**Needs Work** | [#corefx/28046](https://github.com/dotnet/corefx/issues/28046#issuecomment-391102771) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=1h34m28s)

The options listed here are all somewhat unfortunate (that is, they suck for various reasons :-)).

One compromise would be to add a new method called `GetEnumerator2()` and change the compiler to support foreach-ing enumerators and code would like this:

```csharp
var dict = new ConcurrentDictionary<string, int>();
// add stuff

foreach (var kv in dict.GetEnumerator2())
{
    Use(enumerator.Current);
}
```

The established patterns for struct-based alternative is `ValueXxx`, like `ValueTuple` or `ValueTask` but in this case `value` might be a bad idea due to the generic parameter `TValue`.

@jaredpar, are you still supportive of this change to the compiler?
@stephentoub, are you OK with the resulting usage?
## TypeInfo doesn't expose a parameterless constructor

**Approved** | [#corefx/14334](https://github.com/dotnet/corefx/issues/14334#issuecomment-391103797) | [Video](https://www.youtube.com/watch?v=-6wyH7K4L9g&t=1h47m52s)

Let's start doing this in .NET Core. This doesn't help the RefEmit case for .NET Standard, but we gonna start somewhere :-)

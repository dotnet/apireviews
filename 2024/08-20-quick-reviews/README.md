# API Review 08/20/2024

## ArrayRecord.TotalElementsCount

**Approved** | [#runtime/106644](https://github.com/dotnet/runtime/issues/106644#issuecomment-2299397896) | [Video](https://www.youtube.com/watch?v=BWMC5Xh4piI&t=0h0m0s)

* We renamed TotalElementsCount to FlattenedLength to match the same concept on TensorSpan
* The hierarchy is closed, so virtual or not is author's discretion.

```C#
namespace System.Formats.Nrbf;

public abstract partial class ArrayRecord : SerializationRecord
{
    public virtual long FlattenedLength { get; }
}
```
## AsnWriter Encode with callback

**Approved** | [#runtime/75759](https://github.com/dotnet/runtime/issues/75759#issuecomment-2299433962) | [Video](https://www.youtube.com/watch?v=BWMC5Xh4piI&t=0h30m15s)


* We talked about the consequences and future upgradability of TReturn not being `allows ref struct`, and are good with it as proposed.

```C#
namespace System.Formats.Asn1;

public partial class AsnWriter {
#if NET9_0_OR_GREATER
    public TReturn Encode<TReturn>(Func<ReadOnlySpan<byte>, TReturn> encodeCallback);
    public TReturn Encode<TState, TReturn>(TState state, Func<TState, ReadOnlySpan<byte>, TReturn> encodeCallback)
        where TState : allows ref struct;
#endif
}
```


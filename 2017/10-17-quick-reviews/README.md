# Quick Reviews 10/17/2017

## Rename MemoryHandle.PinnedPointer to Pointer

**Approved** | [#corefx/24426](https://github.com/dotnet/corefx/issues/24426#issuecomment-337305000) | [Video](https://www.youtube.com/watch?v=BEBq3__WfDc&t=0h0m0s)

Looks good as proposed. Also, rename the constructor.

AND NEXT TIME INCLUDE THE API REF.
## Disposables in CoreFx: SerialDisposable, CompositeDisposable, etc

**Needs Work** | [#corefx/14891](https://github.com/dotnet/corefx/issues/14891) | [Video](https://www.youtube.com/watch?v=BEBq3__WfDc&t=0h18m8s)

## Add DateTime.UnixEpoch and DateTimeOffset.UnixEpoch fields

**Approved** | [#corefx/24449](https://github.com/dotnet/corefx/issues/24449#issuecomment-337315763) | [Video](https://www.youtube.com/watch?v=BEBq3__WfDc&t=0h40m23s)

Looks good as proposed. We may want to add the `ToUnixXxx` and `FromUnixXxx` to `DateTime` as well though.
## [ExcludeFromCodeCoverageAttribute] should be applicable to assemblies

**Approved** | [#corefx/24694](https://github.com/dotnet/corefx/issues/24694) | [Video](https://www.youtube.com/watch?v=BEBq3__WfDc&t=0h51m47s)

## Add Base64 conversion APIs that support UTF-8 for Span<T> 

**Approved** | [#corefx/24568](https://github.com/dotnet/corefx/issues/24568#issuecomment-337334707) | [Video](https://www.youtube.com/watch?v=BEBq3__WfDc&t=1h49m6s)

This needs more work. Raw notes:

### Rough shape of API

```C#
// System.Memory.dll
namespace System.Buffers.Text {
    public static class Base64 {
        public static OperationStatus EncodeToUtf8(ReadOnlySpan<byte> bytes, Span<byte> utf8, out int consumed, out int written, bool isFinalBlock = true);
        public static OperationStatus EncodeToUtf8InPlace(Span<byte> buffer, int bytesLength, out int written, bool isFinalBlock = true);
        public static int GetMaxEncodedUtf8Length(int length);
        public static OperationStatus DecodeFromUtf8(ReadOnlySpan<byte> utf8, Span<byte> bytes, out int consumed, out int written, bool isFinalBlock = true);
        public static OperationStatus DecodeFromUtf8InPlace(Span<byte> buffer, out int consumed, out int written, bool isFinalBlock = true);
        public static int GetMaxDecodedBytesLength(int length);
    }
}
```

### isFinalBlock

We need it, using a more general term (as opposed to `usePadding`) avoids the caller having to understand the format/specification.

### OperationStatus/TransformationStatus

It was brought up that we may not need the enum at all.

@ahsonkhan /Levi: Look at the general pattern and decide whether the API is needed. In the past we concluded that the code becomes too convoluted and the conditions are easy to get wrong.

### Length

We should not take the input, only the size. We should also not take the `isFinalBlock`. Basically, the contract is "give an upper bound".



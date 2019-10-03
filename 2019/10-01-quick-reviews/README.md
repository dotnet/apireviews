# Quick Reviews 10/1/2019

## Add Encoding/Decoding APIs for new System.Buffer types

**Approved** | [#corefx/41166](https://github.com/dotnet/corefx/issues/41166#issuecomment-537141144) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=0h0m0s)

* The signatures makes sense
* Should also add 
    ```C#
    public static long GetByteCount(this Encoding encoding, in ReadOnlySequence<char> chars);
    public static long GetCharCount(this Encoding encoding, in ReadOnlySequence<byte> bytes);
    ```
* Making this an extension seems slightly sad; is this the point where we should consider movign `ReadOnlySequence<T>`, `IBfferWriter<T>` into corlib? Can anybody think of scenarios where we'd like those types down?
* @GrabYourPitchforks filed #41474 to make these instance methods should we move the types down
## Add Encoder/Decoder for new System.Buffer types

**Approved** | [#corefx/41326](https://github.com/dotnet/corefx/issues/41326#issuecomment-537150026) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=0h15m18s)

* Should these be `OperationStatus` APIs? We decided not to, because we modeled these overloads after the existing APIs.
* We considered replacing the outed sequence position with an `int`, but slicing with a position is simpler and more performant (because O(1)).
* We should rename `bytesUsedPosition` to just `position`
* Same as #41166: if we move `ReadOnlySequence<T>` and `IBufferWriter<T>` to corlib, we should make those instance methods.
## Add List<T> AsSpan to CollectionsMarshal

**Approved** | [#corefx/31597](https://github.com/dotnet/corefx/issues/31597#issuecomment-537151655) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=0h35m57s)

We decided to not increment the version, which is why we removed the `AsReadOnlySpan()` overload.

```C#
namespace System.Runtime.InteropServices
{
    public partial static class CollectionsMarshal
    {
        public static Span<T> AsSpan(List<T>? list);
    }
}
```
## a new GC API for large array allocation

**Approved** | [#corefx/31787](https://github.com/dotnet/corefx/issues/31787#issuecomment-537157276) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=0h39m36s)

Looks good as proposed.

```C#
namespace System
{
    public partial class GC
    {
        public static T[] AllocateArray<T>(int length, int generation=-1, bool pinned=false, int alignment=-1);
        public static T[] AllocateUninitializedArray<T>(int length, int generation=-1, bool pinned=false, int alignment=-1);
    }
}
```
## long ReadOnlySequence<T>.GetPosition(SequencePosition)

**Approved** | [#corefx/31807](https://github.com/dotnet/corefx/issues/31807#issuecomment-537160854) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=0h52m58s)

Given that the nomenclature uses `offset` for `long`-based absolute values and `position` for a `SequencePosition` value, we should be using `GetOffset()`. This also solves some overload issue that only @tannergooding understands 😄 

```C#
namespace System
{
    public struct ReadOnlySequence<T>
    {
       public long GetOffset(SequencePosition position);
    }
}
```
## Color should implement FromHsl methods

**Rejected** | [#corefx/31820](https://github.com/dotnet/corefx/issues/31820#issuecomment-537164275) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=1h1m58s)

* It [sounds like](https://stackoverflow.com/questions/15668623/hsb-vs-hsl-vs-hsv) HSL is different from HSB.
* Given that your sample code uses `GetBrightness()` as the input to `FromHsl`, is it possible that you really meant `FromHsb()`? 
## Extend CultureInfo.GetCultureInfo and new CultureInfo to optionally accept predefined cultures only

**Approved** | [#corefx/32201](https://github.com/dotnet/corefx/issues/32201#issuecomment-537177127) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=1h10m15s)

After some debate we settled on this:

```C#
namespace System.Globalization
{
    public partial class CultureInfo
    {
        public static CultureInfo GetCulture(string name, bool prefinedOnly);
    }
}
```

This API would throw for invalid names as well as names that weren't system provided when `predefinedOnly` is `true`.
## Add remove Range to sorted List

**Rejected** | [#corefx/32987](https://github.com/dotnet/corefx/issues/32987#issuecomment-537183526) | [Video](https://www.youtube.com/watch?v=b-Hz_CBobfU&t=1h39m50s)

The generic sorted list doesn't allow indexing by `int`, however, it does expose a `RemoveAt` method. But we'd have to make sure this plays nicely for cases where custom compares are used that, for example, take into consideration the number of elements and use different sorting.

Right now, we're not convinced the usability and minor performance gains are motivating enough. Feel free to push back though and provide more arguments :-)

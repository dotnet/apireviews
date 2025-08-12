# API Review 08/12/2025

## Introduce Azure VM ambient metadata

**Rejected** | [#extensions/6613](https://github.com/dotnet/extensions/issues/6613#issuecomment-3180299143) | [Video](https://www.youtube.com/watch?v=BeoDUMLNAFw&t=0h0m0s)

This seems like something that belongs in the Azure SDK layer, not this layer.

We didn't really look at any of the details here, as they may have different conventions, etc, that might make any of our feedback here moot.
## Indexed version of `Utf8.IsValid`

**Approved** | [#runtime/118113](https://github.com/dotnet/runtime/issues/118113#issuecomment-3180495126) | [Video](https://www.youtube.com/watch?v=BeoDUMLNAFw&t=0h15m6s)

* There was a lively debate for the name of what goes after "Invalid".  In the end, the best answer we came up with was Subsequence.
  * This is one of the rare cases where we had to decide by vote.
* For ASCII, there's tension on the name Subsequence there, but also not making it match on Utf8/Utf16

```C#
namespace System.Text.Unicode;

public static class Utf8
{
    public static int IndexOfInvalidSubsequence(ReadOnlySpan<byte> value) => ...;
}

public static class Utf16
{
    public static int IndexOfInvalidSubsequence(ReadOnlySpan<char> value) => ...;
}
```
## UTF-16 version of `Utf8.IsValid`

**Approved** | [#runtime/118018](https://github.com/dotnet/runtime/issues/118018#issuecomment-3180499505) | [Video](https://www.youtube.com/watch?v=BeoDUMLNAFw&t=1h13m5s)

* IsWellFormed is a better name, but IsValid matches Utf8.
* Looks good as proposed (but not on `String`).

```C#
namespace System.Text.Unicode;

public static class Utf16
{
    public static bool IsValid(ReadOnlySpan<char> value) => ...;
}
```
## JsonSerializerOptions.GetTypeInfo<T>

**Approved** | [#runtime/118468](https://github.com/dotnet/runtime/issues/118468#issuecomment-3180537330) | [Video](https://www.youtube.com/watch?v=BeoDUMLNAFw&t=1h14m21s)

* The method should be an instance method instead of an extension method.
* Let's go ahead and approve the Try at the same time.

```c#
namespace System.Text.Json;

public partial class JsonSerializerOptions
{
    public JsonTypeInfo<T> GetTypeInfo<T>();
    public bool TryGetTypeInfo<T>([NotNullWhen(true)] out JsonTypeInfo<T>? typeInfo);
}
```

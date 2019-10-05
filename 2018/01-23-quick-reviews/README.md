# Quick Reviews 1/23/2018

## Update HttpStatusCode enum with updates

**Approved** | [#corefx/4382](https://github.com/dotnet/corefx/issues/4382) | [Video](https://www.youtube.com/watch?v=m3Q6MENq17g&t=-16h-57m-36s)

## Add application/json to System.Net.Mime.MediaTypeNames.Application

**Approved** | [#corefx/26201](https://github.com/dotnet/corefx/issues/26201) | [Video](https://www.youtube.com/watch?v=m3Q6MENq17g&t=0h4m27s)

## Unsafe API for comparing byrefs as pointers

**Approved** | [#corefx/26451](https://github.com/dotnet/corefx/issues/26451#issuecomment-359887092) | [Video](https://www.youtube.com/watch?v=m3Q6MENq17g&t=0h6m23s)

We think these names align closer to the framework naming:

```C#
public static class Unsafe
{
    public static bool IsAddressLessThan<T>(ref T left, ref T right);
    public static bool IsAddressGreaterThan<T>(ref T left, ref T right);
}
```

## New APIs for Accessing RSS Optional Elements

**Approved** | [#corefx/25718](https://github.com/dotnet/corefx/issues/25718#issuecomment-359890337) | [Video](https://www.youtube.com/watch?v=m3Q6MENq17g&t=0h28m53s)

* `TimeToLive` should throw when the `TimeSpan` when the time span cannot be represented as an `int`.

```C#
namespace System.ServiceModel.Syndication
{
    public partial class SyndicationFeed
    {
        public SyndicationLink Documentation { get; set; }
        public Collection<string> SkipDays { get; }
        public Collection<int> SkipHours { get; }
        public SyndicationTextInput TextInput { get; set; }
        public TimeSpan? TimeToLive { get; set; }
    }
    public partial class SyndicationTextInput
    {
        public string Description { get; set; }
        public SyndicationLink Link { get; set; }
        public string Name { get; set; }
        public string Title { get; set; }
        public SyndicationTextInput();
    }
}
```
## String-like extension methods to ReadOnlySpan<char> Epic

**Approved** | [#corefx/21395](https://github.com/dotnet/corefx/issues/21395#issuecomment-359906138) | [Video](https://www.youtube.com/watch?v=m3Q6MENq17g&t=0h40m44s)

* We can expose all these APIs from MemoryExtensions, even the ones that need globalization APIs as this just means we can't expose them (yet).
* We'll not expose APIs that make `StringComparison` or `CultureInfo` implicit. We might get feedback, but we think forcing the developer to make a decision is less prone to errors than selecting a default.
* We've removed `Replace` as we don't think the API interaction / pattern works out. It need more thought/design.

```csharp
public static class MemoryExtensions
{
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);

    public static bool IsWhiteSpace(this ReadOnlySpan<char> span);

    public static void Remove(this ReadOnlySpan<char> source, int startIndex, int count, Span<char> destination);    
    public static void PadLeft(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination);
    public static void PadLeft(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination, char paddingChar);
    public static void PadRight(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination);
    public static void PadRight(this ReadOnlySpan<char> source, int totalWidth, Span<char> destination, char paddingChar);

    // Those need access to globalization APIs. We'll also expose them from
    // the .NET Framework OOB (slow span). They will try to extract the string
    // from the underlying span (because slow span stores it) -- or -- allocate
    // a new string. This avoids bifurcating the API surface.

    public static bool Contains(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool EndsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool Equals(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static bool StartsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static int CompareTo(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);
    public static int IndexOf(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparison);

    public static void ToLower(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
    public static void ToLowerInvariant(this ReadOnlySpan<char> source, Span<char> destination);
    public static void ToUpper(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
    public static void ToUpperInvariant(this ReadOnlySpan<char> source, Span<char> destination);
}
```

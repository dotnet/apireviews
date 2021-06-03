# API Review 06/03/2021

## Non-validated HttpHeaders enumeration

**Approved** | [#runtime/35126](https://github.com/dotnet/runtime/issues/35126#issuecomment-854040319) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=0h0m0s)

The proposed amendments look good.  The constructors can always be added later when there's a compelling scenario.

The net approved shape is now

```C#
namespace System.Net.Http.Headers
{
    public partial class HttpHeaders
    {
        public HttpHeadersNonValidated NonValidated { get; }
    }

    public readonly struct HttpHeadersNonValidated : 
         IReadOnlyDictionary<string, HeaderStringValues>
    {
         public int Count { get; }
         public bool Contains(string headerName);
         public bool TryGetValues(string headerName, out HeaderStringValues values);
         public HeaderStringValues this[string headerName];
         public Enumerator GetEnumerator();
         public readonly struct Enumerator : IEnumerator<KeyValuePair<string, HeaderStringValues>>
         {
             public bool MoveNext();
             public KeyValuePair<string, HeaderStringValues> Current { get; }
             public void Dispose();
             ... // explicitly implemented interface members
         }
         ... // explicitly implemented interface members
    }

    public readonly struct HeaderStringValues : 
        IReadOnlyCollection<string>
    {
         public int Count { get; }
         public override string ToString();
         public Enumerator GetEnumerator();
         public readonly struct Enumerator : IEnumerator<string>
         {
             public bool MoveNext();
             public string Current { get; }
             public void Dispose();
             ... // explicitly implemented interface members
         }
         ... // explicitly implemented interface members
    }
}
```
## Do not do math with local DateTime

**NeedsWork** | [#runtime/33764](https://github.com/dotnet/runtime/issues/33764#issuecomment-854046706) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=0h14m3s)

It's unclear what is being proposed here.  The examples seem to focus on the interaction with DateTimeKind.Local and DateTimeKind.Utc, not "do[ing] math with local DateTime", as the title suggests.

It's also not clear if this is realistically achievable, since any DateTime that comes in as a method parameter has a DateTimeKind that is not known to the called method (unless something coerced it).

Perhaps an updated title, or more clear examples would help drive a discussion.
## Do not pass Utf8JsonReader by value

**Approved** | [#runtime/33772](https://github.com/dotnet/runtime/issues/33772#issuecomment-854061368) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=0h22m24s)

Severity: Warning
Category: Reliability

* The analyzer, and the messaging, should be more generalized, like "Do not pass mutable value type '{0}' by value."
* We should have a list of well-known problematic types, like Utf8JsonReader, but also support loading other types from config.
  * Some types may be able to be identified heuristically, like "ends in Enumerator, is a value type, and is a nested type"
  * The list should also include SpinLock
* The analyzer should look for method parameter declarations where the parameter is of one of these types and the parameter mode is not ref or out (either by-value or in/readonly-ref).
  * It should also look for these types in "output positions", like property declared types or method returns.
* The fixer should change the parameter to be by ref, and then (as a stretch goal) update calls appropriately (if able).

## Prefer static ReadOnlySpan<byte> properties over static readonly byte[] fields

**Approved** | [#runtime/33780](https://github.com/dotnet/runtime/issues/33780#issuecomment-854077948) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=0h47m22s)

Category: Performance
Level: Info

* The analyzer should look at all static readonly field declarations where the field type is an array of byte/sbyte/bool, the array is initialized with a literal array initializer.  (Note that in the future some other types may also be allowed, but that will depend on the presence of a RuntimeFeatures feature)
  *  Then do a candidate elimination round: Any time that the array was passed to some other method as an array (vs going through AsSpan explicitly or implicitly) then do not report on this field.
* Any remaining fields can be changed to the new pattern.

## Avoid unnecessary string concatenation

**NeedsWork** | [#runtime/33783](https://github.com/dotnet/runtime/issues/33783#issuecomment-854088293) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=1h13m9s)

It's not clear that the resulting code is "better", especially if it inlines a concatenation across conditional statements (duplicated code costs vs performance benefits).  There may be room for "StringBuilder would be better here", but they're hard to describe.

Separating out the unconditional (unobserved) concatenation into a distinct analyzer/diagnostic would be less controversial; but the conditional cases are definitely more complicated.
## Override Stream.ReadByte/WriteByte

**Rejected** | [#runtime/33788](https://github.com/dotnet/runtime/issues/33788#issuecomment-854101396) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=1h31m35s)

Knowing to override ReadByte and WriteByte is something that would show up in a performance analysis, and a performance-improvements focus would likely have already highlighted it.

Without usage on a specific Stream-derived type saying that ReadByte/WriteByte were even called, this is too low value considering the already high cost of implementing a Stream "properly", and so doesn't seem justified.

The group got sidetracked by theorizing about a `DelgatingStream : Stream` type and a new intermediate `Stream` type that rewrites everything in terms of fewer protected abstract methods focused on Span/Memory... but that's not really what this issue was about.
## Prefer Span<T>.Clear() over Span<T>.Fill(default)

**Approved** | [#runtime/33813](https://github.com/dotnet/runtime/issues/33813#issuecomment-854102050) | [Video](https://www.youtube.com/watch?v=m8JAKGO8zrI&t=1h54m32s)

Level: Info
Category: Performance

Looks good as proposed.

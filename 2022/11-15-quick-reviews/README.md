# API Review 11/15/2022

## Add System.Runtime.CompilerServices.RefSafetyRulesAttribute

**Approved** | [#runtime/76032](https://github.com/dotnet/runtime/issues/76032#issuecomment-1315701185) | [Video](https://www.youtube.com/watch?v=73BAn9B3ni4&t=0h0m0s)

* We changed the Version field to a Version get-only property
* Otherwise, looks good as proposed.

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Module, AllowMultiple = false, Inherited = false)]
    public sealed class RefSafetyRulesAttribute : Attribute
    {
        public RefSafetyRulesAttribute(int version) { Version = version; }
        public int Version { get; }
    }
}
```
## QuicError.AddressNotAvailable

**NeedsWork** | [#runtime/76435](https://github.com/dotnet/runtime/issues/76435#issuecomment-1315730331) | [Video](https://www.youtube.com/watch?v=73BAn9B3ni4&t=0h16m51s)

* The two proposed inputs don't seem to both cleanly map to the same error (POSIX has ENETUNREACH which seems a better match to WSAENETUNREACH)
* There's already an existing "AddressInUse", which makes it hard for callers to understand why "NotAvailable" is different from "InUse"
* If it's (based on the EAFNOSUPPORT) address -family- not available, that should be in the name (e.g. `AddressFamilyNotAvailable`)
* It doesn't feel like there's actually a scenario where someone needs this value (or these values, if it's multiple), making it unclear why we're making essentially a copy of `SocketError`.
  * It seems like perhaps a better answer here is to expose the `SocketException` directly (probably wrapped by QuicException)

```C#
namespace System.Net.Quic;

public partial enum QuicError
{
    /// <summary>
    /// The requested address family is not available on this machine
    /// </summary>
    AddressNotAvailable,
}
```
## Add support for MissingMemberHandling to System.Text.Json

**Approved** | [#runtime/37483](https://github.com/dotnet/runtime/issues/37483#issuecomment-1315781708) | [Video](https://www.youtube.com/watch?v=73BAn9B3ni4&t=0h48m8s)

* Since the proposal involved a two-enum we asked if a boolean would be better, and the conclusion was yes.  And then it was no.
* While Newtonsoft.Json calls it MissingMemberHandling, that's actually a debatable/confusing name.  The property was specified, but it didn't map to a property/field on the target type (so reflection's MissingMember).
  * So we renamed "MissingMember" to "UnmappedMember".  Sorry, future us.

```C#
namespace System.Text.Json
{
    public enum JsonUnmappedMemberHandling { Skip, Disallow }

    public partial class JsonSerializerOptions
    {
        // Global configuration
        public JsonUnmappedMemberHandling UnmappedMemberHandling { get; set; } = JsonUnmappedMemberHandling.Skip;
    }
}

namespace System.Text.Json.Serialization
{
    // Per-type attribute-level configuration
    [AttributeUsageAttribute(AttributeTargets.Class | AttributeTargets.Interface | AttributeTargets.Struct, AllowMultiple=false, Inherited=false)]
    public class JsonUnmappedMemberHandlingAttribute : JsonAttribute
    {
        public JsonUnmappedMemberHandlingAttribute(JsonUnmappedMemberHandling unmappedMemberHandling);
        public JsonUnmappedMemberHandling UnmappedMemberHandling { get; }
    }
}

namespace System.Text.Json.Serialization.Metadata
{
    public partial class JsonTypeInfo
    {
        // Per-type configuration via contract customization.
        public JsonUnmappedMemberHandling UnmappedMemberHandling { get; set; }; // defaults to the JsonSerializerOptions value
    }
}
```
## RandomDataGenerator

**Approved** | [#runtime/73864](https://github.com/dotnet/runtime/issues/73864#issuecomment-1315849980) | [Video](https://www.youtube.com/watch?v=73BAn9B3ni4&t=1h35m36s)

* We renamed some members while I was being bad at notes.
* The group wanted to collapse it into the existing RandomNumberGenerator type
* And let's do this for System.Random, too.
  * We added array overloads for System.Random
  * We felt none of the things on System.Random needed to be virtual

```C#
namespace System.Security.Cryptography {
    public partial class RandomNumberGenerator {
        // Proposed:
        public static void GetItems<T>(ReadOnlySpan<T> choices, Span<T> destination);
        public static T[] GetItems<T>(ReadOnlySpan<T> choices, int length);
        public static string GetString(ReadOnlySpan<char> choices, int length);

        // Optional helpers:
        // Accelerator where 'choices' is 0-9A-F.
        public static void GetHexString(Span<char> destination, bool lowercase=false);
        public static string GetHexString(int stringLength, bool lowercase=false);

        public static void Shuffle<T>(Span<T> values);
    }
}

namespace System
{
    partial class Random
    {
        public void GetItems<T>(ReadOnlySpan<T> choices, Span<T> destination);
        public T[] GetItems<T>(T[] choices, int length);
        public T[] GetItems<T>(ReadOnlySpan<T> choices, int length);

        public void Shuffle<T>(Span<T> values);
        public void Shuffle<T>(T[] values);
    }
}
```

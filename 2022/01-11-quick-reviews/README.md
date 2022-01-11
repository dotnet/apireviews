# API Review 01/11/2022

## ActivityContext.TryParse should support IsRemote

**Approved** | [#runtime/42575](https://github.com/dotnet/runtime/issues/42575#issuecomment-1010229716) | [Video](https://www.youtube.com/watch?v=zBGTrbH8sAk&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Diagnostics
{
    public readonly partial struct ActivityContext
    {
        // existing
        // public static bool TryParse(string? traceParent, string? traceState, out System.Diagnostics.ActivityContext context);

        // new
        public static bool TryParse(
            string? traceParent,
            string? traceState,
            bool isRemote,
            out System.Diagnostics.ActivityContext context);
    }
}
```
## Activity.IsStopped  (functionality required to conform to OpenTelemetry specification)

**Approved** | [#runtime/63353](https://github.com/dotnet/runtime/issues/63353#issuecomment-1010232959) | [Video](https://www.youtube.com/watch?v=zBGTrbH8sAk&t=0h5m23s)

Looks good as proposed

```C#
namespace System.Diagnostics
{
    public partial class Activity
    {
        public bool IsStopped { get; }
    }
}
```
## PathAssemblyResolver System.Reflection.MetadataLoadContext should be sealed or allow overrides

**NeedsWork** | [#runtime/36753](https://github.com/dotnet/runtime/issues/36753#issuecomment-1010237884) | [Video](https://www.youtube.com/watch?v=zBGTrbH8sAk&t=0h8m51s)

For the primary proposal: we (generally) don't expose fields as public or protected.

For the alternative proposal: we feel that simply exposing the field via a property is still too little abstraction and too limiting on the type.  Instead, determine what the use cases are for the extensibility and add protected members to provide that functionality without limiting the implementation details (for example, and add method / remove method / indexer)
## Analyzer for detecting multiple Enumerations

**Approved** | [#runtime/63381](https://github.com/dotnet/runtime/issues/63381#issuecomment-1010272987) | [Video](https://www.youtube.com/watch?v=zBGTrbH8sAk&t=0h14m36s)

* The consensus seems to be that the analyzer has value
* There was one "vote" for changing the category to Reliability, but the consensus was to leave it at Performance
* There's no strong opinion on servicing it into the .NET 6 SDK or merely adding it to the next version of the NuGet package
* This analyzer needs a lot of comprehensive documentation, demonstrating various ways that `i.ElementAt(10); i.ElementAt(100);` could be re-written (`ToList()` is not always a winning move)
* The initial version should use the hard-coded list plus extensibility via editorconfig
  * "Assume iterating" vs "assume non-iterating" needs more data.

## Make digest size constants public

**Approved** | [#runtime/63130](https://github.com/dotnet/runtime/issues/63130#issuecomment-1010279732) | [Video](https://www.youtube.com/watch?v=zBGTrbH8sAk&t=0h59m23s)

Looks good as proposed

```diff
namespace System.Security.Cryptography
{
     public class HMACMD5 : HMAC
     {
-        private const int HmacSizeBits = 128;
-        private const int HmacSizeBytes = HmacSizeBits / 8;
+        public const int HashSizeInBits = 128;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public class HMACSHA1 : HMAC
     {
-        private const int HmacSizeBits = 160;
-        private const int HmacSizeBytes = HmacSizeBits / 8;
+        public const int HashSizeInBits = 160;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public class HMACSHA256 : HMAC
     {
-        private const int HmacSizeBits = 256;
-        private const int HmacSizeBytes = HmacSizeBits / 8;
+        public const int HashSizeInBits = 256;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public class HMACSHA384 : HMAC
     {
-        private const int HmacSizeBits = 384;
-        private const int HmacSizeBytes = HmacSizeBits / 8;
+        public const int HashSizeInBits = 384;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public class HMACSHA512 : HMAC
     {
-        private const int HmacSizeBits = 512;
-        private const int HmacSizeBytes = HmacSizeBits / 8;
+        public const int HashSizeInBits = 512;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public abstract class MD5 : HashAlgorithm
     {
-        private const int HashSizeBits = 128;
-        private const int HashSizeBytes = HashSizeBits / 8;
+        public const int HashSizeInBits = 128;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public abstract class SHA1 : HashAlgorithm
     {
-        private const int HashSizeBits = 160;
-        private const int HashSizeBytes = HashSizeBits / 8;
+        public const int HashSizeInBits = 160;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public abstract class SHA256 : HashAlgorithm
     {
-        private const int HashSizeBits = 256;
-        private const int HashSizeBytes = HashSizeBits / 8;
+        public const int HashSizeInBits = 256;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public abstract class SHA384 : HashAlgorithm
     {
-        private const int HashSizeBits = 384;
-        private const int HashSizeBytes = HashSizeBits / 8;
+        public const int HashSizeInBits = 384;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }

     public abstract class SHA512 : HashAlgorithm
     {
-        private const int HashSizeBits = 512;
-        private const int HashSizeBytes = HashSizeBits / 8;
+        public const int HashSizeInBits = 512;
+        public const int HashSizeInBytes = HashSizeInBits / 8;
     }
}
```

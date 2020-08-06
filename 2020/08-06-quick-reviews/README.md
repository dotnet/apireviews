# Quick Reviews 08/06/2020

## Add a way to the ActivityListeners to add more data to the Activity when it gets created

**Approved** | [#runtime/40339](https://github.com/dotnet/runtime/issues/40339#issuecomment-670076292) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=0h0m0s)

* What happens when you remove the `readonly` specifier? Does this potentially create defensive copies.
* If the only reason is lazy initialization it seems better keep it `readonly` and use `Unsafe` and initialize

```diff
 namespace System.Diagnostics
 {
     public readonly struct ActivityCreationOptions<T>
     {
+        public ActivityTagsCollection SamplingTags { get; }
+        public ActivityTraceId TraceId { get; }
     }
     public sealed class ActivityListener : IDisposable
     {
-        public bool AutoGenerateRootContextTraceId { get; set;}
         // Renames:
-        public GetRequestedData<string>? GetRequestedDataUsingParentId { get; set; }
-        public GetRequestedData<ActivityContext>? GetRequestedDataUsingContext { get; set; }
+        public SampleActivity<string>? SampleUsingParentId { get; set; }
+        public SampleActivity<ActivityContext>? Sample { get; set; }
     }
-    public delegate ActivityDataRequest GetRequestedData<T>(ref ActivityCreationOptions<T> options);
+    public delegate ActivitySamplingResult SampleActivity<T>(ref ActivityCreationOptions<T> options);
}
```

## Non-validated HttpHeaders enumeration

**Approved** | [#runtime/35126](https://github.com/dotnet/runtime/issues/35126#issuecomment-670095983) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=0h34m17s)

* `NonValidatedEnumerator` should be split into an enumerable and an enumerator
* We should compare `HeaderStringValues` to the one ASP.NET Core has
* Should `HeaderStringValues` have constructors for the single value/multiple values?

```C#
namespace System.Net.Http.Headers
{
    public partial class HttpHeaders
    {
        public HttpHeadersNonValidated NonValidated { get; }
    }

    public readonly struct HttpHeadersNonValidated : IEnumerable<KeyValuePair<string, HeaderStringValues>>
    {
        public Enumerator GetEnumerator();
        public readonly struct Enumerator : IEnumerator<KeyValuePair<string, HeaderStringValues>>
        {
            public bool MoveNext();
            public KeyValuePair<string, HeaderStringValues> Current { get; }
            public void Dispose();
            // explicitly implemented interface members
        }
    }

    public readonly struct HeaderStringValues : IEnumerable<string>
    {
        public HeaderStringValues(string value);
        public HeaderStringValues(IEnumerable<string> value);
        public Enumerator GetEnumerator();
        public readonly struct Enumerator : IEnumerator<string>
        {
            public bool MoveNext();
            public string Current { get; }
            public void Dispose();
            ... // explicitly implemented interface members
        }
    }
}
```

## Exception types for System.Net.Connections, Part 2

**Approved** | [#runtime/40451](https://github.com/dotnet/runtime/issues/40451#issuecomment-670117109) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=1h14m57s)

* Looks good as proposed.

```C#
namespace System.Net
{
    public class NetworkException : IOException
    {
        public NetworkException(string message, NetworkError error, Exception innerException = null);
        public NetworkException(NetworkError error, Exception innerException = null);
        public NetworkError NetworkError { get; }
    }
    public enum NetworkError
    {
        Unknown,

        EndPointInUse,         // SocketError.AddressAlreadyInUse
        HostNotFound,          // SocketError.HostNotFound

        ConnectionRefused,     // SocketError.ConnectionRefused
        ConnectionAborted,     // SocketError.ConnectionAborted
        ConnectionReset,       // SocketError.ConnectionReset

        OperationAborted       // SocketError.OperationAborted
    }
}
```

## Consider expanding `Vector<T>` to support `nint` and `nuint`

**Approved** | [#runtime/36160](https://github.com/dotnet/runtime/issues/36160#issuecomment-670131376) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=1h35m24s)

* We should use `NInt` and `NUInt` as the the non-keyword types names for APIs that have to refer to `nint` and `nuint`, as opposed to `IntPtr` and `UIntPtr`,

```C#
namespace System.Numerics
{
    public partial struct Vector<T>
    {
        public static explicit operator Vector<nint>(Vector<T> value);
        public static explicit operator Vector<nuint>(Vector<T> value);
    }

    public static partial class Vector
    {
        public static Vector<nint> AsVectorNInt<T>(Vector<T> value);
        public static Vector<nuint> AsVectorNUInt<T>(Vector<T> value);

        public static Vector<nint> Equals(Vector<nint> left, Vector<nint> right);
        public static Vector<nuint> Equals(Vector<nuint> left, Vector<nuint> right);

        public static Vector<nint> GreaterThan(Vector<nint> left, Vector<nint> right);
        public static Vector<nuint> GreaterThan(Vector<nuint> left, Vector<nuint> right);

        public static Vector<nint> GreaterThanOrEqual(Vector<nint> left, Vector<nint> right);
        public static Vector<nuint> GreaterThanOrEqual(Vector<nuint> left, Vector<nuint> right);

        public static Vector<nint> LessThan(Vector<nint> left, Vector<nint> right);
        public static Vector<nuint> LessThan(Vector<nuint> left, Vector<nuint> right);

        public static Vector<nint> LessThanOrEqual(Vector<nint> left, Vector<nint> right);
        public static Vector<nuint> LessThanOrEqual(Vector<nuint> left, Vector<nuint> right);
    }
}
```

## Expose a new CallConvSuppressGCTransition so SuppressGCTransition works for function pointers

**Approved** | [#runtime/38134](https://github.com/dotnet/runtime/issues/38134#issuecomment-670133996) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=1h44m19s)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices
{
    public class CallConvSuppressGCTransition
    {
    }
}
```

## Expose general purpose Crc32 APIs

**Approved** | [#runtime/2036](https://github.com/dotnet/runtime/issues/2036#issuecomment-670136776) | [Video](https://www.youtube.com/watch?v=Ok1l1fODjzo&t=1h49m58s)

* Looks good as proposed
* We can add overloads with custom polynomials later, but we should document the ones that we're using here.

```C#
namespace System.Numerics
{
    public static class BitOperations
    {
        public static uint Crc32(uint crc, byte data);
        public static uint Crc32(uint crc, ushort data);
        public static uint Crc32(uint crc, uint data);
        public static uint Crc32(uint crc, ulong data);
    }
}
```


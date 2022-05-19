# API Review 05/19/2022

## Provide `BSTR` marshaller for `LibraryImport` source generator

**Approved** | [#runtime/69021](https://github.com/dotnet/runtime/issues/69021#issuecomment-1131970556) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=0h0m0s)

Looks good as proposed

```C#
namespace System.Runtime.InteropServices.Marshalling
{
    /// <summary>
    /// Marshaller for BSTR strings
    /// </summary>
    [CLSCompliant(false)]
    [CustomTypeMarshaller(typeof(string), BufferSize = 0x100,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.TwoStageMarshalling | CustomTypeMarshallerFeatures.CallerAllocatedBuffer)]
    public unsafe ref struct BStrStringMarshaller
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="BStrStringMarshaller"/>.
        /// </summary>
        /// <param name="str">The string to marshal.</param>
        public BStrStringMarshaller(string? str);

        /// <summary>
        /// Initializes a new instance of the <see cref="BStrStringMarshaller"/>.
        /// </summary>
        /// <param name="str">The string to marshal.</param>
        /// <param name="buffer">Buffer that may be used for marshalling.</param>
        /// <remarks>
        /// The <paramref name="buffer"/> must not be movable - that is, it should not be
        /// on the managed heap or it should be pinned.
        /// <seealso cref="CustomTypeMarshallerFeatures.CallerAllocatedBuffer"/>
        /// </remarks>
        public BStrStringMarshaller(string? str, Span<ushort> buffer);

        /// <summary>
        /// Returns the native value representing the string.
        /// </summary>
        /// <remarks>
        /// <seealso cref="CustomTypeMarshallerFeatures.TwoStageMarshalling"/>
        /// </remarks>
        public void* ToNativeValue();

        /// <summary>
        /// Sets the native value representing the string.
        /// </summary>
        /// <param name="value">The native value.</param>
        /// <remarks>
        /// <seealso cref="CustomTypeMarshallerFeatures.TwoStageMarshalling"/>
        /// </remarks>
        public void FromNativeValue(void* value);

        /// <summary>
        /// Returns the managed string.
        /// </summary>
        /// <remarks>
        /// <seealso cref="CustomTypeMarshallerDirection.Out"/>
        /// </remarks>
        public string? ToManaged();

        /// <summary>
        /// Frees native resources.
        /// </summary>
        /// <remarks>
        /// <seealso cref="CustomTypeMarshallerFeatures.UnmanagedResources"/>
        /// </remarks>
        public void FreeNative();
    }
}
```
## Add back `ObsoletedInOSPlatformAttribute` for .NET 7

**Approved** | [#runtime/68557](https://github.com/dotnet/runtime/issues/68557#issuecomment-1131995684) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=0h6m9s)

Generally looks good as proposed, except
* we synchronized the AttributeTargets with what we have on `SupportedOSPlatformAttribute`.
* we made UnsupportedOSPlatformAttribute.Message be get-only with a new ctor.

```C#
namespace System.Runtime.Versioning
{
    public partial class UnsupportedOSPlatformAttribute : OSPlatformAttribute
    {
        public UnsupportedOSPlatformAttribute(string platformName, string? message);

        public string? Message { get; }
    }

    [AttributeUsage(AttributeTargets.Assembly |
                    AttributeTargets.Class |
                    AttributeTargets.Constructor |
                    AttributeTargets.Enum |
                    AttributeTargets.Event |
                    AttributeTargets.Field |
                    AttributeTargets.Interface |
                    AttributeTargets.Method |
                    AttributeTargets.Module |
                    AttributeTargets.Property |
                    AttributeTargets.Struct,
                    AllowMultiple=true, Inherited=false)]
    public sealed class ObsoletedInOSPlatformAttribute : OSPlatformAttribute
    {
        public ObsoletedInPlatformAttribute(string platformName);
        public ObsoletedInPlatformAttribute(string platformName, string? message);

        public string? Message { get; }
        public string? Url { get; set; }
    }
}
```
## Add rich type for reading X509 Authority Information Access

**Approved** | [#runtime/68792](https://github.com/dotnet/runtime/issues/68792#issuecomment-1132005378) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=0h31m0s)

Generally looks good as proposed, except we added an overload of EnumerateUris that takes a System.Security.Cryptography.Oid

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public sealed partial class X509AuthorityInformationAccessExtension : X509Extension
    {
        public X509AuthorityInformationAccessExtension() { }
        public X509AuthorityInformationAccessExtension(byte[] rawData, bool critical = false) { }
        public X509AuthorityInformationAccessExtension(ReadOnlySpan<byte> rawData, bool critical = false) { }
        public X509AuthorityInformationAccessExtension(IEnumerable<string> ocspUris, IEnumerable<string> caIssuersUris, bool critical = false) { }
        public override void CopyFrom(AsnEncodedData asnEncodedData) { }
        public IEnumerable<string> EnumerateCAIssuersUris() { throw null; }
        public IEnumerable<string> EnumerateOcspUris() { throw null; }
        public IEnumerable<string> EnumerateUris(string accessMethodOid) { throw null; }
        public IEnumerable<string> EnumerateUris(Oid accessMethodOid) { throw null; }
    }
}
```
## Add property to ZipArchiveEntry to indicate if the entry is encrypted

**Approved** | [#runtime/68897](https://github.com/dotnet/runtime/issues/68897#issuecomment-1132020748) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=0h40m44s)

Looks good as proposed.  It seems like there's some missing user story around what to do when it's `true`, but it does provide incremental value.

```C#
namespace System.IO.Compression
{
    public class ZipArchiveEntry
    {
        public bool IsEncrypted { get; }
    }
}
```
## ClientWebSocket upgrade response details

**Approved** | [#runtime/25918](https://github.com/dotnet/runtime/issues/25918#issuecomment-1132039238) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=0h58m42s)

We generally liked option (2) over option (1).

* We don't particularly feel like the nullable return value on the HttpStatusCode is needed, the code should always be populated and can use a sentinel value for "I haven't attempted connecting yet"
* The HttpStatusCode property seems like it should be using `System.Net.HttpStatusCode` instead of `int` as its return type.  If there's a layering problem, or other technical constraint, then `int` is OK.

```C#
namespace System.Net.WebSockets
{
    public partial class ClientWebSocketOptions
    {
        public bool CollectHttpResponseDetails { get; set; } = false;
    }

    public partial class ClientWebSocket
    {
        // EXISTING
        //public string? SubProtocol { get; }

        // NEW
        public System.Net.HttpStatusCode HttpStatusCode { get; }
        public IReadOnlyDictionary<string, IEnumerable<string>>? HttpResponseHeaders { get; set; } // setter to clean up when not needed anymore
    }
}
```
## .NET console logging uses a blocking collection which is not optimal for cloud-native applications

**Approved** | [#runtime/65264](https://github.com/dotnet/runtime/issues/65264#issuecomment-1132088353) | [Video](https://www.youtube.com/watch?v=XdPiAqfc3U0&t=1h14m10s)

* We changed the buffer size limit property to just be a simple queue length property
* There was a lot of discussion around DropNewest vs DropOldest vs a more complicated policy (such as Drop(Something)UnlessError), so there may be more enum values needed in the near future.
* We renamed all "Buffer" items to be "Queue"

```C#
namespace Microsoft.Extensions.Logging.Console
{
    /// <summary>
    /// Options for a <see cref="ConsoleLogger"/>.
    /// </summary>
    public partial class ConsoleLoggerOptions
    {
        /// <summary>
        /// Gets or sets the desired console logger behavior when queue becomes full.
        /// Defaults to <see cref="ConsoleLoggerQueueFullMode.Wait"/>.
        /// </summary>
        public ConsoleLoggerQueueFullMode QueueFullMode { get; set; }

        /// <summary>
        /// Gets or sets the maximum number of enqueued messages.
        /// </summary>
        public int MaxQueueLength { get; set; } = SomeDefault;
    }

    /// <summary>
    /// Determines the console logger behaviour when queue becomes full.
    /// </summary>
    public enum ConsoleLoggerQueueFullMode
    {
        /// <summary>
        /// Blocks the console thread once the queue limit is reached
        /// </summary>
        Wait,
        /// <summary>
        /// Drops new log messages when the queue is full
        /// </summary>
        DropNewest,
    }
}
```

# API Review 04/07/2026

## Add ConfigurationIgnoreAttribute to exclude properties from configuration binding

**Approved** | [#runtime/125111](https://github.com/dotnet/runtime/issues/125111#issuecomment-4200893822)

Looks good as proposed

```csharp
namespace Microsoft.Extensions.Configuration;

[AttributeUsage(AttributeTargets.Property)]
public sealed class ConfigurationIgnoreAttribute : Attribute
{
}
```

## Add `Stream` wrappers for memory and text-based types

**Approved** | [#runtime/82801](https://github.com/dotnet/runtime/issues/82801#issuecomment-4201484885)


* Instead of factory methods, we feel it's better to make StringStream.
  * We debated System.IO or System.Text for the new StringStream, and ended up with IO because that's where StringWriter is
  * StringStream shouldn't be seekable (at least not now), because it's a non-trivial operation
  * Given strong weight both for UTF-16 and for UTF-8 for a default encoding, we feel that making callers specify it is best.
    * Which we can choose to default later
  * We don't feel like these streams should ever emit BOMs.  Callers can do it in advance if they want.
* For Memory/ROMemory-based streams, we decided on separate ReadOnlyMemoryStream and WritableMemoryStream types
  * For now, they do not support `publiclyVisible`, so TryGetBuffer will return false, and GetBuffer will throw.  
* Now that everything is based on new types, ReadOnlySequence can easily follow suit.
* For ReadOnlySequenceStream, we're not concerned at the loss of generics, since only bytes make sense.

```csharp
namespace System.IO
{
    public sealed class StringStream : Stream
    {
        public StringStream(string text, Encoding encoding);
        public StringStream(ReadOnlyMemory<char> text, Encoding encoding);

        public Encoding Encoding { get; }
        public override bool CanSeek => false;
    }

    public sealed class ReadOnlyMemoryStream : MemoryStream
    {
        public ReadOnlyMemoryStream(ReadOnlyMemory<byte> source);
        public override bool CanWrite => false;
    }

    public sealed class WritableMemoryStream : MemoryStream
    {
        public WritableMemoryStream(Memory<byte> buffer);
        public override bool CanWrite => true;
    }
}

namespace System.Buffers
{
    public sealed class ReadOnlySequenceStream : Stream
    {
        public ReadOnlySequenceStream(ReadOnlySequence<byte> source);
        public override bool CanWrite => false;
        public override bool CanSeek => true;
    }
}
```
## ActivityTraceFlags.RandomTraceId flag

**Approved** | [#runtime/124509](https://github.com/dotnet/runtime/issues/124509#issuecomment-4201526152)

We think adding the "Has" prefix to RandomizedTraceId, otherwise looks good as proposed.

```csharp
namespace System.Diagnostics
{
    [Flags]
    public partial enum ActivityTraceFlags
    {
        RandomTraceId = 0b_0_0000010, // 2
    }

    public partial class Activity : IDisposable
    {
        public bool HasRandomizedTraceId { get; }
    }
}
```

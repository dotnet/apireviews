# API Review 05/31/2022

## TarReader should not assume same format for all entries in a tar file

**Approved** | [#runtime/69544](https://github.com/dotnet/runtime/issues/69544#issuecomment-1142399023) | [Video](https://www.youtube.com/watch?v=xFjPGD_GFOg&t=0h0m0s)

Looks good as proposed.

```diff

namespace System.Formats.Tar
{
    public partial class TarReader
    {
-       public TarFormat Format { get; }
    }

    public partial class TarEntry
    {
+       public TarFormat Format { get; }
    }
}
```
## Allow multiple Global Extended Attributes entries in Tar archives with PAX format

**Approved** | [#runtime/69935](https://github.com/dotnet/runtime/issues/69935#issuecomment-1142455260) | [Video](https://www.youtube.com/watch?v=xFjPGD_GFOg&t=0h6m59s)

* The new PaxGlobalExtendedAttributesTarEntry looks good as proposed
* Combined with the previous update that moved the TarFormat property from TarReader to TarEntry we made some systematic updates while we were here
  * Instead of the TarWriter doing automatic conversion of any TarEntry into the ctor-provided TarFormat, conversion must be done explicitly by the caller (new conversion constructors), and the TarWriter will just write whatever it's given.
  * TarFormat is now TarEntryFormat since it only applies to entries
  * We kept TarWriter.Format and the constructor parameter, because they control the format for the TarWriter.Write(string, string) method.
    * But we cleaned up the constructors+defaults to take this global extended attributes change into account.

```C#
namespace System.Formats.Tar
{
    public sealed partial class PaxGlobalExtendedAttributesTarEntry : System.Formats.Tar.PosixTarEntry
    {
        public PaxGlobalExtendedAttributesTarEntry(System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<string, string>> globalExtendedAttributes) { }
        public System.Collections.Generic.IReadOnlyDictionary<string, string> GlobalExtendedAttributes { get { throw null; } }
    }

    // Conversion constructors.  They move the DataStream value from other to [new].
    public partial class GnuTarEntry
    {
        public GnuTarEntry(TarEntry other);
    }
    public partial class PaxTarEntry
    {
        public PaxTarEntry(TarEntry other);
    }
    public partial class PosixTarEntry
    {
        public PosixTarEntry(TarEntry other);
    }
    public partial class UstarTarEntry
    {
        public UstarTarEntry(TarEntry other);
    }
    public partial class V7TarEntry
    {
        public V7TarEntry(TarEntry other);
    }
}
```

```diff
namespace System.Formats.Tar
{
    public sealed partial class TarReader : System.IDisposable
    {
-        public System.Collections.Generic.IReadOnlyDictionary<string, string>? GlobalExtendedAttributes { get { throw null; } }
    }

    public sealed partial class TarWriter : System.IDisposable
    {
-        public TarWriter(System.IO.Stream archiveStream, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<string, string>>? globalExtendedAttributes = null, bool leaveOpen = false) { }
+        public TarWriter(System.IO.Stream archiveStream) {}

-        public TarWriter(Stream archiveStream, TarEntryFormat archiveFormat, bool leaveOpen = false);
+        public TarWriter(Stream archiveStream, TarEntryFormat archiveFormat = TarEntryFormat.Pax, bool leaveOpen = false);
    }

-   public partial enum TarFormat { ... }
+   public partial enum TarEntryFormat { ... }
}
```
## Support for Span<T> and ReadOnlySpan<T> in source-generated marshalling

**Approved** | [#runtime/69281](https://github.com/dotnet/runtime/issues/69281#issuecomment-1142534492) | [Video](https://www.youtube.com/watch?v=xFjPGD_GFOg&t=1h4m11s)

* We removed the NeverNull variants from the approved shape.  The name we were leaning toward was NullAsEmpty instead of NeverNull, but we felt that if the default marshaller matches MemoryMarshal.GetReference instead of Span.GetPinnableReference that we won't need it.
* I added some `readonly` modifiers to where the `ref`s were in the ReadOnlySpan marshaller.  They might be wrong.  Make them be right :smile:.

```C#

namespace System.Runtime.InteropServices.Marshalling
{
    [CustomTypeMarshaller(typeof(ReadOnlySpan<>), CustomTypeMarshallerKind.LinearCollection, Direction = CustomTypeMarshallerDirection.In, Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling, BufferSize = 0x200)]
    public unsafe ref struct ReadOnlySpanMarshaller<T>
    {
        public ReadOnlySpanMarshaller(int sizeOfNativeElement);

        public ReadOnlySpanMarshaller(ReadOnlySpan<T> managed, int sizeOfNativeElement);

        public ReadOnlySpanMarshaller(ReadOnlySpan<T> managed, Span<byte> stackSpace, int sizeOfNativeElement);

        public ReadOnlySpan<T> GetManagedValuesSource();
        public Span<byte> GetNativeValuesDestination();
        public ref readonly byte GetPinnableReference();
        public byte* ToNativeValue();
        public void FreeNative();
        public static ref readonly T GetPinnableReference(ReadOnlySpan<T> managed);
    }

    [CustomTypeMarshaller(typeof(Span<>), CustomTypeMarshallerKind.LinearCollection, Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling, BufferSize = 0x200)]
    public unsafe ref struct SpanMarshaller<T>
    {
        public SpanMarshaller(int sizeOfNativeElement);

        public SpanMarshaller(Span<T> managed, int sizeOfNativeElement);

        public SpanMarshaller(Span<T> managed, Span<byte> stackSpace, int sizeOfNativeElement);


        public ReadOnlySpan<T> GetManagedValuesSource();
        public Span<T> GetManagedValuesDestination(int length);
        public Span<byte> GetNativeValuesDestination();
        public ReadOnlySpan<byte> GetNativeValuesSource(int length);
        public ref byte GetPinnableReference();
        public byte* ToNativeValue();
        public void FromNativeValue(byte* value);
        public static ref T GetPinnableReference(Span<T> managed);

        public Span<T> ToManaged();

        public void FreeNative();
    }
}
```
## Add AllBitsSet to the IBinaryNumber Interface

**Approved** | [#runtime/69676](https://github.com/dotnet/runtime/issues/69676#issuecomment-1142542210) | [Video](https://www.youtube.com/watch?v=xFjPGD_GFOg&t=1h55m9s)

* Looks good as proposed
* Consider DIMming it as ~Zero

```C#
namespace System.Numerics
{
    public partial interface IBinaryNumber<TSelf>
    {
        static abstract TSelf AllBitsSet { get; }
    }
}
```

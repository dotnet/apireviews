# API Review 04/08/2022

## Let consumers of MemoryCache access metrics

**Approved** | [#runtime/50406](https://github.com/dotnet/runtime/issues/50406) | [Video](https://www.youtube.com/watch?v=vIynN1PU3GA&t=0h0m0s)

## Implementations of marshallers using `CustomTypeMarshallerAttribute`

**Approved** | [#runtime/66623](https://github.com/dotnet/runtime/issues/66623#issuecomment-1093113977) | [Video](https://www.youtube.com/watch?v=vIynN1PU3GA&t=0h0m17s)

* We've generally stopped using "Ptr" and instead use "Pointer", so PtrArrayMarshaller got renamed to PointerArrayMarshaller
* Consider if it's valuable to put these (and future) marshallers in a sub-namespace, e.g. `System.Runtime.InteropServices.Marshallers`
  * If it seems valueable to also move the new LibraryImport attribute, something slightly more general, like `System.Runtime.Marshalling` for "the new marshalling" namespace

```C#
namespace System.Runtime.InteropServices
{
    [CLSCompliant(false)]
    [CustomTypeMarshaller(typeof(string), BufferSize = 0x100,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling )]
    public unsafe ref struct AnsiStringMarshaller
    {
        public AnsiStringMarshaller(string? str);
        public AnsiStringMarshaller(string? str, Span<byte> buffer);

        public byte* ToNativeValue();
        public void FromNativeValue(byte* value);

        public string? ToManaged();

        public void FreeNative();
    }

    [CLSCompliant(false)]
    [CustomTypeMarshaller(typeof(string), BufferSize = 0x100,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling )]
    public unsafe ref struct Utf8StringMarshaller
    {
        public Utf8StringMarshaller(string? str);
        public Utf8StringMarshaller(string? str, Span<byte> buffer);

        public byte* ToNativeValue();
        public void FromNativeValue(byte* value);

        public string? ToManaged();

        public void FreeNative();
    }

    [CLSCompliant(false)]
    [CustomTypeMarshaller(typeof(string), BufferSize = 0x100,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling)]
    public unsafe ref struct Utf16StringMarshaller
    {
        public Utf16StringMarshaller(string? str);
        public Utf16StringMarshaller(string? str, Span<ushort> buffer);

        public ref ushort GetPinnableReference();

        public ushort* ToNativeValue();
        public void FromNativeValue(ushort* value);

        public string? ToManaged();

        public void FreeNative();
    }

    [CustomTypeMarshaller(typeof(CustomTypeMarshallerAttribute.GenericPlaceholder[]),
        CustomTypeMarshallerKind.LinearCollection, BufferSize = 0x200,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling)]
    public unsafe ref struct ArrayMarshaller<T>
    {
        public ArrayMarshaller(int sizeOfNativeElement);
        public ArrayMarshaller(T[]? array, int sizeOfNativeElement);
        public ArrayMarshaller(T[]? array, Span<byte> buffer, int sizeOfNativeElement);

        public ReadOnlySpan<T> GetManagedValuesSource();

        public Span<T> GetManagedValuesDestination(int length);
        public Span<byte> GetNativeValuesDestination();

        public ReadOnlySpan<byte> GetNativeValuesSource(int length);

        public ref byte GetPinnableReference();

        public byte* ToNativeValue();
        public void FromNativeValue(byte* value);

        public T[]? ToManaged();

        public void FreeNative();
    }

    [CustomTypeMarshaller(typeof(CustomTypeMarshallerAttribute.GenericPlaceholder*[]),
        CustomTypeMarshallerKind.LinearCollection, BufferSize = 0x200,
        Features = CustomTypeMarshallerFeatures.UnmanagedResources | CustomTypeMarshallerFeatures.CallerAllocatedBuffer | CustomTypeMarshallerFeatures.TwoStageMarshalling)]
    public unsafe ref struct PointerArrayMarshaller<T> where T : unmanaged
    {
        public PointerArrayMarshaller(int sizeOfNativeElement);
        public PointerArrayMarshaller(T*[]? array, int sizeOfNativeElement);
        public PointerArrayMarshaller(T*[]? array, Span<byte> buffer, int sizeOfNativeElement);

        public ReadOnlySpan<IntPtr> GetManagedValuesSource();

        public Span<IntPtr> GetManagedValuesDestination(int length);
        public Span<byte> GetNativeValuesDestination();

        public ReadOnlySpan<byte> GetNativeValuesSource(int length);
        
        public ref byte GetPinnableReference();

        public byte* ToNativeValue();
        public void FromNativeValue(byte* value);

        public T*[]? ToManaged();

        public void FreeNative();
    }
}
```
## System.Diagnostics.Activity: Enumeration API [Iteration 2]

**Approved** | [#runtime/67207](https://github.com/dotnet/runtime/issues/67207#issuecomment-1093174042) | [Video](https://www.youtube.com/watch?v=vIynN1PU3GA&t=0h22m57s)

Rather than expose an abstract class to describe the concept, we feel that it's solvable with just one struct iterator type defined on Activity

```C#
namespace System.Diagnostics
{
    partial class Activity
    {
        public Enumerator<KeyValuePair<string,object>> EnumerateTagObjects();
        public Enumerator<ActivityLink> EnumerateLinks();
        public Enumerator<ActivityEvent> EnumerateEvents();
    
        public struct Enumerator<T>
        {
            private Enumerator(/* The node */)  { }
            public readonly Enumerator<T> GetEnumerator() => this;

            public readonly ref T Current => ref _node.Value;
            public bool MoveNext() { ... }
        }
    }
}
```

## Enhance RequiresDynamicCode Attribute to apply to class

**Approved** | [#runtime/67368](https://github.com/dotnet/runtime/issues/67368#issuecomment-1093201671) | [Video](https://www.youtube.com/watch?v=vIynN1PU3GA&t=1h23m3s)

Looks good as proposed

```diff
 namespace System.Diagnostics.CodeAnalysis
 {
     [AttributeUsage(
         AttributeTargets.Method |
         AttributeTargets.Constructor |
+        AttributeTargets.Class,
         Inherited = false)]
     public partial class RequiresDynamicCodeAttribute: Attribute
     {
     }
 }
```
## Add Regex.Enumerate(ReadOnlySpan<char>) which is allocation free

**Approved** | [#runtime/65011](https://github.com/dotnet/runtime/issues/65011#issuecomment-1093258052) | [Video](https://www.youtube.com/watch?v=vIynN1PU3GA&t=1h34m54s)

We broke Range up into Index and Length to better match the current Match type.  We can easily add a Range property later if it's desired.

The `ValueMatch` type is being approved as a ref struct because we have concerns that it will need it with a more complete implementation of ValueMatch.  In either this release, or a subsequent one, when we're confident we can drop it we would like to do so.

```C#
namespace System.Text.RegularExpressions
{
    // NOTE: This is being approved as a ref struct because we have concerns that it will need it with
    // a more complete implementation of ValueMatch.
    // If the ref-ness can safely be removed, we'd rather this was a simple (non-ref) struct.
    public readonly ref struct ValueMatch
    {
        public int Index { get; }
        public int Length { get; }
    }

    public class Regex : ISerializable
    {
        public ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input) { throw null; }
        public static ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex)] string pattern) { throw null; }
        public static ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options) { throw null; }
        public static ValueMatchEnumerator EnumerateMatches(ReadOnlySpan<char> input, [StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options, TimeSpan matchTimeout) { throw null; }
        public ref struct ValueMatchEnumerator
        {
            public readonly ValueMatchEnumerator GetEnumerator() { throw null; }
            public bool MoveNext() { throw null; }
            public readonly ValueMatch Current { get { throw null; } }
        }
    }
}
```

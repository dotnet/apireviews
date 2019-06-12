# System.Memory

```C#
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Memory\before\System.Memory.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Memory\after\System.Memory.dll
 namespace System {
  public static class MemoryExtensions {
    public static ReadOnlyMemory<char> AsMemory(this string? text);
    public static ReadOnlyMemory<char> AsMemory(this string? text, Index startIndex);
    public static ReadOnlyMemory<char> AsMemory(this string? text, int start);
    public static ReadOnlyMemory<char> AsMemory(this string? text, int start, int length);
    public static ReadOnlyMemory<char> AsMemory(this string? text, Range range);
    public static Memory<T> AsMemory<T>(this ArraySegment<T> segment);
    public static Memory<T> AsMemory<T>(this ArraySegment<T> segment, int start);
    public static Memory<T> AsMemory<T>(this ArraySegment<T> segment, int start, int length);
    public static Memory<T> AsMemory<T>(this T[]? array);
    public static Memory<T> AsMemory<T>(this T[]? array, Index startIndex);
    public static Memory<T> AsMemory<T>(this T[]? array, int start);
    public static Memory<T> AsMemory<T>(this T[]? array, int start, int length);
    public static Memory<T> AsMemory<T>(this T[]? array, Range range);
    public static ReadOnlySpan<char> AsSpan(this string? text);
    public static ReadOnlySpan<char> AsSpan(this string? text, int start);
    public static ReadOnlySpan<char> AsSpan(this string? text, int start, int length);
    public static Span<T> AsSpan<T>(this ArraySegment<T> segment);
    public static Span<T> AsSpan<T>(this ArraySegment<T> segment, Index startIndex);
    public static Span<T> AsSpan<T>(this ArraySegment<T> segment, int start);
    public static Span<T> AsSpan<T>(this ArraySegment<T> segment, int start, int length);
    public static Span<T> AsSpan<T>(this ArraySegment<T> segment, Range range);
    public static Span<T> AsSpan<T>(this T[]? array);
    public static Span<T> AsSpan<T>(this T[]? array, Index startIndex);
    public static Span<T> AsSpan<T>(this T[]? array, int start);
    public static Span<T> AsSpan<T>(this T[]? array, int start, int length);
    public static Span<T> AsSpan<T>(this T[]? array, Range range);
    public static int BinarySearch<T, TComparable>(this ReadOnlySpan<T> span, TComparable comparable) where TComparable : IComparable<T>;
    public static int BinarySearch<T, TComparable>(this Span<T> span, TComparable comparable) where TComparable : IComparable<T>;
    public static int BinarySearch<T, TComparer>(this ReadOnlySpan<T> span, T value, TComparer comparer) where TComparer : IComparer<T>;
    public static int BinarySearch<T, TComparer>(this Span<T> span, T value, TComparer comparer) where TComparer : IComparer<T>;
    public static int BinarySearch<T>(this ReadOnlySpan<T> span, IComparable<T> comparable);
    public static int BinarySearch<T>(this Span<T> span, IComparable<T> comparable);
    public static int CompareTo(this ReadOnlySpan<char> span, ReadOnlySpan<char> other, StringComparison comparisonType);
    public static bool Contains(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    public static bool Contains<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
    public static bool Contains<T>(this Span<T> span, T value) where T : IEquatable<T>;
    public static void CopyTo<T>(this T[]? source, Memory<T> destination);
    public static void CopyTo<T>(this T[]? source, Span<T> destination);
    public static bool EndsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    public static bool EndsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static bool EndsWith<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static SpanRuneEnumerator EnumerateRunes(this ReadOnlySpan<char> span);
    public static SpanRuneEnumerator EnumerateRunes(this Span<char> span);
    public static bool Equals(this ReadOnlySpan<char> span, ReadOnlySpan<char> other, StringComparison comparisonType);
    public static int IndexOf(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    public static int IndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static int IndexOf<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
    public static int IndexOf<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static int IndexOf<T>(this Span<T> span, T value) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
    public static int IndexOfAny<T>(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    public static bool IsWhiteSpace(this ReadOnlySpan<char> span);
    public static int LastIndexOf(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    public static int LastIndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static int LastIndexOf<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
    public static int LastIndexOf<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static int LastIndexOf<T>(this Span<T> span, T value) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
    public static int LastIndexOfAny<T>(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
    public static bool Overlaps<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other);
    public static bool Overlaps<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, out int elementOffset);
    public static bool Overlaps<T>(this Span<T> span, ReadOnlySpan<T> other);
    public static bool Overlaps<T>(this Span<T> span, ReadOnlySpan<T> other, out int elementOffset);
    public static void Reverse<T>(this Span<T> span);
    public static int SequenceCompareTo<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T : IComparable<T>;
    public static int SequenceCompareTo<T>(this Span<T> span, ReadOnlySpan<T> other) where T : IComparable<T>;
    public static bool SequenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T : IEquatable<T>;
    public static bool SequenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other) where T : IEquatable<T>;
    public static bool StartsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
    public static bool StartsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static bool StartsWith<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
    public static int ToLower(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
    public static int ToLowerInvariant(this ReadOnlySpan<char> source, Span<char> destination);
    public static int ToUpper(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
    public static int ToUpperInvariant(this ReadOnlySpan<char> source, Span<char> destination);
    public static Memory<char> Trim(this Memory<char> memory);
    public static ReadOnlyMemory<char> Trim(this ReadOnlyMemory<char> memory);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static Span<char> Trim(this Span<char> span);
    public static Memory<T> Trim<T>(this Memory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Memory<T> Trim<T>(this Memory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> Trim<T>(this ReadOnlyMemory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> Trim<T>(this ReadOnlyMemory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlySpan<T> Trim<T>(this ReadOnlySpan<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlySpan<T> Trim<T>(this ReadOnlySpan<T> memoryspan, T trimElement) where T : IEquatable<T>;
    public static Span<T> Trim<T>(this Span<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Span<T> Trim<T>(this Span<T> memoryspan, T trimElement) where T : IEquatable<T>;
    public static Memory<char> TrimEnd(this Memory<char> memory);
    public static ReadOnlyMemory<char> TrimEnd(this ReadOnlyMemory<char> memory);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static Span<char> TrimEnd(this Span<char> span);
    public static Memory<T> TrimEnd<T>(this Memory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Memory<T> TrimEnd<T>(this Memory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> TrimEnd<T>(this ReadOnlyMemory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> TrimEnd<T>(this ReadOnlyMemory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlySpan<T> TrimEnd<T>(this ReadOnlySpan<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlySpan<T> TrimEnd<T>(this ReadOnlySpan<T> memoryspan, T trimElement) where T : IEquatable<T>;
    public static Span<T> TrimEnd<T>(this Span<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Span<T> TrimEnd<T>(this Span<T> memoryspan, T trimElement) where T : IEquatable<T>;
    public static Memory<char> TrimStart(this Memory<char> memory);
    public static ReadOnlyMemory<char> TrimStart(this ReadOnlyMemory<char> memory);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, char trimChar);
    public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
    public static Span<char> TrimStart(this Span<char> span);
    public static Memory<T> TrimStart<T>(this Memory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Memory<T> TrimStart<T>(this Memory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> TrimStart<T>(this ReadOnlyMemory<T> memory, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlyMemory<T> TrimStart<T>(this ReadOnlyMemory<T> memory, T trimElement) where T : IEquatable<T>;
    public static ReadOnlySpan<T> TrimStart<T>(this ReadOnlySpan<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static ReadOnlySpan<T> TrimStart<T>(this ReadOnlySpan<T> memoryspan, T trimElement) where T : IEquatable<T>;
    public static Span<T> TrimStart<T>(this Span<T> memoryspan, ReadOnlySpan<T> trimElements) where T : IEquatable<T>;
    public static Span<T> TrimStart<T>(this Span<T> memoryspan, T trimElement) where T : IEquatable<T>;
  }
  public readonly struct SequencePosition : IEquatable<SequencePosition> {
    public SequencePosition(object? @object, int integer);
    public override bool Equals(object? obj);
    public bool Equals(SequencePosition other);
    public override int GetHashCode();
    public int GetInteger();
    public object? GetObject();
  }
 }
 namespace System.Buffers {
  public sealed class ArrayBufferWriter<T> : IBufferWriter<T> {
    public ArrayBufferWriter();
    public ArrayBufferWriter(int initialCapacity);
    public int Capacity { get; }
    public int FreeCapacity { get; }
    public int WrittenCount { get; }
    public ReadOnlyMemory<T> WrittenMemory { get; }
    public ReadOnlySpan<T> WrittenSpan { get; }
    public void Advance(int count);
    public void Clear();
    public Memory<T> GetMemory(int sizeHint = 0);
    public Span<T> GetSpan(int sizeHint = 0);
  }
  public static class BuffersExtensions {
    public static void CopyTo<T>(this in ReadOnlySequence<T> source, Span<T> destination);
    public static SequencePosition? PositionOf<T>(this in ReadOnlySequence<T> source, T value) where T : IEquatable<T>;
    public static T[] ToArray<T>(this in ReadOnlySequence<T> sequence);
    public static void Write<T>(this IBufferWriter<T> writer, ReadOnlySpan<T> value);
  }
  public interface IBufferWriter<T> {
    void Advance(int count);
    Memory<T> GetMemory(int sizeHint = 0);
    Span<T> GetSpan(int sizeHint = 0);
  }
  public abstract class MemoryPool<T> : IDisposable {
    protected MemoryPool();
    public abstract int MaxBufferSize { get; }
    public static MemoryPool<T> Shared { get; }
    public void Dispose();
    protected abstract void Dispose(bool disposing);
    public abstract IMemoryOwner<T> Rent(int minBufferSize = -1);
  }
  public readonly struct ReadOnlySequence<T> {
    public static readonly ReadOnlySequence<T> Empty;
    public ReadOnlySequence(ReadOnlySequenceSegment<T> startSegment, int startIndex, ReadOnlySequenceSegment<T> endSegment, int endIndex);
    public ReadOnlySequence(ReadOnlyMemory<T> memory);
    public ReadOnlySequence(T[] array);
    public ReadOnlySequence(T[] array, int start, int length);
    public SequencePosition End { get; }
    public ReadOnlyMemory<T> First { get; }
    public ReadOnlySpan<T> FirstSpan { get; }
    public bool IsEmpty { get; }
    public bool IsSingleSegment { get; }
    public long Length { get; }
    public SequencePosition Start { get; }
    public ReadOnlySequence<T>.Enumerator GetEnumerator();
    public SequencePosition GetPosition(long offset);
    public SequencePosition GetPosition(long offset, SequencePosition origin);
    public ReadOnlySequence<T> Slice(int start, int length);
    public ReadOnlySequence<T> Slice(int start, SequencePosition end);
    public ReadOnlySequence<T> Slice(long start);
    public ReadOnlySequence<T> Slice(long start, long length);
    public ReadOnlySequence<T> Slice(long start, SequencePosition end);
    public ReadOnlySequence<T> Slice(SequencePosition start);
    public ReadOnlySequence<T> Slice(SequencePosition start, int length);
    public ReadOnlySequence<T> Slice(SequencePosition start, long length);
    public ReadOnlySequence<T> Slice(SequencePosition start, SequencePosition end);
    public override string ToString();
    public bool TryGet(ref SequencePosition position, out ReadOnlyMemory<T> memory, bool advance = true);
    public struct Enumerator {
      public Enumerator(in ReadOnlySequence<T> sequence);
      public ReadOnlyMemory<T> Current { get; }
      public bool MoveNext();
    }
  }
  public abstract class ReadOnlySequenceSegment<T> {
    protected ReadOnlySequenceSegment();
    public ReadOnlyMemory<T> Memory { get; protected set; }
    public ReadOnlySequenceSegment<T>? Next { get; protected set; }
    public long RunningIndex { get; protected set; }
  }
  public ref struct SequenceReader<T> where T : struct {
    public SequenceReader(ReadOnlySequence<T> sequence);
    public long Consumed { get; }
    public ReadOnlySpan<T> CurrentSpan { get; }
    public int CurrentSpanIndex { get; }
    public bool End { get; }
    public long Length { get; }
    public SequencePosition Position { get; }
    public long Remaining { get; }
    public ReadOnlySequence<T> Sequence { get; }
    public ReadOnlySpan<T> UnreadSpan { get; }
    public void Advance(long count);
    public long AdvancePast(T value);
    public long AdvancePastAny(ReadOnlySpan<T> values);
    public long AdvancePastAny(T value0, T value1);
    public long AdvancePastAny(T value0, T value1, T value2);
    public long AdvancePastAny(T value0, T value1, T value2, T value3);
    public bool IsNext(ReadOnlySpan<T> next, bool advancePast = false);
    public bool IsNext(T next, bool advancePast = false);
    public void Rewind(long count);
    public bool TryAdvanceTo(T delimiter, bool advancePastDelimiter = true);
    public bool TryAdvanceToAny(ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);
    public bool TryCopyTo(Span<T> destination);
    public bool TryPeek(out T value);
    public bool TryRead(out T value);
    public bool TryReadTo(out ReadOnlySequence<T> sequence, ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
    public bool TryReadTo(out ReadOnlySequence<T> sequence, T delimiter, bool advancePastDelimiter = true);
    public bool TryReadTo(out ReadOnlySequence<T> sequence, T delimiter, T delimiterEscape, bool advancePastDelimiter = true);
    public bool TryReadTo(out ReadOnlySpan<T> span, T delimiter, bool advancePastDelimiter = true);
    public bool TryReadTo(out ReadOnlySpan<T> span, T delimiter, T delimiterEscape, bool advancePastDelimiter = true);
    public bool TryReadToAny(out ReadOnlySequence<T> sequence, ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);
    public bool TryReadToAny(out ReadOnlySpan<T> span, ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);
  }
  public static class SequenceReaderExtensions {
    public static bool TryReadBigEndian(this ref SequenceReader<byte> reader, out short value);
    public static bool TryReadBigEndian(this ref SequenceReader<byte> reader, out int value);
    public static bool TryReadBigEndian(this ref SequenceReader<byte> reader, out long value);
    public static bool TryReadLittleEndian(this ref SequenceReader<byte> reader, out short value);
    public static bool TryReadLittleEndian(this ref SequenceReader<byte> reader, out int value);
    public static bool TryReadLittleEndian(this ref SequenceReader<byte> reader, out long value);
  }
  public readonly struct StandardFormat : IEquatable<StandardFormat> {
    public const byte MaxPrecision = (byte)99;
    public const byte NoPrecision = (byte)255;
    public StandardFormat(char symbol, byte precision = (byte)255);
    public bool HasPrecision { get; }
    public bool IsDefault { get; }
    public byte Precision { get; }
    public char Symbol { get; }
    public bool Equals(StandardFormat other);
    public override bool Equals(object? obj);
    public override int GetHashCode();
    public static bool operator ==(StandardFormat left, StandardFormat right);
    public static implicit operator StandardFormat (char symbol);
    public static bool operator !=(StandardFormat left, StandardFormat right);
    public static StandardFormat Parse(ReadOnlySpan<char> format);
    public static StandardFormat Parse(string? format);
    public override string ToString();
    public static bool TryParse(ReadOnlySpan<char> format, out StandardFormat result);
  }
 }
 namespace System.Buffers.Binary {
  public static class BinaryPrimitives {
    public static short ReadInt16BigEndian(ReadOnlySpan<byte> source);
    public static short ReadInt16LittleEndian(ReadOnlySpan<byte> source);
    public static int ReadInt32BigEndian(ReadOnlySpan<byte> source);
    public static int ReadInt32LittleEndian(ReadOnlySpan<byte> source);
    public static long ReadInt64BigEndian(ReadOnlySpan<byte> source);
    public static long ReadInt64LittleEndian(ReadOnlySpan<byte> source);
    public static ushort ReadUInt16BigEndian(ReadOnlySpan<byte> source);
    public static ushort ReadUInt16LittleEndian(ReadOnlySpan<byte> source);
    public static uint ReadUInt32BigEndian(ReadOnlySpan<byte> source);
    public static uint ReadUInt32LittleEndian(ReadOnlySpan<byte> source);
    public static ulong ReadUInt64BigEndian(ReadOnlySpan<byte> source);
    public static ulong ReadUInt64LittleEndian(ReadOnlySpan<byte> source);
    public static byte ReverseEndianness(byte value);
    public static short ReverseEndianness(short value);
    public static int ReverseEndianness(int value);
    public static long ReverseEndianness(long value);
    public static sbyte ReverseEndianness(sbyte value);
    public static ushort ReverseEndianness(ushort value);
    public static uint ReverseEndianness(uint value);
    public static ulong ReverseEndianness(ulong value);
    public static bool TryReadInt16BigEndian(ReadOnlySpan<byte> source, out short value);
    public static bool TryReadInt16LittleEndian(ReadOnlySpan<byte> source, out short value);
    public static bool TryReadInt32BigEndian(ReadOnlySpan<byte> source, out int value);
    public static bool TryReadInt32LittleEndian(ReadOnlySpan<byte> source, out int value);
    public static bool TryReadInt64BigEndian(ReadOnlySpan<byte> source, out long value);
    public static bool TryReadInt64LittleEndian(ReadOnlySpan<byte> source, out long value);
    public static bool TryReadUInt16BigEndian(ReadOnlySpan<byte> source, out ushort value);
    public static bool TryReadUInt16LittleEndian(ReadOnlySpan<byte> source, out ushort value);
    public static bool TryReadUInt32BigEndian(ReadOnlySpan<byte> source, out uint value);
    public static bool TryReadUInt32LittleEndian(ReadOnlySpan<byte> source, out uint value);
    public static bool TryReadUInt64BigEndian(ReadOnlySpan<byte> source, out ulong value);
    public static bool TryReadUInt64LittleEndian(ReadOnlySpan<byte> source, out ulong value);
    public static bool TryWriteInt16BigEndian(Span<byte> destination, short value);
    public static bool TryWriteInt16LittleEndian(Span<byte> destination, short value);
    public static bool TryWriteInt32BigEndian(Span<byte> destination, int value);
    public static bool TryWriteInt32LittleEndian(Span<byte> destination, int value);
    public static bool TryWriteInt64BigEndian(Span<byte> destination, long value);
    public static bool TryWriteInt64LittleEndian(Span<byte> destination, long value);
    public static bool TryWriteUInt16BigEndian(Span<byte> destination, ushort value);
    public static bool TryWriteUInt16LittleEndian(Span<byte> destination, ushort value);
    public static bool TryWriteUInt32BigEndian(Span<byte> destination, uint value);
    public static bool TryWriteUInt32LittleEndian(Span<byte> destination, uint value);
    public static bool TryWriteUInt64BigEndian(Span<byte> destination, ulong value);
    public static bool TryWriteUInt64LittleEndian(Span<byte> destination, ulong value);
    public static void WriteInt16BigEndian(Span<byte> destination, short value);
    public static void WriteInt16LittleEndian(Span<byte> destination, short value);
    public static void WriteInt32BigEndian(Span<byte> destination, int value);
    public static void WriteInt32LittleEndian(Span<byte> destination, int value);
    public static void WriteInt64BigEndian(Span<byte> destination, long value);
    public static void WriteInt64LittleEndian(Span<byte> destination, long value);
    public static void WriteUInt16BigEndian(Span<byte> destination, ushort value);
    public static void WriteUInt16LittleEndian(Span<byte> destination, ushort value);
    public static void WriteUInt32BigEndian(Span<byte> destination, uint value);
    public static void WriteUInt32LittleEndian(Span<byte> destination, uint value);
    public static void WriteUInt64BigEndian(Span<byte> destination, ulong value);
    public static void WriteUInt64LittleEndian(Span<byte> destination, ulong value);
  }
 }
 namespace System.Buffers.Text {
  public static class Base64 {
    public static OperationStatus DecodeFromUtf8(ReadOnlySpan<byte> utf8, Span<byte> bytes, out int bytesConsumed, out int bytesWritten, bool isFinalBlock = true);
    public static OperationStatus DecodeFromUtf8InPlace(Span<byte> buffer, out int bytesWritten);
    public static OperationStatus EncodeToUtf8(ReadOnlySpan<byte> bytes, Span<byte> utf8, out int bytesConsumed, out int bytesWritten, bool isFinalBlock = true);
    public static OperationStatus EncodeToUtf8InPlace(Span<byte> buffer, int dataLength, out int bytesWritten);
    public static int GetMaxDecodedFromUtf8Length(int length);
    public static int GetMaxEncodedToUtf8Length(int length);
  }
  public static class Utf8Formatter {
    public static bool TryFormat(bool value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(byte value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(DateTime value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(DateTimeOffset value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(decimal value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(double value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(Guid value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(short value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(int value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(long value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(sbyte value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(float value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(TimeSpan value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(ushort value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(uint value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
    public static bool TryFormat(ulong value, Span<byte> destination, out int bytesWritten, StandardFormat format = default(StandardFormat));
  }
  public static class Utf8Parser {
    public static bool TryParse(ReadOnlySpan<byte> source, out bool value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out byte value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out DateTime value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out DateTimeOffset value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out decimal value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out double value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out Guid value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out short value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out int value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out long value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out sbyte value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out float value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out TimeSpan value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out ushort value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out uint value, out int bytesConsumed, char standardFormat = '\0');
    public static bool TryParse(ReadOnlySpan<byte> source, out ulong value, out int bytesConsumed, char standardFormat = '\0');
  }
 }
 namespace System.Runtime.InteropServices {
  public static class MemoryMarshal {
    public static ReadOnlySpan<byte> AsBytes<T>(ReadOnlySpan<T> span) where T : struct;
    public static Span<byte> AsBytes<T>(Span<T> span) where T : struct;
    public static Memory<T> AsMemory<T>(ReadOnlyMemory<T> memory);
    public static ref readonly T AsRef<T>(ReadOnlySpan<byte> span) where T : struct;
    public static ref T AsRef<T>(Span<byte> span) where T : struct;
    public static ReadOnlySpan<TTo> Cast<TFrom, TTo>(ReadOnlySpan<TFrom> span) where TFrom : struct where TTo : struct;
    public static Span<TTo> Cast<TFrom, TTo>(Span<TFrom> span) where TFrom : struct where TTo : struct;
    public static Memory<T> CreateFromPinnedArray<T>(T[]? array, int start, int length);
    public static ReadOnlySpan<T> CreateReadOnlySpan<T>(ref T reference, int length);
    public static Span<T> CreateSpan<T>(ref T reference, int length);
    public static ref T GetReference<T>(ReadOnlySpan<T> span);
    public static ref T GetReference<T>(Span<T> span);
    public static T Read<T>(ReadOnlySpan<byte> source) where T : struct;
    public static IEnumerable<T> ToEnumerable<T>(ReadOnlyMemory<T> memory);
    public static bool TryGetArray<T>(ReadOnlyMemory<T> memory, out ArraySegment<T> segment);
    public static bool TryGetMemoryManager<T, TManager>(ReadOnlyMemory<T> memory, out TManager? manager) where TManager : MemoryManager<T>;
    public static bool TryGetMemoryManager<T, TManager>(ReadOnlyMemory<T> memory, out TManager? manager, out int start, out int length) where TManager : MemoryManager<T>;
    public static bool TryGetString(ReadOnlyMemory<char> memory, out string? text, out int start, out int length);
    public static bool TryRead<T>(ReadOnlySpan<byte> source, out T value) where T : struct;
    public static bool TryWrite<T>(Span<byte> destination, ref T value) where T : struct;
    public static void Write<T>(Span<byte> destination, ref T value) where T : struct;
  }
  public static class SequenceMarshal {
    public static bool TryGetArray<T>(ReadOnlySequence<T> sequence, out ArraySegment<T> segment);
    public static bool TryGetReadOnlyMemory<T>(ReadOnlySequence<T> sequence, out ReadOnlyMemory<T> memory);
    public static bool TryGetReadOnlySequenceSegment<T>(ReadOnlySequence<T> sequence, out ReadOnlySequenceSegment<T> startSegment, out int startIndex, out ReadOnlySequenceSegment<T> endSegment, out int endIndex);
    public static bool TryRead<T>(ref SequenceReader<byte> reader, out T value) where T : struct;
  }
 }
 namespace System.Text {
  public ref struct SpanRuneEnumerator {
    public Rune Current { get; }
    public SpanRuneEnumerator GetEnumerator();
    public bool MoveNext();
  }
 }
```
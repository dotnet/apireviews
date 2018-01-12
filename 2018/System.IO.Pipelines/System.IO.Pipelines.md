# System.IO.Pipelines.dll

**Note:** APIs not reviewed were removed from this API listing.

```csharp
namespace System.Buffers {
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ReadOnlyBuffer : ISequence<ReadOnlyMemory<byte>> {
        public ReadOnlyBuffer(OwnedMemory<byte> data, int offset, int length);
        public ReadOnlyBuffer(byte[] data);
        public ReadOnlyBuffer(byte[] data, int offset, int length);
        public ReadOnlyBuffer(IEnumerable<Memory<byte>> buffers);
        public ReadOnlyBuffer(IBufferList startSegment, int offset, IBufferList endSegment, int endIndex);
        public Position End { get; }
        public ReadOnlyMemory<byte> First { get; }
        public bool IsEmpty { get; }
        public bool IsSingleSpan { get; }
        public long Length { get; }
        public Position Start { get; }
        public void CopyTo(Span<byte> destination);
        public ReadOnlyBuffer.Enumerator GetEnumerator();
        public Nullable<Position> PositionOf(byte value);
        public Position Seek(Position position, long count);
        public ReadOnlyBuffer Slice(Position start);
        public ReadOnlyBuffer Slice(Position start, Position end);
        public ReadOnlyBuffer Slice(Position start, long length);
        public ReadOnlyBuffer Slice(long start);
        public ReadOnlyBuffer Slice(long start, Position end);
        public ReadOnlyBuffer Slice(long start, long length);
        public byte[] ToArray();
        public override string ToString();
        public bool TryGet(ref Position cursor, out ReadOnlyMemory<byte> data, bool advance=true);
        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
        public struct Enumerator {
            public Enumerator(ReadOnlyBuffer readOnlyBuffer);
            public ReadOnlyMemory<byte> Current { get; set; }
            public ReadOnlyBuffer.Enumerator GetEnumerator();
            public bool MoveNext();
            public void Reset();
        }
    }
}
namespace System.Collections.Sequences {
    public interface IBufferList {
        Memory<byte> Memory { get; }
        IBufferList Next { get; }
        long VirtualIndex { get; }
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Position : IEquatable<Position> {
        public Position(object segment);
        public Position(object segment, int index);
        public int Index { get; }
        public object Segment { get; }
        public bool Equals(Position position);
        public override bool Equals(object obj);
        public ValueTuple<T, int> Get<T>();
        public override int GetHashCode();
        public T GetSegment<T>();
        public static Position operator +(Position value, int index);
        public static bool operator ==(Position left, Position right);
        public static explicit operator int (Position position);
        public static bool operator !=(Position left, Position right);
        public override string ToString();
    }
}
```


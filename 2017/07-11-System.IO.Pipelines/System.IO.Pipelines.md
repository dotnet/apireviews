# System.IO.Pipelines.dll

```C#
namespace System.IO.Pipelines {
  public struct BufferEnumerator {
    public BufferEnumerator(ReadCursor start, ReadCursor end);
    public Buffer<byte> Current { get; }
    public bool MoveNext();
    public void Reset();
  }
  public struct FlushResult {
    public bool IsCancelled { get; }
    public bool IsCompleted { get; }
  }
  public class InlineScheduler : IScheduler {
    public static readonly InlineScheduler Default;
    public InlineScheduler();
    public void Schedule(Action<object> action, object state);
  }
  public interface IPipe {
    IPipeReader Reader { get; }
    IPipeWriter Writer { get; }
    void Reset();
  }
  public interface IPipeConnection : IDisposable {
    IPipeReader Input { get; }
    IPipeWriter Output { get; }
  }
  public interface IPipeReader {
    void Advance(ReadCursor consumed, ReadCursor examined);
    void CancelPendingRead();
    void Complete(Exception exception=null);
    void OnWriterCompleted(Action<Exception, object> callback, object state);
    ReadableBufferAwaitable ReadAsync(CancellationToken cancellationToken=default(CancellationToken));
    bool TryRead(out ReadResult result);
  }
  public interface IPipeWriter {
    WritableBuffer Alloc(int minimumSize=0);
    void CancelPendingFlush();
    void Complete(Exception exception=null);
    void OnReaderCompleted(Action<Exception, object> callback, object state);
  }
  public interface IReadableBufferAwaiter {
    bool IsCompleted { get; }
    ReadResult GetResult();
    void OnCompleted(Action continuation);
  }
  public interface IScheduler {
    void Schedule(Action<object> action, object state);
  }
  public interface IWritableBufferAwaiter {
    bool IsCompleted { get; }
    FlushResult GetResult();
    void OnCompleted(Action continuation);
  }
  public class MemoryPool : BufferPool {
    public const int MaxPooledBlockLength = 4032;
    public MemoryPool();
    protected override void Dispose(bool disposing);
    public void RegisterSlabAllocationCallback(Action<MemoryPoolSlab> callback);
    public void RegisterSlabDeallocationCallback(Action<MemoryPoolSlab> callback);
    public override OwnedBuffer<byte> Rent(int size);
    public void Return(MemoryPoolBlock block);
  }
  public class MemoryPoolBlock : OwnedBuffer<byte> {
    protected MemoryPoolBlock(MemoryPool pool, MemoryPoolSlab slab, int offset, int length);
    public override int Length { get; }
    public MemoryPool Pool { get; }
    public MemoryPoolSlab Slab { get; }
    public override Span<byte> Span { get; }
    ~MemoryPoolBlock();
    protected override void OnZeroReferences();
    public override string ToString();
    protected override bool TryGetArrayInternal(out ArraySegment<byte> buffer);
    protected unsafe override bool TryGetPointerInternal(out void* pointer);
  }
  public class MemoryPoolSlab : IDisposable {
    public MemoryPoolSlab(byte[] data);
    public byte[] Array { get; }
    public bool IsActive { get; }
    public int Length { get; }
    public IntPtr NativePointer { get; }
    public static MemoryPoolSlab Create(int length);
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    ~MemoryPoolSlab();
  }
  public class PipeFactory : IDisposable {
    public PipeFactory();
    public PipeFactory(BufferPool pool);
    public IPipe Create();
    public IPipe Create(PipeOptions options);
    public void Dispose();
  }
  public static class PipelineReaderExtensions {
    public static void Advance(this IPipeReader input, ReadCursor cursor);
  }
  public static class PipelineWriterExtensions {
    public static Task WriteAsync(this IPipeWriter output, ArraySegment<byte> source);
    public static Task WriteAsync(this IPipeWriter output, byte[] source);
  }
  public class PipeOptions {
    public PipeOptions();
    public long MaximumSizeHigh { get; set; }
    public long MaximumSizeLow { get; set; }
    public IScheduler ReaderScheduler { get; set; }
    public IScheduler WriterScheduler { get; set; }
  }
  public struct PreservedBuffer : IDisposable {
    public ReadableBuffer Buffer { get; }
    public void Dispose();
  }
  public struct ReadableBuffer : ISequence<ReadOnlyBuffer<byte>> {
    public ReadCursor End { get; }
    public Buffer<byte> First { get; }
    public bool IsEmpty { get; }
    public bool IsSingleSpan { get; }
    public int Length { get; }
    public ReadCursor Start { get; }
    public void CopyTo(Span<byte> destination);
    public static ReadableBuffer Create(OwnedBuffer<byte> data, int offset, int length);
    public static ReadableBuffer Create(byte[] data);
    public static ReadableBuffer Create(byte[] data, int offset, int length);
    public BufferEnumerator GetEnumerator();
    public ReadCursor Move(ReadCursor cursor, int count);
    public PreservedBuffer Preserve();
    public ReadableBuffer Slice(int start);
    public ReadableBuffer Slice(int start, int length);
    public ReadableBuffer Slice(int start, ReadCursor end);
    public ReadableBuffer Slice(ReadCursor start);
    public ReadableBuffer Slice(ReadCursor start, int length);
    public ReadableBuffer Slice(ReadCursor start, ReadCursor end);
    bool System.Collections.Sequences.ISequence<System.Buffers.ReadOnlyBuffer<System.Byte>>.TryGet(ref Position position, out ReadOnlyBuffer<byte> item, bool advance);
    public byte[] ToArray();
    public override string ToString();
  }
  public struct ReadableBufferAwaitable : ICriticalNotifyCompletion, INotifyCompletion {
    public ReadableBufferAwaitable(IReadableBufferAwaiter awaiter);
    public bool IsCompleted { get; }
    public ReadableBufferAwaitable GetAwaiter();
    public ReadResult GetResult();
    public void OnCompleted(Action continuation);
    public void UnsafeOnCompleted(Action continuation);
  }
  public struct ReadableBufferReader {
    public ReadableBufferReader(ReadableBuffer buffer);
    public ReadableBufferReader(ReadCursor start, ReadCursor end);
    public int ConsumedBytes { get; }
    public ReadCursor Cursor { get; }
    public bool End { get; }
    public int Index { get; }
    public Span<byte> Span { get; }
    public int Peek();
    public void Skip(int length);
    public int Take();
  }
  public struct ReadCursor : IEquatable<ReadCursor> {
    public bool Equals(ReadCursor other);
    public override bool Equals(object obj);
    public override int GetHashCode();
    public static bool operator ==(ReadCursor c1, ReadCursor c2);
    public static bool operator !=(ReadCursor c1, ReadCursor c2);
    public override string ToString();
  }
  public static class ReadCursorOperations {
    public static int Seek(ReadCursor begin, ReadCursor end, out ReadCursor result, byte byte0);
    public static int Seek(ReadCursor begin, ReadCursor end, out ReadCursor result, byte byte0, byte byte1);
    public static int Seek(ReadCursor begin, ReadCursor end, out ReadCursor result, byte byte0, byte byte1, byte byte2);
  }
  public struct ReadResult {
    public ReadResult(ReadableBuffer buffer, bool isCancelled, bool isCompleted);
    public ReadableBuffer Buffer { get; }
    public bool IsCancelled { get; }
    public bool IsCompleted { get; }
  }
  public class TaskRunScheduler : IScheduler {
    public static TaskRunScheduler Default;
    public TaskRunScheduler();
    public void Schedule(Action<object> action, object state);
  }
  public class UnownedBuffer : OwnedBuffer<byte> {
    public UnownedBuffer(ArraySegment<byte> buffer);
    public override int Length { get; }
    public override Span<byte> Span { get; }
    public OwnedBuffer<byte> MakeCopy(int offset, int length, out int newStart, out int newEnd);
    protected override bool TryGetArrayInternal(out ArraySegment<byte> buffer);
    protected unsafe override bool TryGetPointerInternal(out void* pointer);
  }
  public struct WritableBuffer : IOutput {
    public Buffer<byte> Buffer { get; }
    public int BytesWritten { get; }
    Span<byte> System.Buffers.IOutput.Buffer { get; }
    public void Advance(int bytesWritten);
    public void Append(ReadableBuffer buffer);
    public ReadableBuffer AsReadableBuffer();
    public void Commit();
    public void Ensure(int count=1);
    public WritableBufferAwaitable FlushAsync(CancellationToken cancellationToken=default(CancellationToken));
    void System.Buffers.IOutput.Enlarge(int desiredBufferLength);
  }
  public struct WritableBufferAwaitable : ICriticalNotifyCompletion, INotifyCompletion {
    public WritableBufferAwaitable(IWritableBufferAwaiter awaiter);
    public bool IsCompleted { get; }
    public WritableBufferAwaitable GetAwaiter();
    public FlushResult GetResult();
    public void OnCompleted(Action continuation);
    public void UnsafeOnCompleted(Action continuation);
  }
  public static class WritableBufferExtensions {
    public static void Write(this WritableBuffer buffer, byte[] source);
    public static void Write(this WritableBuffer buffer, byte[] source, int offset, int length);
    public static void Write(this WritableBuffer buffer, ReadOnlySpan<byte> source);
  }
  public struct WritableBufferWriter {
    public WritableBufferWriter(WritableBuffer writableBuffer);
    public Span<byte> Span { get; }
    public void Advance(int count);
    public void Ensure(int count=1);
    public void Write(byte[] source);
    public void Write(byte[] source, int offset, int length);
  }
}
namespace System.IO.Pipelines.Testing {
  public class BufferUtilities {
    public BufferUtilities();
    public static ReadableBuffer CreateBuffer(params byte[][] inputs);
    public static ReadableBuffer CreateBuffer(params int[] inputs);
    public static ReadableBuffer CreateBuffer(params string[] inputs);
  }
}
```
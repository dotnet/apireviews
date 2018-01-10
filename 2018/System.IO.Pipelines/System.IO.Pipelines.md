# System.IO.Pipelines.dll

```csharp
namespace System.Buffers {
    public class BufferReader {
        public BufferReader();
        public static BufferReader<TSequence> Create<TSequence>(TSequence buffer) where TSequence : ISequence<ReadOnlyMemory<byte>>;
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct BufferReader<TSequence> where TSequence : ISequence<ReadOnlyMemory<byte>> {
        public BufferReader(TSequence buffer);
        public int ConsumedBytes { get; }
        public bool End { get; }
        public int Index { get; }
        public Position Position { get; }
        public ReadOnlySpan<byte> Span { get; }
        public int Peek();
        public void Skip(int length);
        public int Take();
    }
    public interface IBufferOperation {
        OperationStatus Execute(ReadOnlySpan<byte> input, Span<byte> output, out int consumed, out int written);
    }
    public interface IBufferTransformation : IBufferOperation {
        OperationStatus Transform(Span<byte> buffer, int dataLength, out int written);
    }
    public interface IOutput {
        void Advance(int bytes);
        void Enlarge(int desiredBufferLength=0);
        Span<byte> GetSpan();
    }
    public class MemoryPool : MemoryPool<byte> {
        public MemoryPool();
        public override int MaxBufferSize { get; }
        protected override void Dispose(bool disposing);
        public override OwnedMemory<byte> Rent(int size=-2147483648);
    }
    public abstract class MemoryPool<T> : IDisposable {
        public const int AnySize = -2147483648;
        protected MemoryPool();
        public static MemoryPool<T> Default { get; }
        public abstract int MaxBufferSize { get; }
        public void Dispose();
        protected abstract void Dispose(bool disposing);
        ~MemoryPool();
        public abstract OwnedMemory<T> Rent(int minimumBufferSize=-2147483648);
    }
    public static class OutputWriter {
        public static OutputWriter<T> Create<T>(T output) where T : IOutput;
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct OutputWriter<T> where T : IOutput {
        public OutputWriter(T output);
        public Span<byte> Span { get; }
        public void Advance(int count);
        public void Ensure(int count=1);
        public void Write(byte[] source);
        public void Write(byte[] source, int offset, int length);
    }
    public class OwnedArray<T> : OwnedMemory<T> {
        public OwnedArray(int length);
        public OwnedArray(T[] array);
        public override bool IsDisposed { get; }
        protected override bool IsRetained { get; }
        public override int Length { get; }
        public override Span<T> Span { get; }
        protected override void Dispose(bool disposing);
        protected virtual void OnNoReferences();
        public static implicit operator OwnedArray<T> (T[] array);
        public override MemoryHandle Pin();
        public override bool Release();
        public override void Retain();
        public static void ThrowArgumentNullException(string argumentName);
        public static void ThrowInvalidOperationException();
        public static void ThrowObjectDisposedException(string objectName);
        protected override bool TryGetArray(out ArraySegment<T> arraySegment);
    }
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
    public interface ISequence<T> {
        Position Start { get; }
        bool TryGet(ref Position position, out T item, bool advance=true);
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
namespace System.IO.Pipelines {
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct FlushResult {
        public bool IsCancelled { get; }
        public bool IsCompleted { get; }
    }
    public interface IPipe {
        IPipeReader Reader { get; }
        IPipeWriter Writer { get; }
    }
    public interface IPipeConnection : IDisposable {
        IPipeReader Input { get; }
        IPipeWriter Output { get; }
    }
    public interface IPipeReader {
        void Advance(Position consumed);
        void Advance(Position consumed, Position examined);
        void CancelPendingRead();
        void Complete(Exception exception=null);
        void OnWriterCompleted(Action<Exception, object> callback, object state);
        ValueAwaiter<ReadResult> ReadAsync(CancellationToken cancellationToken=default(CancellationToken));
        bool TryRead(out ReadResult result);
    }
    public interface IPipeWriter {
        WritableBuffer Alloc(int minimumSize=0);
        void CancelPendingFlush();
        void Complete(Exception exception=null);
        void OnReaderCompleted(Action<Exception, object> callback, object state);
    }
    public class Pipe : IAwaiter<FlushResult>, IAwaiter<ReadResult>, IPipe, IPipeReader, IPipeWriter {
        public Pipe(PipeOptions options);
        public IPipeReader Reader { get; }
        bool System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.IsCompleted { get; }
        bool System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.IsCompleted { get; }
        public IPipeWriter Writer { get; }
        public void Reset();
        void System.IO.Pipelines.IPipeReader.Advance(Position consumed);
        void System.IO.Pipelines.IPipeReader.Advance(Position consumed, Position examined);
        void System.IO.Pipelines.IPipeReader.CancelPendingRead();
        void System.IO.Pipelines.IPipeReader.Complete(Exception exception);
        void System.IO.Pipelines.IPipeReader.OnWriterCompleted(Action<Exception, object> callback, object state);
        ValueAwaiter<ReadResult> System.IO.Pipelines.IPipeReader.ReadAsync(CancellationToken token);
        bool System.IO.Pipelines.IPipeReader.TryRead(out ReadResult result);
        WritableBuffer System.IO.Pipelines.IPipeWriter.Alloc(int minimumSize);
        void System.IO.Pipelines.IPipeWriter.CancelPendingFlush();
        void System.IO.Pipelines.IPipeWriter.Complete(Exception exception);
        void System.IO.Pipelines.IPipeWriter.OnReaderCompleted(Action<Exception, object> callback, object state);
        FlushResult System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.GetResult();
        void System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.OnCompleted(Action continuation);
        ReadResult System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.GetResult();
        void System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.OnCompleted(Action continuation);
    }
    public static class PipelineExtensions {
        public static Task WriteAsync(this IPipeWriter output, byte[] source);
        public static Task WriteAsync(this IPipeWriter output, ReadOnlyMemory<byte> source);
    }
    public class PipeOptions {
        public PipeOptions(MemoryPool<byte> pool, Scheduler readerScheduler=null, Scheduler writerScheduler=null, long maximumSizeHigh=(long)0, long maximumSizeLow=(long)0);
        public long MaximumSizeHigh { get; }
        public long MaximumSizeLow { get; }
        public MemoryPool<byte> Pool { get; }
        public Scheduler ReaderScheduler { get; }
        public Scheduler WriterScheduler { get; }
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ReadResult {
        public ReadResult(ReadOnlyBuffer buffer, bool isCancelled, bool isCompleted);
        public ReadOnlyBuffer Buffer { get; }
        public bool IsCancelled { get; }
        public bool IsCompleted { get; }
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct WritableBuffer : IOutput {
        public Memory<byte> Buffer { get; }
        public void Advance(int bytesWritten);
        public void Commit();
        public void Ensure(int count=1);
        public ValueAwaiter<FlushResult> FlushAsync(CancellationToken cancellationToken=default(CancellationToken));
        void System.Buffers.IOutput.Enlarge(int desiredBufferLength);
        Span<byte> System.Buffers.IOutput.GetSpan();
        public void Write(ReadOnlySpan<byte> source);
    }
}
namespace System.Threading {
    public interface IAwaiter<out T> {
        bool IsCompleted { get; }
        T GetResult();
        void OnCompleted(Action continuation);
    }
    public abstract class Scheduler {
        protected Scheduler();
        public static Scheduler Inline { get; }
        public static Scheduler TaskRun { get; }
        public abstract void Schedule(Action action);
        public abstract void Schedule(Action<object> action, object state);
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ValueAwaiter<T> : ICriticalNotifyCompletion, INotifyCompletion {
        public ValueAwaiter(IAwaiter<T> awaiter);
        public bool IsCompleted { get; }
        public ValueAwaiter<T> GetAwaiter();
        public T GetResult();
        public void OnCompleted(Action continuation);
        public void UnsafeOnCompleted(Action continuation);
    }
}
```


# System.IO.Pipelines.dll

```C#
namespace System.Buffers {
    public class MemoryPool : MemoryPool<byte> {
        public MemoryPool();
        public override int MaxBufferSize { get; }
        protected override void Dispose(bool disposing);
        public override OwnedMemory<byte> Rent(int size=-2147483648);
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


# System.IO.Pipelines

```csharp
public interface IPipe {
    IPipeReader Reader { get; }
    IPipeWriter Writer { get; }
}
public interface IPipeReader {
    ValueAwaiter<ReadResult> ReadAsync(CancellationToken cancellationToken=default(CancellationToken));
    bool TryRead(out ReadResult result);

    void Advance(Position consumed);
    void Advance(Position consumed, Position examined);

    void CancelPendingRead();
    void Complete(Exception exception=null);
    void OnWriterCompleted(Action<Exception, object> callback, object state);
}
public interface IPipeWriter : IOutput {
    ValueAwaiter<FlushResult> FlushAsync(CancellationToken cancellationToken=default(CancellationToken));
    void Commit();

    void CancelPendingFlush();
    void Complete(Exception exception=null);
    void OnReaderCompleted(Action<Exception, object> callback, object state);
}

public interface IPipeConnection : IDisposable {
    IPipeReader Input { get; }
    IPipeWriter Output { get; }
}

public struct FlushResult {
    public bool IsCancelled { get; }
    public bool IsCompleted { get; }
}
public struct ReadResult {
    public ReadResult(ReadOnlyBuffer buffer, bool isCancelled, bool isCompleted);
    public ReadOnlyBuffer Buffer { get; }
    public bool IsCancelled { get; }
    public bool IsCompleted { get; }
}
public class Pipe : IAwaiter<FlushResult>, IAwaiter<ReadResult>, IOutput, IPipe, IPipeReader, IPipeWriter {
    public Pipe(PipeOptions options);
    public IPipeReader Reader { get; }
    public IPipeWriter Writer { get; }
    public void Reset();
}

public class PipeOptions {
    public PipeOptions(MemoryPool<byte> pool, Scheduler readerScheduler=null, Scheduler writerScheduler=null, long maximumSizeHigh=0, long maximumSizeLow=0, int minimumSegmentSize=2048);
    public long MaximumSizeHigh { get; }
    public long MaximumSizeLow { get; }
    public int MinimumSegmentSize { get; }
    public MemoryPool<byte> Pool { get; }
    public Scheduler ReaderScheduler { get; }
    public Scheduler WriterScheduler { get; }
} 

public static class PipelineExtensions {
    public static Task WriteAsync(this IPipeWriter output, byte[] source);
    public static Task WriteAsync(this IPipeWriter output, ReadOnlyMemory<byte> source);
    }

public interface IOutput {
     void Advance(int bytes);
     Memory<byte> GetMemory(int minimumSize=0);
}
```
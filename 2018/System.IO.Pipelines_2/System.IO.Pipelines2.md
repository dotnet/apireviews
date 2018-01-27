# System.IO.Pipelines

```csharp
namespace System.IO.Pipelines {
 public readonly struct FlushResult {
   public bool IsCancelled { get; }
   public bool IsCompleted { get; }
 }
 public interface IDuplexPipe : IDisposable {
   PipeReader Input { get; }
   PipeWriter Output { get; }
 }
 public class Pipe : IAwaiter<FlushResult>, IAwaiter<ReadResult> {
   public Pipe();
   public Pipe(PipeOptions options);
   public PipeReader Reader { get; }
   bool System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.IsCompleted { get; }
   bool System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.IsCompleted { get; }
   public PipeWriter Writer { get; }
   public void Reset();
   FlushResult System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.GetResult();
   void System.Threading.IAwaiter<System.IO.Pipelines.FlushResult>.OnCompleted(Action continuation);
   ReadResult System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.GetResult();
   void System.Threading.IAwaiter<System.IO.Pipelines.ReadResult>.OnCompleted(Action continuation);
 }
 public static class PipelineExtensions {
   public static Task WriteAsync(this PipeWriter output, ReadOnlyMemory<byte> source);
 }
 public class PipeOptions {
   public PipeOptions(MemoryPool<byte> pool=null, PipeScheduler readerScheduler=null, PipeScheduler writerScheduler=null, long pauseWriterThreshold=0, long resumeWriterThreshold=0, int minimumSegmentSize=2048);
   public static PipeOptions Default { get; }
   public int MinimumSegmentSize { get; }
   public long PauseWriterThreshold { get; }
   public MemoryPool<byte> Pool { get; }
   public PipeScheduler ReaderScheduler { get; }
   public long ResumeWriterThreshold { get; }
   public PipeScheduler WriterScheduler { get; }
 }
 public abstract class PipeReader {
   protected PipeReader();
   public abstract void AdvanceTo(SequencePosition consumed);
   public abstract void AdvanceTo(SequencePosition consumed, SequencePosition examined);
   public abstract void CancelPendingRead();
   public abstract void Complete(Exception exception=null);
   public abstract void OnWriterCompleted(Action<Exception, object> callback, object state);
   public abstract ValueAwaiter<ReadResult> ReadAsync(CancellationToken cancellationToken=default);
   public abstract bool TryRead(out ReadResult result);
 }
 public abstract class PipeWriter : IBufferWriter {
   protected PipeWriter();
   public abstract void Advance(int bytes);
   public abstract void CancelPendingFlush();
   public abstract void Commit();
   public abstract void Complete(Exception exception=null);
   public abstract ValueAwaiter<FlushResult> FlushAsync(CancellationToken cancellationToken=default);
   public abstract Memory<byte> GetMemory(int minimumLength=0);
   public abstract Span<byte> GetSpan(int minimumLength=0);
   public abstract void OnReaderCompleted(Action<Exception, object> callback, object state);
 }
 public readonly struct ReadResult {
   public ReadResult(ReadOnlyBuffer<byte> buffer, bool isCancelled, bool isCompleted);
   public ReadOnlyBuffer<byte> Buffer { get; }
   public bool IsCancelled { get; }
   public bool IsCompleted { get; }
 }
}
```

# System.Threading.*

This covers the following reference assemblies:

* System.Threading
* System.Threading.Overlapped
* System.Threading.Tasks
* System.Threading.Thread
* System.Threading.ThreadPool
* System.Threading.Timer

```diff
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Threading\before
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Threading\after
 namespace System {
  public sealed class LocalDataStoreSlot {
    ~LocalDataStoreSlot();
  }
  public class OperationCanceledException : SystemException {
    public OperationCanceledException();
    protected OperationCanceledException(SerializationInfo info, StreamingContext context);
    public OperationCanceledException(string? message);
    public OperationCanceledException(string? message, Exception? innerException);
    public OperationCanceledException(string? message, Exception? innerException, CancellationToken token);
    public OperationCanceledException(string? message, CancellationToken token);
    public OperationCanceledException(CancellationToken token);
    public CancellationToken CancellationToken { get; }
  }
 }
 namespace System.Runtime.CompilerServices {
  public struct AsyncIteratorMethodBuilder {
    public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
    public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
    public void Complete();
    public static AsyncIteratorMethodBuilder Create();
    public void MoveNext<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
  }
  public struct AsyncTaskMethodBuilder {
    public Task Task { get; }
    public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
    public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
    public static AsyncTaskMethodBuilder Create();
    public void SetException(Exception exception);
    public void SetResult();
    public void SetStateMachine(IAsyncStateMachine stateMachine);
    public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
  }
  public struct AsyncTaskMethodBuilder<TResult> {
    public Task<TResult> Task { get; }
    public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
    public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
    public static AsyncTaskMethodBuilder<TResult> Create();
    public void SetException(Exception exception);
    public void SetResult(TResult result);
    public void SetStateMachine(IAsyncStateMachine stateMachine);
    public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
  }
  public struct AsyncVoidMethodBuilder {
    public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
    public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
    public static AsyncVoidMethodBuilder Create();
    public void SetException(Exception exception);
    public void SetResult();
    public void SetStateMachine(IAsyncStateMachine stateMachine);
    public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
  }
  public readonly struct ConfiguredAsyncDisposable {
    public ConfiguredValueTaskAwaitable DisposeAsync();
  }
  public readonly struct ConfiguredCancelableAsyncEnumerable<T> {
    public ConfiguredCancelableAsyncEnumerable<T> ConfigureAwait(bool continueOnCapturedContext);
    public ConfiguredCancelableAsyncEnumerable<T>.Enumerator GetAsyncEnumerator();
    public ConfiguredCancelableAsyncEnumerable<T> WithCancellation(CancellationToken cancellationToken);
    public readonly struct Enumerator {
      public T Current { get; }
      public ConfiguredValueTaskAwaitable DisposeAsync();
      public ConfiguredValueTaskAwaitable<bool> MoveNextAsync();
    }
  }
 }
 namespace System.Threading {
  public class AbandonedMutexException : SystemException {
    public AbandonedMutexException();
    public AbandonedMutexException(int location, WaitHandle? handle);
    protected AbandonedMutexException(SerializationInfo info, StreamingContext context);
    public AbandonedMutexException(string? message);
    public AbandonedMutexException(string? message, Exception? inner);
    public AbandonedMutexException(string? message, Exception? inner, int location, WaitHandle? handle);
    public AbandonedMutexException(string? message, int location, WaitHandle? handle);
    public Mutex? Mutex { get; }
    public int MutexIndex { get; }
  }
  public enum ApartmentState {
    MTA = 1,
    STA = 0,
    Unknown = 2,
  }
  public struct AsyncFlowControl : IDisposable {
    public void Dispose();
    public override bool Equals(object? obj);
    public bool Equals(AsyncFlowControl obj);
    public override int GetHashCode();
    public static bool operator ==(AsyncFlowControl a, AsyncFlowControl b);
    public static bool operator !=(AsyncFlowControl a, AsyncFlowControl b);
    public void Undo();
  }
  public sealed class AsyncLocal<T> {
    public AsyncLocal();
    public AsyncLocal(Action<AsyncLocalValueChangedArgs<T>>? valueChangedHandler);
    public T Value { get; set; }
  }
  public readonly struct AsyncLocalValueChangedArgs<T> {
    public T CurrentValue { get; }
    public T PreviousValue { get; }
    public bool ThreadContextChanged { get; }
  }
  public sealed class AutoResetEvent : EventWaitHandle {
    public AutoResetEvent(bool initialState);
  }
  public class Barrier : IDisposable {
    public Barrier(int participantCount);
    public Barrier(int participantCount, Action<Barrier>? postPhaseAction);
    public long CurrentPhaseNumber { get; }
    public int ParticipantCount { get; }
    public int ParticipantsRemaining { get; }
    public long AddParticipant();
    public long AddParticipants(int participantCount);
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public void RemoveParticipant();
    public void RemoveParticipants(int participantCount);
    public void SignalAndWait();
    public bool SignalAndWait(int millisecondsTimeout);
    public bool SignalAndWait(int millisecondsTimeout, CancellationToken cancellationToken);
    public void SignalAndWait(CancellationToken cancellationToken);
    public bool SignalAndWait(TimeSpan timeout);
    public bool SignalAndWait(TimeSpan timeout, CancellationToken cancellationToken);
  }
  public class BarrierPostPhaseException : Exception {
    public BarrierPostPhaseException();
    public BarrierPostPhaseException(Exception? innerException);
    protected BarrierPostPhaseException(SerializationInfo info, StreamingContext context);
    public BarrierPostPhaseException(string? message);
    public BarrierPostPhaseException(string? message, Exception? innerException);
  }
  public class CancellationTokenSource : IDisposable {
    public CancellationTokenSource();
    public CancellationTokenSource(int millisecondsDelay);
    public CancellationTokenSource(TimeSpan delay);
    public bool IsCancellationRequested { get; }
    public CancellationToken Token { get; }
    public void Cancel();
    public void Cancel(bool throwOnFirstException);
    public void CancelAfter(int millisecondsDelay);
    public void CancelAfter(TimeSpan delay);
    public static CancellationTokenSource CreateLinkedTokenSource(CancellationToken token1, CancellationToken token2);
    public static CancellationTokenSource CreateLinkedTokenSource(params CancellationToken[] tokens);
    public void Dispose();
    protected virtual void Dispose(bool disposing);
  }
  public sealed class CompressedStack : ISerializable {
    public static CompressedStack Capture();
    public CompressedStack CreateCopy();
    public static CompressedStack GetCompressedStack();
    public void GetObjectData(SerializationInfo info, StreamingContext context);
    public static void Run(CompressedStack compressedStack, ContextCallback callback, object? state);
  }
  public delegate void ContextCallback(object? state);
  public class CountdownEvent : IDisposable {
    public CountdownEvent(int initialCount);
    public int CurrentCount { get; }
    public int InitialCount { get; }
    public bool IsSet { get; }
    public WaitHandle WaitHandle { get; }
    public void AddCount();
    public void AddCount(int signalCount);
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public void Reset();
    public void Reset(int count);
    public bool Signal();
    public bool Signal(int signalCount);
    public bool TryAddCount();
    public bool TryAddCount(int signalCount);
    public void Wait();
    public bool Wait(int millisecondsTimeout);
    public bool Wait(int millisecondsTimeout, CancellationToken cancellationToken);
    public void Wait(CancellationToken cancellationToken);
    public bool Wait(TimeSpan timeout);
    public bool Wait(TimeSpan timeout, CancellationToken cancellationToken);
  }
  public enum EventResetMode {
    AutoReset = 0,
    ManualReset = 1,
  }
  public class EventWaitHandle : WaitHandle {
    public EventWaitHandle(bool initialState, EventResetMode mode);
    public EventWaitHandle(bool initialState, EventResetMode mode, string? name);
    public EventWaitHandle(bool initialState, EventResetMode mode, string? name, out bool createdNew);
    public static EventWaitHandle OpenExisting(string name);
    public bool Reset();
    public bool Set();
    public static bool TryOpenExisting(string name, out EventWaitHandle? result);
  }
  public sealed class ExecutionContext : IDisposable, ISerializable {
    public static ExecutionContext? Capture();
    public ExecutionContext CreateCopy();
    public void Dispose();
    public void GetObjectData(SerializationInfo info, StreamingContext context);
    public static bool IsFlowSuppressed();
    public static void RestoreFlow();
    public static void Run(ExecutionContext executionContext, ContextCallback callback, object? state);
    public static AsyncFlowControl SuppressFlow();
  }
  public class HostExecutionContext : IDisposable {
    public HostExecutionContext();
    public HostExecutionContext(object? state);
    protected internal object? State { get; set; }
    public virtual HostExecutionContext CreateCopy();
    public void Dispose();
    public virtual void Dispose(bool disposing);
  }
  public class HostExecutionContextManager {
    public HostExecutionContextManager();
    public virtual HostExecutionContext? Capture();
    public virtual void Revert(object previousState);
    public virtual object SetHostExecutionContext(HostExecutionContext hostExecutionContext);
  }
  public static class Interlocked {
    public static int Add(ref int location1, int value);
    public static long Add(ref long location1, long value);
    public static double CompareExchange(ref double location1, double value, double comparand);
    public static int CompareExchange(ref int location1, int value, int comparand);
    public static long CompareExchange(ref long location1, long value, long comparand);
    public static IntPtr CompareExchange(ref IntPtr location1, IntPtr value, IntPtr comparand);
    public static object? CompareExchange(ref object? location1, object? value, object? comparand);
    public static float CompareExchange(ref float location1, float value, float comparand);
    public static T CompareExchange<T>(ref T location1, T value, T comparand) where T : class?;
    public static int Decrement(ref int location);
    public static long Decrement(ref long location);
    public static double Exchange(ref double location1, double value);
    public static int Exchange(ref int location1, int value);
    public static long Exchange(ref long location1, long value);
    public static IntPtr Exchange(ref IntPtr location1, IntPtr value);
    public static object? Exchange(ref object? location1, object? value);
    public static float Exchange(ref float location1, float value);
    public static T Exchange<T>(ref T location1, T value) where T : class?;
    public static int Increment(ref int location);
    public static long Increment(ref long location);
    public static void MemoryBarrier();
    public static void MemoryBarrierProcessWide();
    public static long Read(ref long location);
  }
  public unsafe delegate void IOCompletionCallback(uint errorCode, uint numBytes, NativeOverlapped* pOVERLAP);
  public interface IThreadPoolWorkItem {
    void Execute();
  }
  public static class LazyInitializer {
    public static T EnsureInitialized<T>(ref T target) where T : class;
    public static T EnsureInitialized<T>(ref T target, ref bool initialized, ref object? syncLock);
    public static T EnsureInitialized<T>(ref T target, ref bool initialized, ref object? syncLock, Func<T> valueFactory);
    public static T EnsureInitialized<T>(ref T target, Func<T> valueFactory) where T : class;
    public static T EnsureInitialized<T>(ref T target, ref object? syncLock, Func<T> valueFactory) where T : class;
  }
  public struct LockCookie {
    public override bool Equals(object? obj);
    public bool Equals(LockCookie obj);
    public override int GetHashCode();
    public static bool operator ==(LockCookie a, LockCookie b);
    public static bool operator !=(LockCookie a, LockCookie b);
  }
  public class LockRecursionException : Exception {
    public LockRecursionException();
    protected LockRecursionException(SerializationInfo info, StreamingContext context);
    public LockRecursionException(string? message);
    public LockRecursionException(string? message, Exception? innerException);
  }
  public enum LockRecursionPolicy {
    NoRecursion = 0,
    SupportsRecursion = 1,
  }
  public sealed class ManualResetEvent : EventWaitHandle {
    public ManualResetEvent(bool initialState);
  }
  public class ManualResetEventSlim : IDisposable {
    public ManualResetEventSlim();
    public ManualResetEventSlim(bool initialState);
    public ManualResetEventSlim(bool initialState, int spinCount);
    public bool IsSet { get; }
    public int SpinCount { get; }
    public WaitHandle WaitHandle { get; }
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public void Reset();
    public void Set();
    public void Wait();
    public bool Wait(int millisecondsTimeout);
    public bool Wait(int millisecondsTimeout, CancellationToken cancellationToken);
    public void Wait(CancellationToken cancellationToken);
    public bool Wait(TimeSpan timeout);
    public bool Wait(TimeSpan timeout, CancellationToken cancellationToken);
  }
  public static class Monitor {
    public static long LockContentionCount { get; }
    public static void Enter(object obj);
    public static void Enter(object obj, ref bool lockTaken);
    public static void Exit(object obj);
    public static bool IsEntered(object obj);
    public static void Pulse(object obj);
    public static void PulseAll(object obj);
    public static bool TryEnter(object obj);
    public static void TryEnter(object obj, ref bool lockTaken);
    public static bool TryEnter(object obj, int millisecondsTimeout);
    public static void TryEnter(object obj, int millisecondsTimeout, ref bool lockTaken);
    public static bool TryEnter(object obj, TimeSpan timeout);
    public static void TryEnter(object obj, TimeSpan timeout, ref bool lockTaken);
    public static bool Wait(object obj);
    public static bool Wait(object obj, int millisecondsTimeout);
    public static bool Wait(object obj, int millisecondsTimeout, bool exitContext);
    public static bool Wait(object obj, TimeSpan timeout);
    public static bool Wait(object obj, TimeSpan timeout, bool exitContext);
  }
  public sealed class Mutex : WaitHandle {
    public Mutex();
    public Mutex(bool initiallyOwned);
    public Mutex(bool initiallyOwned, string? name);
    public Mutex(bool initiallyOwned, string? name, out bool createdNew);
    public static Mutex OpenExisting(string name);
    public void ReleaseMutex();
    public static bool TryOpenExisting(string name, out Mutex? result);
  }
  public struct NativeOverlapped {
    public int OffsetHigh;
    public int OffsetLow;
    public IntPtr EventHandle;
    public IntPtr InternalHigh;
    public IntPtr InternalLow;
  }
  public class Overlapped {
    public Overlapped();
    public Overlapped(int offsetLo, int offsetHi, int hEvent, IAsyncResult ar);
    public Overlapped(int offsetLo, int offsetHi, IntPtr hEvent, IAsyncResult ar);
    public IAsyncResult? AsyncResult { get; set; }
    public int EventHandle { get; set; }
    public IntPtr EventHandleIntPtr { get; set; }
    public int OffsetHigh { get; set; }
    public int OffsetLow { get; set; }
    public unsafe static void Free(NativeOverlapped* nativeOverlappedPtr);
    public unsafe NativeOverlapped* Pack(IOCompletionCallback? iocb);
    public unsafe NativeOverlapped* Pack(IOCompletionCallback? iocb, object? userData);
    public unsafe static Overlapped Unpack(NativeOverlapped* nativeOverlappedPtr);
    public unsafe NativeOverlapped* UnsafePack(IOCompletionCallback? iocb);
    public unsafe NativeOverlapped* UnsafePack(IOCompletionCallback? iocb, object? userData);
  }
  public delegate void ParameterizedThreadStart(object? obj);
  public sealed class PreAllocatedOverlapped : IDisposable {
    public PreAllocatedOverlapped(IOCompletionCallback callback, object? state, object? pinData);
    public void Dispose();
    ~PreAllocatedOverlapped();
  }
  public sealed class ReaderWriterLock : CriticalFinalizerObject {
    public ReaderWriterLock();
    public bool IsReaderLockHeld { get; }
    public bool IsWriterLockHeld { get; }
    public int WriterSeqNum { get; }
    public void AcquireReaderLock(int millisecondsTimeout);
    public void AcquireReaderLock(TimeSpan timeout);
    public void AcquireWriterLock(int millisecondsTimeout);
    public void AcquireWriterLock(TimeSpan timeout);
    public bool AnyWritersSince(int seqNum);
    public void DowngradeFromWriterLock(ref LockCookie lockCookie);
    public LockCookie ReleaseLock();
    public void ReleaseReaderLock();
    public void ReleaseWriterLock();
    public void RestoreLock(ref LockCookie lockCookie);
    public LockCookie UpgradeToWriterLock(int millisecondsTimeout);
    public LockCookie UpgradeToWriterLock(TimeSpan timeout);
  }
  public class ReaderWriterLockSlim : IDisposable {
    public ReaderWriterLockSlim();
    public ReaderWriterLockSlim(LockRecursionPolicy recursionPolicy);
    public int CurrentReadCount { get; }
    public bool IsReadLockHeld { get; }
    public bool IsUpgradeableReadLockHeld { get; }
    public bool IsWriteLockHeld { get; }
    public LockRecursionPolicy RecursionPolicy { get; }
    public int RecursiveReadCount { get; }
    public int RecursiveUpgradeCount { get; }
    public int RecursiveWriteCount { get; }
    public int WaitingReadCount { get; }
    public int WaitingUpgradeCount { get; }
    public int WaitingWriteCount { get; }
    public void Dispose();
    public void EnterReadLock();
    public void EnterUpgradeableReadLock();
    public void EnterWriteLock();
    public void ExitReadLock();
    public void ExitUpgradeableReadLock();
    public void ExitWriteLock();
    public bool TryEnterReadLock(int millisecondsTimeout);
    public bool TryEnterReadLock(TimeSpan timeout);
    public bool TryEnterUpgradeableReadLock(int millisecondsTimeout);
    public bool TryEnterUpgradeableReadLock(TimeSpan timeout);
    public bool TryEnterWriteLock(int millisecondsTimeout);
    public bool TryEnterWriteLock(TimeSpan timeout);
  }
  public sealed class RegisteredWaitHandle : MarshalByRefObject {
    public bool Unregister(WaitHandle? waitObject);
  }
  public sealed class Semaphore : WaitHandle {
    public Semaphore(int initialCount, int maximumCount);
    public Semaphore(int initialCount, int maximumCount, string? name);
    public Semaphore(int initialCount, int maximumCount, string? name, out bool createdNew);
    public static Semaphore OpenExisting(string name);
    public int Release();
    public int Release(int releaseCount);
    public static bool TryOpenExisting(string name, out Semaphore? result);
  }
  public class SemaphoreFullException : SystemException {
    public SemaphoreFullException();
    protected SemaphoreFullException(SerializationInfo info, StreamingContext context);
    public SemaphoreFullException(string? message);
    public SemaphoreFullException(string? message, Exception? innerException);
  }
  public class SemaphoreSlim : IDisposable {
    public SemaphoreSlim(int initialCount);
    public SemaphoreSlim(int initialCount, int maxCount);
    public WaitHandle AvailableWaitHandle { get; }
    public int CurrentCount { get; }
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public int Release();
    public int Release(int releaseCount);
    public void Wait();
    public bool Wait(int millisecondsTimeout);
    public bool Wait(int millisecondsTimeout, CancellationToken cancellationToken);
    public void Wait(CancellationToken cancellationToken);
    public bool Wait(TimeSpan timeout);
    public bool Wait(TimeSpan timeout, CancellationToken cancellationToken);
    public Task WaitAsync();
    public Task<bool> WaitAsync(int millisecondsTimeout);
    public Task<bool> WaitAsync(int millisecondsTimeout, CancellationToken cancellationToken);
    public Task WaitAsync(CancellationToken cancellationToken);
    public Task<bool> WaitAsync(TimeSpan timeout);
    public Task<bool> WaitAsync(TimeSpan timeout, CancellationToken cancellationToken);
  }
  public delegate void SendOrPostCallback(object? state);
  public struct SpinLock {
    public SpinLock(bool enableThreadOwnerTracking);
    public bool IsHeld { get; }
    public bool IsHeldByCurrentThread { get; }
    public bool IsThreadOwnerTrackingEnabled { get; }
    public void Enter(ref bool lockTaken);
    public void Exit();
    public void Exit(bool useMemoryBarrier);
    public void TryEnter(ref bool lockTaken);
    public void TryEnter(int millisecondsTimeout, ref bool lockTaken);
    public void TryEnter(TimeSpan timeout, ref bool lockTaken);
  }
  public struct SpinWait {
    public int Count { get; }
    public bool NextSpinWillYield { get; }
    public void Reset();
    public void SpinOnce();
    public void SpinOnce(int sleep1Threshold);
    public static void SpinUntil(Func<bool> condition);
    public static bool SpinUntil(Func<bool> condition, int millisecondsTimeout);
    public static bool SpinUntil(Func<bool> condition, TimeSpan timeout);
  }
  public class SynchronizationContext {
    public SynchronizationContext();
    public static SynchronizationContext? Current { get; }
    public virtual SynchronizationContext CreateCopy();
    public bool IsWaitNotificationRequired();
    public virtual void OperationCompleted();
    public virtual void OperationStarted();
    public virtual void Post(SendOrPostCallback d, object? state);
    public virtual void Send(SendOrPostCallback d, object? state);
    public static void SetSynchronizationContext(SynchronizationContext? syncContext);
    protected void SetWaitNotificationRequired();
    public virtual int Wait(IntPtr[] waitHandles, bool waitAll, int millisecondsTimeout);
    protected static int WaitHelper(IntPtr[] waitHandles, bool waitAll, int millisecondsTimeout);
  }
  public class SynchronizationLockException : SystemException {
    public SynchronizationLockException();
    protected SynchronizationLockException(SerializationInfo info, StreamingContext context);
    public SynchronizationLockException(string? message);
    public SynchronizationLockException(string? message, Exception? innerException);
  }
  public sealed class Thread : CriticalFinalizerObject {
    public Thread(ParameterizedThreadStart start);
    public Thread(ParameterizedThreadStart start, int maxStackSize);
    public Thread(ThreadStart start);
    public Thread(ThreadStart start, int maxStackSize);
    public ApartmentState ApartmentState { get; set; }
    public CultureInfo CurrentCulture { get; set; }
    public static IPrincipal? CurrentPrincipal { get; set; }
    public static Thread CurrentThread { get; }
    public CultureInfo CurrentUICulture { get; set; }
    public ExecutionContext? ExecutionContext { get; }
    public bool IsAlive { get; }
    public bool IsBackground { get; set; }
    public bool IsThreadPoolThread { get; }
    public int ManagedThreadId { get; }
    public string? Name { get; set; }
    public ThreadPriority Priority { get; set; }
    public ThreadState ThreadState { get; }
    public void Abort();
    public void Abort(object? stateInfo);
    public static LocalDataStoreSlot AllocateDataSlot();
    public static LocalDataStoreSlot AllocateNamedDataSlot(string name);
    public static void BeginCriticalRegion();
    public static void BeginThreadAffinity();
    public void DisableComObjectEagerCleanup();
    public static void EndCriticalRegion();
    public static void EndThreadAffinity();
    ~Thread();
    public static void FreeNamedDataSlot(string name);
    public ApartmentState GetApartmentState();
    public CompressedStack GetCompressedStack();
    public static int GetCurrentProcessorId();
    public static object? GetData(LocalDataStoreSlot slot);
    public static AppDomain GetDomain();
    public static int GetDomainID();
    public override int GetHashCode();
    public static LocalDataStoreSlot GetNamedDataSlot(string name);
    public void Interrupt();
    public void Join();
    public bool Join(int millisecondsTimeout);
    public bool Join(TimeSpan timeout);
    public static void MemoryBarrier();
    public static void ResetAbort();
    public void Resume();
    public void SetApartmentState(ApartmentState state);
    public void SetCompressedStack(CompressedStack stack);
    public static void SetData(LocalDataStoreSlot slot, object? data);
    public static void Sleep(int millisecondsTimeout);
    public static void Sleep(TimeSpan timeout);
    public static void SpinWait(int iterations);
    public void Start();
    public void Start(object? parameter);
    public void Suspend();
    public bool TrySetApartmentState(ApartmentState state);
    public static byte VolatileRead(ref byte address);
    public static double VolatileRead(ref double address);
    public static short VolatileRead(ref short address);
    public static int VolatileRead(ref int address);
    public static long VolatileRead(ref long address);
    public static IntPtr VolatileRead(ref IntPtr address);
    public static object? VolatileRead(ref object? address);
    public static sbyte VolatileRead(ref sbyte address);
    public static float VolatileRead(ref float address);
    public static ushort VolatileRead(ref ushort address);
    public static uint VolatileRead(ref uint address);
    public static ulong VolatileRead(ref ulong address);
    public static UIntPtr VolatileRead(ref UIntPtr address);
    public static void VolatileWrite(ref byte address, byte value);
    public static void VolatileWrite(ref double address, double value);
    public static void VolatileWrite(ref short address, short value);
    public static void VolatileWrite(ref int address, int value);
    public static void VolatileWrite(ref long address, long value);
    public static void VolatileWrite(ref IntPtr address, IntPtr value);
    public static void VolatileWrite(ref object? address, object? value);
    public static void VolatileWrite(ref sbyte address, sbyte value);
    public static void VolatileWrite(ref float address, float value);
    public static void VolatileWrite(ref ushort address, ushort value);
    public static void VolatileWrite(ref uint address, uint value);
    public static void VolatileWrite(ref ulong address, ulong value);
    public static void VolatileWrite(ref UIntPtr address, UIntPtr value);
    public static bool Yield();
  }
  public sealed class ThreadAbortException : SystemException {
    public object? ExceptionState { get; }
  }
  public class ThreadExceptionEventArgs : EventArgs {
    public ThreadExceptionEventArgs(Exception t);
    public Exception Exception { get; }
  }
  public delegate void ThreadExceptionEventHandler(object sender, ThreadExceptionEventArgs e);
  public class ThreadInterruptedException : SystemException {
    public ThreadInterruptedException();
    protected ThreadInterruptedException(SerializationInfo info, StreamingContext context);
    public ThreadInterruptedException(string? message);
    public ThreadInterruptedException(string? message, Exception? innerException);
  }
  public class ThreadLocal<T> : IDisposable {
    public ThreadLocal();
    public ThreadLocal(bool trackAllValues);
    public ThreadLocal(Func<T> valueFactory);
    public ThreadLocal(Func<T> valueFactory, bool trackAllValues);
    public bool IsValueCreated { get; }
    public T Value { get; set; }
    public IList<T> Values { get; }
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    ~ThreadLocal();
    public override string? ToString();
  }
  public static class ThreadPool {
    public static long CompletedWorkItemCount { get; }
    public static long PendingWorkItemCount { get; }
    public static int ThreadCount { get; }
    public static bool BindHandle(IntPtr osHandle);
    public static bool BindHandle(SafeHandle osHandle);
    public static void GetAvailableThreads(out int workerThreads, out int completionPortThreads);
    public static void GetMaxThreads(out int workerThreads, out int completionPortThreads);
    public static void GetMinThreads(out int workerThreads, out int completionPortThreads);
    public static bool QueueUserWorkItem(WaitCallback callBack);
    public static bool QueueUserWorkItem(WaitCallback callBack, object? state);
    public static bool QueueUserWorkItem<TState>(Action<TState> callBack, TState state, bool preferLocal);
    public static RegisteredWaitHandle RegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, int millisecondsTimeOutInterval, bool executeOnlyOnce);
    public static RegisteredWaitHandle RegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, long millisecondsTimeOutInterval, bool executeOnlyOnce);
    public static RegisteredWaitHandle RegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, TimeSpan timeout, bool executeOnlyOnce);
    public static RegisteredWaitHandle RegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, uint millisecondsTimeOutInterval, bool executeOnlyOnce);
    public static bool SetMaxThreads(int workerThreads, int completionPortThreads);
    public static bool SetMinThreads(int workerThreads, int completionPortThreads);
    public unsafe static bool UnsafeQueueNativeOverlapped(NativeOverlapped* overlapped);
    public static bool UnsafeQueueUserWorkItem(IThreadPoolWorkItem callBack, bool preferLocal);
    public static bool UnsafeQueueUserWorkItem(WaitCallback callBack, object? state);
    public static bool UnsafeQueueUserWorkItem<TState>(Action<TState> callBack, TState state, bool preferLocal);
    public static RegisteredWaitHandle UnsafeRegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, int millisecondsTimeOutInterval, bool executeOnlyOnce);
    public static RegisteredWaitHandle UnsafeRegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, long millisecondsTimeOutInterval, bool executeOnlyOnce);
    public static RegisteredWaitHandle UnsafeRegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, TimeSpan timeout, bool executeOnlyOnce);
    public static RegisteredWaitHandle UnsafeRegisterWaitForSingleObject(WaitHandle waitObject, WaitOrTimerCallback callBack, object? state, uint millisecondsTimeOutInterval, bool executeOnlyOnce);
  }
  public sealed class ThreadPoolBoundHandle : IDisposable {
    public SafeHandle Handle { get; }
    public unsafe NativeOverlapped* AllocateNativeOverlapped(IOCompletionCallback callback, object? state, object? pinData);
    public unsafe NativeOverlapped* AllocateNativeOverlapped(PreAllocatedOverlapped preAllocated);
    public static ThreadPoolBoundHandle BindHandle(SafeHandle handle);
    public void Dispose();
    public unsafe void FreeNativeOverlapped(NativeOverlapped* overlapped);
    public unsafe static object? GetNativeOverlappedState(NativeOverlapped* overlapped);
  }
  public enum ThreadPriority {
    AboveNormal = 3,
    BelowNormal = 1,
    Highest = 4,
    Lowest = 0,
    Normal = 2,
  }
  public delegate void ThreadStart();
  public sealed class ThreadStartException : SystemException {
  }
  public enum ThreadState {
    Aborted = 256,
    AbortRequested = 128,
    Background = 4,
    Running = 0,
    Stopped = 16,
    StopRequested = 1,
    Suspended = 64,
    SuspendRequested = 2,
    Unstarted = 8,
    WaitSleepJoin = 32,
  }
  public class ThreadStateException : SystemException {
    public ThreadStateException();
    protected ThreadStateException(SerializationInfo info, StreamingContext context);
    public ThreadStateException(string? message);
    public ThreadStateException(string? message, Exception? innerException);
  }
  public sealed class Timer : MarshalByRefObject, IAsyncDisposable, IDisposable {
    public Timer(TimerCallback callback);
    public Timer(TimerCallback callback, object? state, int dueTime, int period);
    public Timer(TimerCallback callback, object? state, long dueTime, long period);
    public Timer(TimerCallback callback, object? state, TimeSpan dueTime, TimeSpan period);
    public Timer(TimerCallback callback, object? state, uint dueTime, uint period);
    public bool Change(int dueTime, int period);
    public bool Change(long dueTime, long period);
    public bool Change(TimeSpan dueTime, TimeSpan period);
    public bool Change(uint dueTime, uint period);
    public void Dispose();
    public bool Dispose(WaitHandle notifyObject);
    public ValueTask DisposeAsync();
  }
  public delegate void TimerCallback(object? state);
  public static class Volatile {
    public static bool Read(ref bool location);
    public static byte Read(ref byte location);
    public static double Read(ref double location);
    public static short Read(ref short location);
    public static int Read(ref int location);
    public static long Read(ref long location);
    public static IntPtr Read(ref IntPtr location);
    public static sbyte Read(ref sbyte location);
    public static float Read(ref float location);
    public static ushort Read(ref ushort location);
    public static uint Read(ref uint location);
    public static ulong Read(ref ulong location);
    public static UIntPtr Read(ref UIntPtr location);
    public static T Read<T>(ref T location) where T : class?;
    public static void Write(ref bool location, bool value);
    public static void Write(ref byte location, byte value);
    public static void Write(ref double location, double value);
    public static void Write(ref short location, short value);
    public static void Write(ref int location, int value);
    public static void Write(ref long location, long value);
    public static void Write(ref IntPtr location, IntPtr value);
    public static void Write(ref sbyte location, sbyte value);
    public static void Write(ref float location, float value);
    public static void Write(ref ushort location, ushort value);
    public static void Write(ref uint location, uint value);
    public static void Write(ref ulong location, ulong value);
    public static void Write(ref UIntPtr location, UIntPtr value);
    public static void Write<T>(ref T location, T value) where T : class?;
  }
  public delegate void WaitCallback(object? state);
  public class WaitHandleCannotBeOpenedException : ApplicationException {
    public WaitHandleCannotBeOpenedException();
    protected WaitHandleCannotBeOpenedException(SerializationInfo info, StreamingContext context);
    public WaitHandleCannotBeOpenedException(string? message);
    public WaitHandleCannotBeOpenedException(string? message, Exception? innerException);
  }
  public delegate void WaitOrTimerCallback(object? state, bool timedOut);
 }
 namespace System.Threading.Tasks {
  public class ConcurrentExclusiveSchedulerPair {
    public ConcurrentExclusiveSchedulerPair();
    public ConcurrentExclusiveSchedulerPair(TaskScheduler taskScheduler);
    public ConcurrentExclusiveSchedulerPair(TaskScheduler taskScheduler, int maxConcurrencyLevel);
    public ConcurrentExclusiveSchedulerPair(TaskScheduler taskScheduler, int maxConcurrencyLevel, int maxItemsPerTask);
    public Task Completion { get; }
    public TaskScheduler ConcurrentScheduler { get; }
    public TaskScheduler ExclusiveScheduler { get; }
    public void Complete();
  }
  public static class TaskAsyncEnumerableExtensions {
    public static ConfiguredAsyncDisposable ConfigureAwait(this IAsyncDisposable source, bool continueOnCapturedContext);
    public static ConfiguredCancelableAsyncEnumerable<T> ConfigureAwait<T>(this IAsyncEnumerable<T> source, bool continueOnCapturedContext);
    public static ConfiguredCancelableAsyncEnumerable<T> WithCancellation<T>(this IAsyncEnumerable<T> source, CancellationToken cancellationToken);
  }
  public class TaskCanceledException : OperationCanceledException {
    public TaskCanceledException();
    protected TaskCanceledException(SerializationInfo info, StreamingContext context);
    public TaskCanceledException(string? message);
    public TaskCanceledException(string? message, Exception? innerException);
    public TaskCanceledException(string? message, Exception? innerException, CancellationToken token);
    public TaskCanceledException(Task? task);
    public Task? Task { get; }
  }
  public class TaskCompletionSource<TResult> {
    public TaskCompletionSource();
    public TaskCompletionSource(object? state);
    public TaskCompletionSource(object? state, TaskCreationOptions creationOptions);
    public TaskCompletionSource(TaskCreationOptions creationOptions);
    public Task<TResult> Task { get; }
    public void SetCanceled();
    public void SetException(IEnumerable<Exception> exceptions);
    public void SetException(Exception exception);
    public void SetResult(TResult result);
    public bool TrySetCanceled();
    public bool TrySetCanceled(CancellationToken cancellationToken);
    public bool TrySetException(IEnumerable<Exception> exceptions);
    public bool TrySetException(Exception exception);
    public bool TrySetResult(TResult result);
  }
  public static class TaskExtensions {
    public static Task Unwrap(this Task<Task> task);
    public static Task<TResult> Unwrap<TResult>(this Task<Task<TResult>> task);
  }
  public class TaskSchedulerException : Exception {
    public TaskSchedulerException();
    public TaskSchedulerException(Exception? innerException);
    protected TaskSchedulerException(SerializationInfo info, StreamingContext context);
    public TaskSchedulerException(string? message);
    public TaskSchedulerException(string? message, Exception? innerException);
  }
 }
```
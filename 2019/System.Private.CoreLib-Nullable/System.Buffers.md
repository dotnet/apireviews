# System.Buffers

```diff
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Buffers\before\System.Buffers.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Buffers\after\System.Buffers.dll
 namespace System.Buffers {
  public abstract class ArrayPool<T> {
    protected ArrayPool();
    public static ArrayPool<T> Shared { get; }
    public static ArrayPool<T> Create();
    public static ArrayPool<T> Create(int maxArrayLength, int maxArraysPerBucket);
    public abstract T[] Rent(int minimumLength);
    public abstract void Return(T[] array, bool clearArray = false);
  }
 }
```

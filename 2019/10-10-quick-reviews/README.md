# Quick Reviews 10/10/2019

## Add file and directory creation methods that take an ACL

**Approved** | [#corefx/41614](https://github.com/dotnet/corefx/issues/41614#issuecomment-540773661) | [Video](https://www.youtube.com/watch?v=7KTxd_41Lus&t=0h0m0s)

We considered adding the APIs back to the relevant types (i.e. as constructors). While this would be preferred from a porting/backwards compabitility case, it would mean we'd have to pull-in a large amount of ACL / Windows security into `System.Private.Corelib`.

We think these APIs are weird and shouldn't be exposed:

```C#
namespace System.IO
{
    public static class FileSystemAclExtensions
    {
        public static FileStream CreateFile(
            this FileSecurity fileSecurity,
            string path,
            FileMode mode,
            FileSystemRights rights,
            FileShare share,
            int bufferSize,
            FileOptions options);

        public static DirectoryInfo CreateDirectory(
            this DirectorySecurity directorySecurity
            string path);
    }
 }
```

These APIs look fine (assuming they match the existing instance methods of .NET Framework in terms of naming and order):

```C#
namespace System.IO
{
    public static class FileSystemAclExtensions
    {
        // Add
        public static FileStream Create(
            this FileInfo fileInfo,
            FileMode mode,
            FileSystemRights rights,
            FileShare share,
            int bufferSize,
            FileOptions options,
            FileSecurity fileSecurity);

        public static void Create(
            this DirectoryInfo directoryInfo,
            DirectorySecurity directorySecurity);
    }
}
```
## Port `MemoryMappedFileSecurity` and add extensions for `MemoryMappedFile`

**Approved** | [#corefx/41654](https://github.com/dotnet/corefx/issues/41654#issuecomment-540781324) | [Video](https://www.youtube.com/watch?v=7KTxd_41Lus&t=0h17m27s)

We should introduce a new static type in the same assembly as `FileSystemAclExtensions`. The pattern we settled on was suffixing the type with `Acl`.

@JeremyKuhne: we need to update the ordering to match the existing APIs. Please update this comment.

```C#
namespace System.IO.MemoryMappedFiles
{
    public static class MemoryMappedFileAcl
    {
        public static MemoryMappedFile CreateFromFile(
            MemoryMappedFileSecurity memoryMappedFileSecurity,
            FileStream fileStream,
            string mapName,
            long capacity,
            MemoryMappedFileAccess access,
            HandleInheritability inheritability,
            bool leaveOpen);

        public static MemoryMappedFile CreateNew(
            MemoryMappedFileSecurity memoryMappedFileSecurity,
            string mapName,
            long capacity,
            MemoryMappedFileAccess access,
            MemoryMappedFileOptions options,
            HandleInheritability inheritability);

        public static MemoryMappedFile CreateOrOpenMappedFile(
            MemoryMappedFileSecurity memoryMappedFileSecurity,
            string mapName,
            long capacity,
            MemoryMappedFileAccess access,
            MemoryMappedFileOptions options,
            HandleInheritability inheritability);

        // Those are fine as extensions:

        public static MemoryMappedFileSecurity GetAccessControl(
            this MemoryMappedFile memoryMappedFile);

        public static void SetAccessControl(
            this MemoryMappedFile memoryMappedFile,
            MemoryMappedFileSecurity memoryMappedFileSecurity);
    }
}
```
## Add pipe creation extension methods that take an ACL

**Approved** | [#corefx/41657](https://github.com/dotnet/corefx/issues/41657#issuecomment-540786760) | [Video](https://www.youtube.com/watch?v=7KTxd_41Lus&t=0h31m52s)

We should not make them extension methods. We should follow the other pattern and create new static types suffixed with `Acl`:

@JeremyKuhne: we need to update the ordering to match the existing APIs. Please update this comment.

```C#
namespace System.IO.Pipes
{
    public static class AnonymousPipeServerStreamAcl
    {
        public AnonymousPipeServerStream Create(
            PipeSecurity pipeSecurity
            PipeDirection direction,
            HandleInheritability inheritability,
            int bufferSize);
    }

    public static class NamedPipeServerStreamAcl
    {
        public NamedPipeServerStream Create(
            PipeSecurity pipeSecurity
            string pipeName,
            PipeDirection direction,
            int maxNumberOfServerInstances,
            PipeTransmissionMode transmissionMode,
            PipeOptions options,
            int inBufferSize,
            int outBufferSize,
            HandleInheritability inheritability = HandleInheritability.None,
            PipeAccessRights additionalAccessRights = default);
    }
}
```
## Add Mutex, Semaphore, and EventWaitHandle creation extension methods that take an ACL

**Approved** | [#corefx/41662](https://github.com/dotnet/corefx/issues/41662#issuecomment-540790209) | [Video](https://www.youtube.com/watch?v=7KTxd_41Lus&t=0h41m46s)

We should not make them extension methods. We should follow the other pattern and create new static types suffixed with `Acl`:

@JeremyKuhne: we need to update the ordering to match the existing APIs. Please update this comment.

```C#
namespace System.Threading
{
    public static class EventWaitHandleAcl
    {   
        public static EventWaitHandle Create(
            EventWaitHandleSecurity eventSecurity
            bool initialState,
            EventResetMode mode,
            string name,
            out bool createdNew);
    }

    public static class MutexAcl
    {   
        public static Mutex Create(
            MutexSecurity mutexSecurity,
            bool initiallyOwned,
            string name,
            out bool createdNew);
    }

    public static class SemaphoreAcl
    {   
        public static Semaphore Create(
            SemaphoreSecurity semaphoreSecurity,
            int initialCount,
            int maximumCount,
            string name,
            out bool createdNew);
    }
}
```
## Readonly members in Drawing primitives 

**Approved** | [#corefx/40407](https://github.com/dotnet/corefx/pull/40407#issuecomment-540800566) | [Video](https://www.youtube.com/watch?v=7KTxd_41Lus&t=0h50m12s)

This seems reasonable to be merged (for 5.0, we don't believe this makes the bar for 3.1). We probably want to do this piecemeal, like we do for nullable. We already have a tracking issue to do a pass (#36586). /cc @tannergooding 

This also seems like a good idea for an analyzer that can suggest adding the `readonly` modifier. /cc @stephentoub 



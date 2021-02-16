# Quick Reviews 02/16/2021

## Add runtime API to load Hot Reload deltas

**Approved** | [#runtime/45689](https://github.com/dotnet/runtime/issues/45689#issuecomment-780028743) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=0h0m0s)

* It's not a mainline API, but it's logically similar to the existing `TryGetRawMetadata()`
* However, we don't believe this should be an extension method because most people would never need it

```C#
namespace System.Reflection.Metadata
{
    public static partial class AssemblyExtensions
    {
        public static void ApplyUpdate(Assembly assembly, ReadOnlySpan<byte> metadataDelta, ReadOnlySpan<byte> ilDelta, ReadOnlySpan<byte> pdbDelta = default);
    }
}
```

## Provide a way to get a PipeReader from a `ReadOnlySequence<byte>`

**Approved** | [#runtime/47990](https://github.com/dotnet/runtime/issues/47990#issuecomment-780034307) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=0h17m51s)

* We could also overloads to create a reader straight from a `byte[]` or `ReadOnlyMemory<byte>` but those can be constructed by creating a sequence, so we can add them later if we think that's common enough.
* We don't believe this method needs any options

```C#
namespace System.IO.Pipelines
{
    public abstract class PipeReader
    {
        public static PipeReader Create(ReadOnlySequence<byte> sequence);
    }
}
```

## Improve performance of Rfc2898DeriveBytes

**Approved** | [#runtime/24897](https://github.com/dotnet/runtime/issues/24897#issuecomment-780044973) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=0h26m43s)

* Let's drop the name `DeriveBytes` from the method name because it doesn't really add anything
* We considered a separate type but it seems the difference is streaming vs non-streaming and static vs non-static seems to convey is sufficiently.

```C#
namespace System.Security.Cryptography
{
    public partial class Rfc2898DeriveBytes
    {
        public static byte[] Pbkdf2(
            byte[] password,
            byte[] salt,
            int iterations,
            HashAlgorithmName hashAlgorithm,
            int outputLength);

        public static byte[] Pbkdf2(
            ReadOnlySpan<byte> password,
            ReadOnlySpan<byte> salt,
            int iterations,
            HashAlgorithmName hashAlgorithm,
            int outputLength);

        public static void Pbkdf2(
            ReadOnlySpan<byte> password,
            ReadOnlySpan<byte> salt,
            Span<byte> destination,
            int iterations,
            HashAlgorithmName hashAlgorithm);

        public static byte[] Pbkdf2(
            string password,
            byte[] salt,
            int iterations,
            HashAlgorithmName hashAlgorithm,
            int outputLength);

        public static byte[] Pbkdf2(
            ReadOnlySpan<char> password,
            ReadOnlySpan<byte> salt,
            int iterations,
            HashAlgorithmName hashAlgorithm,
            int outputLength);

        public static void Pbkdf2(
            ReadOnlySpan<char> password,
            ReadOnlySpan<byte> salt,
            Span<byte> destination,
            int iterations,
            HashAlgorithmName hashAlgorithm);
    }
}
```

## Allow setting the Activity properties after Activity creation and before starting it when using ActivitySource

**Approved** | [#runtime/42784](https://github.com/dotnet/runtime/issues/42784#issuecomment-780052959) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=0h45m30s)

* We should add a simplified overload that only requires name and kind
* The proposed name is sufficiently clear

```C#
namespace System.Diagnostics
{
    public sealed class ActivitySource : IDisposable
    {
        public Activity? CreateActivity(string name, 
                                        ActivityKind kind);

        public Activity? CreateActivity(string name,
                                        ActivityKind kind, 
                                        ActivityContext parentContext,
                                        IEnumerable<KeyValuePair<string, object?>>? tags = null, 
                                        IEnumerable<ActivityLink>? links = null);

        public Activity? CreateActivity(string name, 
                                        ActivityKind kind,
                                        string parentContext,
                                        IEnumerable<KeyValuePair<string, object?>>? tags = null, 
                                        IEnumerable<ActivityLink>? links = null);
    }
}
```

## Impossible to set Activity.IdFormat when using ActivitySource

**Rejected** | [#runtime/43853](https://github.com/dotnet/runtime/issues/43853#issuecomment-780060001) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=0h59m28s)

* We don't need this API because we just added `CreateActivity`

```C#
namespace System.Diagnostics
{
    public sealed class ActivitySource : IDisposable
    {
        public Activity? StartActivity(ActivityKind kind, 
                                       ActivityContext parentContext = default, 
                                       IEnumerable<KeyValuePair<string, object?>>? tags = null, 
                                       IEnumerable<ActivityLink>? links = null, 
                                       ActivityIdFormat idFormat = ActivityIdFormat.Unknown,
                                       DateTimeOffset startTime = default,
                                       [CallerMemberName] string name = "");
    }
}
```

## Make it safer and easier to build an X500DistinguishedName

**Approved** | [#runtime/44738](https://github.com/dotnet/runtime/issues/44738#issuecomment-780064442) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=1h12m57s)

* Let's just drop the nested type and replace it with an `int`. It'a a power API and users likely end up hard casting the `Asn1` enums.

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public sealed partial class X500DistinguishedNameBuilder
    {
        public void AddEmailAddress(string emailAddress);
        public void AddDomainComponent(string domainComponent);
        public void AddLocalityName(string localityName);
        public void AddCommonName(string commonName); 
        public void AddCountryOrRegion(string twoLetterCode);
        public void AddOrganizationName(string organizationName);
        public void AddOrganizationalUnitName(string organizationalUnitName);
        public void AddStateOrProvinceName(string stateOrProvinceName);
    
        public void Add(string oidValue, string value, int stringEncodingType = 0);
        public void Add(Oid oid, string value, int stringEncodingType = 0);
    
        public void AddEncoded(Oid oid, byte[] encodedValue);
        public void AddEncoded(Oid oid, ReadOnlySpan<byte> encodedValue);
        public void AddEncoded(string oidValue, byte[] encodedValue);
        public void AddEncoded(string oidValue, ReadOnlySpan<byte> encodedValue);
    
        public X500DistinguishedName Build();    
    }
}
```

## Developers can have access to more options when configuring async awaitables

**NeedsWork** | [#runtime/47525](https://github.com/dotnet/runtime/issues/47525#issuecomment-780093604) | [Video](https://www.youtube.com/watch?v=MFUXZLyDqbU&t=1h21m3s)

* Let's make it readonly so that we can mark APIs as `in`
* We need to decide whether to have a type `AwaitBehavior` or whether we should use optional parameters
* It seems `ValueTask` and `Task` might differ in what they can support; the same might be true for other `IAsync` interfaces
* We may also want to add an analyzer to flag cancellation tokens that should have been past to the underlying operation instead

```C#
namespace System.Threading.Tasks
{
    public readonly struct AwaitBehavior
    {
        public CancellationToken CancellationToken { get; init; }
        public TimeSpan Timeout { get; init; }
        public bool ContinueOnCapturedContext { get; init; }
        public bool ForceAsync { get; init; }
        public bool SuppressExceptions { get; init; }
    }
    public partial class Task
    {
        public ConfiguredCancelableTaskAwaitable ConfigureAwait(AwaitBehavior awaitBehavior);
    }
    public partial class Task<TResult>
    {
        public new ConfiguredCancelableTaskAwaitable<TResult> ConfigureAwait(AwaitBehavior awaitBehavior);
    }
    public partial struct ValueTask
    {
        public ConfiguredCancelableValueTaskAwaitable ConfigureAwait(AwaitBehavior awaitBehavior);
    }
    public partial struct ValueTask<TResult>
    {
        public ConfiguredCancelableValueTaskAwaitable<TResult> ConfigureAwait(AwaitBehavior awaitBehavior);
    }
}
namespace System.Runtime.CompilerServices
{
    public readonly struct ConfiguredCancelableTaskAwaitable
    {
        public ConfiguredCancelableTaskAwaiter GetAwaiter();
        public readonly struct ConfiguredCancelableTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public void GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly struct ConfiguredCancelableTaskAwaitable<TResult>
    {
        public ConfiguredCancelableTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public TResult GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly partial struct ConfiguredCancelableValueTaskAwaitable
    {
        public ConfiguredCancelableValueTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public void GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
    public readonly partial struct ConfiguredCancelableValueTaskAwaitable<TResult>
    {
        public ConfiguredCancelableValueTaskAwaiter GetAwaiter();
        public readonly partial struct ConfiguredCancelableValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion
        {
            public bool IsCompleted { get; }
            public TResult GetResult();
            public void OnCompleted(Action continuation);
            public void UnsafeOnCompleted(Action continuation);
        }
    }
}
```


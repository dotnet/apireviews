# API Review 05/05/2022

## Introduce DbDataSource abstraction to System.Data

**Approved** | [#runtime/64812](https://github.com/dotnet/runtime/issues/64812#issuecomment-1118872279) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=0h0m0s)

* Should use the `Dispose` pattern with `Dispose(bool)` being virtual while the others aren't
    - And same for `DisposeAsync()`
* Maybe `GetConnection()` should be called `CreateConnection()` because it returns a new instance
* Consider introducing a term that represents connection who logically don't have a connection and rename `CreateCommand()`
    - For example `CreateLightweightCommand()`
    - The key is: consumers should be aware that this method isn't a convenience for `OpenConnection().CreateCommand()`.

```C#
public abstract class DbDataSource : IDisposable, IAsyncDisposable
{
    public DbConnection GetConnection();
    public DbConnection OpenConnection();
    public ValueTask<DbConnection> OpenConnectionAsync();

    public DbCommand CreateCommand(string? commandText = null);
    public DbBatch CreateBatch();

    public virtual void Dispose();
    public virtual ValueTask DisposeAsync();

    public abstract string ConnectionString { get; }

    protected abstract DbConnection GetDbConnection();
    protected virtual DbConnection OpenDbConnection();
    protected virtual async ValueTask<DbConnection> OpenDbConnectionAsync(CancellationToken cancellationToken = default);

    protected virtual DbCommand CreateDbCommand(string? commandText = null);
    protected virtual DbBatch CreateDbBatch();
}
```
## Support ref fields in System.Reflection.Metadata.Ecma335.BlobEncoder

**Approved** | [#runtime/68309](https://github.com/dotnet/runtime/issues/68309#issuecomment-1118883135) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=0h33m55s)

* Let's call the method `BlobEncoder` just `Field`

```C#
namespace System.Reflection.Metadata.Ecma335;

public partial readonly struct BlobEncoder
{
    public FieldTypeEncoder Field();
}

public readonly struct FieldTypeEncoder
{
    public BlobBuilder Builder { get; }

    public FieldTypeEncoder(BlobBuilder builder);
    public CustomModifiersEncoder CustomModifiers();
    public SignatureTypeEncoder Type(bool isByRef = false);
    public void TypedReference();
}
```
## System.Diagnostics.ActivityLink/ActivityEvent: Enumeration API

**Approved** | [#runtime/68056](https://github.com/dotnet/runtime/issues/68056#issuecomment-1118889468) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=0h47m26s)

Looks good as proposed

```C#
namespace System.Diagnostics;

partial struct ActivityLink
{
    public Activity.Enumerator<KeyValuePair<string, object?>> EnumerateTagObjects();
}

partial struct ActivityEvent
{
    public Activity.Enumerator<KeyValuePair<string, object?>> EnumerateTagObjects();
}
```
## add overloads to support IAsyncEnumerable with source-gen

**Approved** | [#runtime/59268](https://github.com/dotnet/runtime/issues/59268#issuecomment-1118891931) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=0h54m45s)

Looks good as proposed

```C#
namespace System.Text.Json
{
     public partial static class JsonSerializer
     {
        // Existing
        // public static IAsyncEnumerable<TValue?> DeserializeAsyncEnumerable<TValue>(Stream utf8Json, JsonSerializerOptions options, CancellationToken cancellationToken = default);
        public static IAsyncEnumerable<TValue?> DeserializeAsyncEnumerable<TValue>(Stream utf8Json, JsonTypeInfo<TValue> jsonTypeInfo, CancellationToken cancellationToken = default);
     }
}

namespace System.Text.Json.Serialization.Metadata
{
    [EditorBrowsable(EditorBrowsableState.Never)]
    public static class JsonMetadataServices
    {
        // Existing
        // public static JsonTypeInfo<TCollection> CreateIEnumerableInfo<TCollection, TElement>(JsonSerializerOptions options, JsonCollectionInfoValues<TCollection> collectionInfo) where TCollection : IEnumerable<TElement>;
        public static JsonTypeInfo<TAsyncEnumerable> CreateIAsyncEnumerableInfo<TAsyncEnumerable, TElement>(JsonSerializerOptions options, JsonCollectionInfoValues<TAsyncEnumerable> collectionInfo) where TAsyncEnumerable : IAsyncEnumerable<TElement>;
    }
}
```
## Support DateOnly and TimeOnly in JsonSerializer

**Approved** | [#runtime/53539](https://github.com/dotnet/runtime/issues/53539#issuecomment-1118896272) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=0h56m51s)

* Looks good as proposed
* First time we're splitting the reference assembly for `System.Text.Json` between .NET Standard 2.0 and .NET X, but we have confidence that we have validation to avoid making accidental breaks

```C#
namespace System.Text.Json.Serialization.Metadata;

public static partial class JsonMetadataServices
{
    public static JsonConverter<DateOnly> DateOnlyConverter { get; }
    public static JsonConverter<TimeOnly> TimeOnlyConverter { get; }
}
```
## Provide a version of Stream.Read that reads as much as possible

**Approved** | [#runtime/16598](https://github.com/dotnet/runtime/issues/16598#issuecomment-1118973589) | [Video](https://www.youtube.com/watch?v=G9uf3VWBzQk&t=1h6m35s)

After lots of discussion we arrived a this:

```C#
namespace System.IO;

public partial class Stream
{
    public void ReadExactly(byte[] buffer, int offset, int count);
    public void ReadExactly(Span<byte> buffer);

    public int ReadAtLeast(Span<byte> buffer, int minimumBytes, bool throwOnEndOfStream = true);
    public ValueTask<int> ReadAtLeastAsync(Memory<byte> buffer, int minimumBytes, bool throwOnEndOfStream = true, CancellationToken cancellationToken = default);

    public ValueTask ReadExactlyAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken = default);
    public ValueTask ReadExactlyAsync(Memory<byte> buffer, CancellationToken cancellationToken = default);
}
```

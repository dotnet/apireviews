# Quick Reviews 6/30/2020

## Async System.Data resultset and database schema APIs

**Approved** | [#runtime/38028](https://github.com/dotnet/runtime/issues/38028#issuecomment-651967965) | [Video](https://www.youtube.com/watch?v=5w9Cbgr1HJY&t=0h0m0s)

* Instead of adding an interface member, we'd prefer exposing a new virtual on `DbDataReader` directly
    - @ajcvickers will follow up to see why we had the extension method calling through the interface instead.
* We avoid having more than two optional parameters b/c we found in UX studies that optional parameters are confusing
    - We should mirror the sync version which has had three overloads
    - Sadly all three were virtual and no overload accepts null (thus can't be chained)
    - For consistency, we should follow this pattern.

```C#
namespace System.Data.Common
{
    public partial class DbConnection
    {
        public virtual Task<DataTable> GetSchemaAsync(CancellationToken cancellationToken = default);
        public virtual Task<DataTable> GetSchemaAsync(string collectionName, CancellationToken cancellationToken = default);
        public virtual Task<DataTable> GetSchemaAsync(string collectionName, string[] restrictions, CancellationToken cancellationToken = default);
    }

    public partial class DbDataReader
    {
        public virtual Task<DataTable> GetSchemaTableAsync(CancellationToken cancellationToken = default);
        public virtual Task<ReadOnlyCollection<DbColumn>> GetColumnSchemaAsync(CancellationToken cancellationToken = default);
    }
}
```

## Database-agnostic way to detect transient database errors

**Approved** | [#runtime/34817](https://github.com/dotnet/runtime/issues/34817#issuecomment-651987951) | [Video](https://www.youtube.com/watch?v=5w9Cbgr1HJY&t=0h25m54s)

* Makes sense as proposed.

```C#
namespace System.Data.Common
{
    public partial class DbException
    {
        public virtual bool IsTransient => false;
    }
}
```

## Introduce SqlState on DbException for standard cross-database errors

**Approved** | [#runtime/35601](https://github.com/dotnet/runtime/issues/35601#issuecomment-651990751) | [Video](https://www.youtube.com/watch?v=5w9Cbgr1HJY&t=1h4m47s)

* Makes sense as proposed.
* It seems if we had this, we could make the default implementation for `DbException.IsTransient` (#34817) check whether `SqlState` is not null and return true for transient SQLSTATE values.

```C#
namespace System.Data.Common
{
    public partial class DbException
    {
        public virtual string? SqlState { get; }
    }
}
```


# API Review 05/06/2025

## Expose Vector Dataype in SqlDbType

**Approved** | [#runtime/115148](https://github.com/dotnet/runtime/issues/115148#issuecomment-2852839590)

Looks good as proposed.  Approved via email (trivial addition)

```C#
namespace System.Data
{
    // Specifies the SQL Server data type.
    public enum SqlDbType
    {
        Vector = 36,
    }
}
```

# Quick Reviews 6/12/2018

## Make it easy for large StringBuilders to be written to TextWriters without making a large string 

**Approved** | [#corefx/30048](https://github.com/dotnet/corefx/issues/30048#issuecomment-396671773) | [Video](https://www.youtube.com/watch?v=CcocbzCjCxM&t=-2h-2m-53s)

Looks good as proposed. A few minor suggestions:

```csharp
class TextWriter {
    public virtual void Write(StringBuilder value);
    public virtual void WriteLine(StringBuilder value);
    public virtual Task WriteAsync(StringBuilder value, CancelationToken cancellationToken=default);
    public virtual Task WriteLineAsync(StringBuilder value, CancelationToken cancellationToken=default);
}
```

We should consider special casing `Write(object)` to check for `StringBuilder` to avoid going through `StringBuilder.ToString()`. Althought it seems we aren't special casing any other types but `IFormattable` today.
## Allow Dictionary<K,V>.Remove during enumeration

**Approved** | [#corefx/29979](https://github.com/dotnet/corefx/issues/29979) | [Video](https://www.youtube.com/watch?v=CcocbzCjCxM&t=0h27m56s)

## new System.ComponentModel.VersionConverter class

**Approved** | [#corefx/28594](https://github.com/dotnet/corefx/issues/28594#issuecomment-396690545) | [Video](https://www.youtube.com/watch?v=CcocbzCjCxM&t=1h19m29s)

Looks good as proposed.

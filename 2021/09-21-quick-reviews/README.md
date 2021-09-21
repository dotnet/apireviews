# API Review 09/21/2021

## Add new ObjectDisposedException constructor overload

**Approved** | [#runtime/51700](https://github.com/dotnet/runtime/issues/51700#issuecomment-924242084) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h0m0s)

We discussed the method pattern here, and debated for a bit on whether the parameters should be nullable and we concluded no.  Since we're early in the release we have time to learn that was wrong.  (And going "more nullable" is not breaking)

```C#
namespace System
{
    public partial class ObjectDisposedException
    {
        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
        [System.Diagnostics.StackTraceHidden]
        public static void Throw(System.Object instance) => throw null;
        [System.Diagnostics.StackTraceHidden]
        [System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute]
        public static void Throw(System.Type type) => throw null;
    }
}
```

One notable change from the PR state: We added StackTraceHidden to both methods.
## Add reciprocal and SinCos methods to IFloatingPoint

**Approved** | [#runtime/58607](https://github.com/dotnet/runtime/issues/58607#issuecomment-924245358) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h16m36s)

Looks good as proposed.

```diff
namespace System
{
    public interface IFloatingPoint<TSelf>
    {
+        (TSelf Sin, TSelf Cos) SinCos(TSelf x);
+        TSelf ReciprocalEstimate(TSelf x);
+        TSelf ReciprocalSqrtEstimate(TSelf x);
    }
}
```
## Obsolete thumbtacked AssemblyName properties 

**Approved** | [#runtime/59061](https://github.com/dotnet/runtime/issues/59061#issuecomment-924248304) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h19m10s)

Looks good as proposed (though we moved the obsoletions to the properties instead of the accessors

```diff
    public sealed partial class AssemblyName
    {
+       [Obsolete("AssemblyName.HashAlgorithm has been deprecated and is not supported.", DiagnosticId = next available)]
        public System.Configuration.Assemblies.AssemblyHashAlgorithm HashAlgorithm { get; set; }
+       [Obsolete("AssemblyName.ProcessorArchitecture has been deprecated and is not supported.", DiagnosticId = same as above)]
        public System.Reflection.ProcessorArchitecture ProcessorArchitecture { get; set; }
+       [Obsolete("AssemblyName.VersionCompatibility has been deprecated and is not supported.", DiagnosticId = same as above)]
        public System.Configuration.Assemblies.AssemblyVersionCompatibility VersionCompatibility { get; set;}
    }
```
## Additional ArgumentNullException.ThrowIfNull overloads

**Approved** | [#runtime/58047](https://github.com/dotnet/runtime/issues/58047#issuecomment-924251709) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h23m19s)

We think that the `void*` version makes sense, but the `Nullable<T>` version doesn't (any message associated with that error should be more complicated than the default).

```C#
namespace System
{
    public partial class ArgumentNullException : ArgumentException
    {
        /// <summary>Throws an <see cref="ArgumentNullException"/> if <paramref name="argument"/> is null.</summary>
        /// <param name="argument">The pointer argument to validate as non-null.</param>
        /// <param name="paramName">The name of the parameter with which <paramref name="argument"/> corresponds.</param>
        public static unsafe void ThrowIfNull(void* argument, [CallerArgumentExpression("argument")] string? paramName = null);
   }
}
```
## Add the ArchiveComment property for ZipArchive class

**Approved** | [#runtime/1547](https://github.com/dotnet/runtime/issues/1547#issuecomment-924273994) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h27m51s)

Since the spec says that there's always a comment (but it can be zero length) we don't think the properties should be nullable (null-returning).  The setters can certainly accept null to normalize to whatever the default we have historically done.  (string.Empty / "Built with .NET", whatever it is we do...)

If you don't want to do null-normalization, then remove the `[AllowNull]`s, but it's probably nice to do.

```diff
namespace System.IO.Compression
{
    public partial class ZipArchive : IDisposable
    {
        public ZipArchive(Stream stream);
        public ZipArchive(Stream stream, ZipArchiveMode mode);
        public ZipArchive(Stream stream, ZipArchiveMode mode, bool leaveOpen);
        public ZipArchive(Stream stream, ZipArchiveMode mode, bool leaveOpen, Encoding? entryNameEncoding);
+       [AllowNull]
+       public string Comment { get; set; }
        public ReadOnlyCollection<ZipArchiveEntry> Entries { get; }
        public ZipArchiveMode Mode { get; }
        public ZipArchiveEntry CreateEntry(string entryName);
        public ZipArchiveEntry CreateEntry(string entryName, CompressionLevel compressionLevel);
        public void Dispose();
        protected virtual void Dispose(bool disposing);
        public ZipArchiveEntry? GetEntry(string entryName);
    }
    public partial class ZipArchiveEntry
    {
        internal ZipArchiveEntry() { }
        public ZipArchive Archive { get ; }
+       [AllowNull]
+       public string Comment { get; set; }
        public long CompressedLength { get ; }
        public uint Crc32 { get; }
        public int ExternalAttributes { get; set; }
        public string FullName { get; }
        public DateTimeOffset LastWriteTime { get; set; }
        public long Length { get; }
        public string Name { get; }
        public void Delete();
        public Stream Open();
        public override string ToString();
    }
}
```
## Make it safer and easier to build an X500DistinguishedName

**Approved** | [#runtime/44738](https://github.com/dotnet/runtime/issues/44738#issuecomment-924277199) | [Video](https://www.youtube.com/watch?v=-t3ELCIJpZo&t=0h42m24s)

Clicking's hard, mm'kay?  :smile:

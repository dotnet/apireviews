# API Review 03/17/2026

## Unsafe evolution attributes

**Approved** | [#runtime/125134](https://github.com/dotnet/runtime/issues/125134#issuecomment-4076858068) | [Video](https://www.youtube.com/watch?v=pPprMPpKKfY&t=0h0m0s)

* Moved RequiresUnsafeAttribute to System.Diagnostics.CodeAnalysis.
* Added Field as an AttributeTarget for RequiresUnsafe, and we're open to it being left out or other targets being added in the future.
* Corrected MemorySafetyRulesAttribute.Version to be a get-only property

```csharp
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.Constructor | AttributeTargets.Event | AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Field, Inherited = false, AllowMultiple = false)]
    public sealed class RequiresUnsafeAttribute : Attribute { }
}

namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Module, Inherited = false, AllowMultiple = false)]
    public sealed class MemorySafetyRulesAttribute : Attribute
    {
        public MemorySafetyRulesAttribute(int version) { Version = version; }
        public int Version { get; }
    }
}
```
## [Process]: Start detached process

**NeedsWork** | [#runtime/124334](https://github.com/dotnet/runtime/issues/124334#issuecomment-4077031439) | [Video](https://www.youtube.com/watch?v=pPprMPpKKfY&t=0h44m59s)

The discussion on this made us seem like we're already regretting ProcessStartOptions, and maybe it should go back to ProcessStartInfo and be rethought from there.
## Tar: archive creation should allow dereferencing soft/hard links to same file

**NeedsWork** | [#runtime/74404](https://github.com/dotnet/runtime/issues/74404#issuecomment-4077297337) | [Video](https://www.youtube.com/watch?v=pPprMPpKKfY&t=1h9m47s)


* Removed TarCreateOptions, we can always add it later when it has more unique options.
* Created TarLinkStrategy to handle the ambiguities in the boolean nature of the symlink handling
  * Renamed properties to match
  * We got late suggestion to move this to System.IO, but we couldn't come up with a name before loss of quorum
* Renamed SoftLink to SymbolicLink for consistency

```csharp
namespace System.Formats.Tar;

// System.IO.LinkReplicationMode?
// System.IO.LinkPropoagationStrategy?
// etc?
public enum TarLinkStrategy
{
    PreserveLink,
    CopyContents,
    Ignore,
}

// New class
public sealed class TarWriterOptions
{
    // Corresponds to existing constructor argument.
    public TarEntryFormat Format { get; set; } = System.Formats.Tar.TarEntryFormat.Pax;

    public TarLinkStrategy HardLinkStrategy { get; set; } = TarLinkStrategy.PreserveLink;
    public TarLinkStrategy SymbolicLinkStrategy { get; set; } = TarLinkStrategy.PreserveLink;
}

// New class
public sealed class TarExtractOptions
{
    // Corresponds to existing ExtractToDirectoryAsync argument.
    public bool OverwriteFiles { get; set; } = false;

    public TarLinkStrategy HardLinkStrategy { get; set; } = TarLinkStrategy.PreserveLink;
    public TarLinkStrategy SymbolicLinkStrategy { get; set; } = TarLinkStrategy.PreserveLink;
}

public class TarWriter
{
    // New overload accepting options
    public TarWriter(Stream stream, TarWriterOptions options, bool leaveOpen = false);
}

public static class TarFile
{
    // New overloads accepting options for creation
    public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarWriterOptions options);
    public static void CreateFromDirectory(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarWriterOptions options);
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, TarWriterOptions options, CancellationToken cancellationToken = default);
    public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, TarWriterOptions options, CancellationToken cancellationToken = default);

    // New overloads accepting options for extraction
    public static void ExtractToDirectory(Stream source, string destinationDirectoryName, TarExtractOptions options);
    public static void ExtractToDirectory(string sourceFileName, string destinationDirectoryName, TarExtractOptions options);
    public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, TarExtractOptions options, CancellationToken cancellationToken = default);
    public static Task ExtractToDirectoryAsync(string sourceFileName, string destinationDirectoryName, TarExtractOptions options, CancellationToken cancellationToken = default);
}
```

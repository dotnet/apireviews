# API Review 11/30/2021

## DirectoryInfo.Copy / Directory.Copy

**Approved** | [#runtime/60903](https://github.com/dotnet/runtime/issues/60903#issuecomment-982958180) | [Video](https://www.youtube.com/watch?v=9fAW50_edfM&t=0h0m0s)

* We changed the 3 overloads to two with default parameters.
* We added cancellation to the still-synchronous methods
* We renamed some parameters
* We added a boolean return value.  False indiciates that something couldn't be read (a file couldn't be read, or a directory couldn't be enumerated).  Write failures should throw.

```C#

namespace System.IO
{
    public partial class DirectoryInfo : FileSystemInfo
    {
        public bool CopyTo(string destinationPath, bool recursive);
        public bool CopyTo(string destinationPath, bool recursive, bool skipExistingFiles=true, CancellationToken cancellationToken=default);
    }
    public partial class Directory
    {
        public static bool Copy(string sourcePath, string destinationPath, bool recursive);
        public static bool Copy(string sourcePath, string destinationPath, bool recursive, bool skipExistingFiles=true, CancellationToken cancellationToken=default);
    }
}
```
## Analyzer: Replace occurrences of TypeInfo with Type and GetTypeInfo() with GetType()

**Approved** | [#runtime/61122](https://github.com/dotnet/runtime/issues/61122#issuecomment-982968329) | [Video](https://www.youtube.com/watch?v=9fAW50_edfM&t=0h37m1s)

* Generally looks good as proposed.
* While a fixer may produce errors in a few edge cases, it seems generally valueable, so we should make one.
* The analyzer should check the Type/TypeInfo hierarchy to not run when it's not appropriate (e.g. netstandard1.x).
* It should not flag uses where any TypeInfo-specific members are used.

Category: Maintainability
Severity: Info
## Port rule CA1011 - Consider passing base types as parameters

**Approved** | [#runtime/61916](https://github.com/dotnet/runtime/issues/61916#issuecomment-982973839) | [Video](https://www.youtube.com/watch?v=9fAW50_edfM&t=0h50m29s)

* Generally looks good as proposed, but we changed Hidden to IdeSuggestion

Category: Design
Severity: IdeSuggestion

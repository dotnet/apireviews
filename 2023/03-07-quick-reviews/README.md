# API Review 03/07/2023

## MemoryExtensions.Replace with separate source/destination buffers

**Approved** | [#runtime/81829](https://github.com/dotnet/runtime/issues/81829#issuecomment-1458626970) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=0h0m0s)

Looks good as proposed.  We discussed whether or not the method warranted an "-Into" suffix, given the existing overload, and decided it didn't.

The method probably should reject source and destination in a partial overlap.

Given that the "existing" Replace method was added in this release, consider whether it is still warranted, or should be removed in favor of this one.

The concern was also raised that a `this Span<T> source, Span<T> destination` overload might be desired, given the extension methods.  Such an overload is an easy approval of just squaring off the overload group, but isn't specifically being recommended at this time.

```C#
namespace System

public static partial class MemoryExtensions
{
     public static void Replace<T>(this ReadOnlySpan<T> source, Span<T> destination, T oldValue, T newValue) where T : IEquatable<T>?
}
```
## Add ConsoleModifiers.None and ConsoleKey.None

**Approved** | [#runtime/79868](https://github.com/dotnet/runtime/issues/79868#issuecomment-1458633827) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=0h16m13s)

Looks good as proposed.

```C#
namespace System;

[Flags]
public partial enum ConsoleModifiers
{
    None = 0,
}

public partial enum ConsoleKey
{
    None = 0,
}
```
## Remove unused resources

**Approved** | [#runtime/79765](https://github.com/dotnet/runtime/issues/79765#issuecomment-1458663596) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=0h21m50s)

This seems generally good as proposed, since it is focused on generated code (which is often ignored by other analyzers).  Rather than using convention-based naming it probably wants to find the resx->cs options in the csproj and behave like the tooling would.

It's a little sad that a fixer isn't viable here, but c'est la vie.

Category: Maintainability
Visibility: None (off by default)
Code: TBD (by the roslyn-analyzers side)
## X509Certificate.ExportPkcs12 with PBE parameters

**Approved** | [#runtime/80314](https://github.com/dotnet/runtime/issues/80314#issuecomment-1458687800) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=0h46m7s)

Looks good as proposed.

We discussed replacing the enum with prepopulated accelerators on PbeParameters, but the enum values don't specifically encode the iteration count (where we're following OS defaults).

```C#
namespace System.Security.Cryptography.X509Certificates;

public partial class X509Certificate  {
    public byte[] ExportPkcs12(Pkcs12ExportPbeParameters exportParameters, string? password);
    public byte[] ExportPkcs12(PbeParameters exportParameters, string? password);
}

public partial class X509Certificate2Collection  {
    public byte[] ExportPkcs12(Pkcs12ExportPbeParameters exportParameters, string? password);
    public byte[] ExportPkcs12(PbeParameters exportParameters, string? password);
}


public enum Pkcs12ExportPbeParameters {
    // Initially will behave as `Pbes2TripleDesSha1`, but reserved to change behavior in the future.
    Default = 0,

    // TripleDes3KeyPkcs12, SHA1, 2000 PBKDF2 rounds. Matches Win32.
    Pbes2TripleDesSha1 = 1,

    // AES-256, SHA256, 2000 PBKDF2 rounds. Matches Win32.
    Pbes2Aes256Sha256 = 2,
}
```
## [Analyzer] Async methods that don't take a CancellationToken

**Approved** | [#runtime/78397](https://github.com/dotnet/runtime/issues/78397#issuecomment-1458690991) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=1h6m35s)

Looks good as proposed (finding Task-like returning method groups with no overloads that accept a CancellationToken).

Category: Design
Severity: None (off by default)
## [Analyzer] Unawaited / returned tasks created with a disposable in using block for that disposable

**Approved** | [#runtime/78394](https://github.com/dotnet/runtime/issues/78394#issuecomment-1458707957) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=1h9m29s)

While it can't detect cases where the IDisposable got wrapped into another parameter, it does seem like it provides value for the ones it detected.

It would also be nice if it could detect the dispose via a manual try-finally or direct imperative code (e.g.

```C#
    SomeIDisposable disposable = new SomeIDisposable(); 
    Task t = SomethingAsync(disposable);
    disposable.Dispose();
    return t;
```
)

And it might be further nice if we could generalize it to rent/return patterns like ArrayPool, but that's perhaps getting a bit fantastical (and each would require a custom flow analyzer, or putting some sort of attribute on IDisposable.Dispose() (and all aliasing Close() methods) and ArrayPool.Return)

Category: Reliability
Severity: Warning
## [Analyzer] Use IndexOfAnyValues instead of inlined or cached array

**Approved** | [#runtime/78587](https://github.com/dotnet/runtime/issues/78587#issuecomment-1458729896) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=1h24m37s)

The analyzer looks good as proposed.

Category: Performance
Severity: Info (which may get lowered based on the amount of noise in testing/development)
## [Analyzer] Make types declared in an executable internal

**Approved** | [#runtime/78388](https://github.com/dotnet/runtime/issues/78388#issuecomment-1458759543) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=1h37m29s)

Looks good as proposed.

Given concerns with some of the templates (e.g. WinForms creating the skeleton for a new form type being default) we're recommending this analyzer be opt-in.  But we're probably easily convinced that it should be something else.

Category: Maintainability (?)
Visibility: None (off by default; might be nice if trimmer-style scenarios can flip it on by default)
## Analyzer/fixer for converting Stream.Read calls to ReadAtLeast and ReadExactly

**Approved** | [#runtime/69159](https://github.com/dotnet/runtime/issues/69159#issuecomment-1458771875) | [Video](https://www.youtube.com/watch?v=wagxiBYZn-A&t=1h53m23s)

Looks good as proposed.

The diagnostic (possibly with a different diagnostic ID) should still fire on older TFMs to report that the call made to Read is unreliable as written.

Category: Reliability
Severity: Warning

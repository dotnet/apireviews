# API Review 11/16/2021

## System.Runtime.CompilerServices.RuntimeCompatibilityAttribute.DisableRuntimeMarshalling

**Approved** | [#runtime/60639](https://github.com/dotnet/runtime/issues/60639#issuecomment-970569985) | [Video](https://www.youtube.com/watch?v=d1hJwHDz5Wc&t=0h0m0s)

* We changed this to be a separate attribute because of how it composes with the compiler-emitted `RuntimeCompatibilityAttribute`
* We had a philosophical discussion about generators emitting attributes like this, and concluded that they shouldn't because of the generator source-view model and composability.  (When they're required that should be detected by an analyzer)

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(System.AttributeTargets.Assembly, AllowMultiple = false, Inherited = false)]
    public sealed class DisableRuntimeMarshallingAttribute : Attribute
    {
    }
}
```
## Add RequiresDynamicCodeAttribute

**Approved** | [#runtime/61239](https://github.com/dotnet/runtime/issues/61239#issuecomment-970579765) | [Video](https://www.youtube.com/watch?v=d1hJwHDz5Wc&t=0h46m3s)

Looks good as proposed.

```C#
namespace System.Diagnostics.CodeAnalysis
{
    /// <summary>
    /// Indicates that the specified method requires the ability to generate new code at runtime,
    /// for example through <see cref="System.Reflection"/>.
    /// </summary>
    /// <remarks>
    /// This allows tools to understand which methods are unsafe to call when compiling ahead of time.
    /// </remarks>
    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Constructor, Inherited = false)]
    public sealed class RequiresDynamicCodeAttribute : Attribute
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="RequiresDynamicCodeAttribute"/> class
        /// with the specified message.
        /// </summary>
        /// <param name="message">
        /// A message that contains information about the usage of dynamic code.
        /// </param>
        public RequiresDynamicCodeAttribute(string message)
        {
            Message = message;
        }

        /// <summary>
        /// Gets a message that contains information about the usage of dynamic code.
        /// </summary>
        public string Message { get; }

        /// <summary>
        /// Gets or sets an optional URL that contains more information about the method,
        /// why it requries dynamic code, and what options a consumer has to deal with it.
        /// </summary>
        public string? Url { get; set; }
    }
}
```
## Introduce a common base type for ECDsa and ECDiffieHellman

**Approved** | [#runtime/61516](https://github.com/dotnet/runtime/issues/61516#issuecomment-970588262) | [Video](https://www.youtube.com/watch?v=d1hJwHDz5Wc&t=0h59m30s)

Looks good as proposed

```diff
namespace System.Security.Cryptography
{
+    public abstract class ECAlgorithm : AsymmetricAlgorithm
+    {
#       Existing implementations on ECDsa and ECDiffieHellman that throw NotImplementedException
#       These will throw NotImplementedException, as they do now.
+       public virtual void ImportParameters(ECParameters parameters);
+       public virtual ECParameters ExportParameters(bool includePrivateParameters);
+       public virtual ECParameters ExportExplicitParameters(bool includePrivateParameters);
+       public virtual void GenerateKey(ECCurve curve);

#       Virtuals on ECDsa and ECDiffieHellman with identical implementations.
#       We could push the implementation down to this type.
+       public virtual void ImportECPrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
+       public virtual byte[] ExportECPrivateKey();
+       public virtual bool TryExportECPrivateKey(Span<byte> destination, out int bytesWritten);

#       Overrides from ECDsa and ECDiffieHellman that have identical implementation and
#       can be pushed down to this type.
+       public override void ImportFromPem(ReadOnlySpan<char> input);
+       public override void ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source, out int bytesRead);
+       public override bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
+       public override void ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source, out int bytesRead);
+       public override void ImportPkcs8PrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
+       public override void TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

#       APIs that are new in .NET 7 and are not too late to move from ECDsa and ECDiffieHellman
#       since they are non-virtual but identical implementation.
+       public string ExportECPrivateKeyPem();
+       public bool TryExportECPrivateKeyPem(Span<char> destination, out int charsWritten);
+    }

-   public abstract class ECDsa : AsymmetricAlgorithm
+   public abstract class ECDsa : ECAlgorithm
    {
#       existing virtuals that are now handled by the base class.
#       if we need the members to explicitly exist on this type, then they
#       can become overrides that simply call `base.` I've been told that the
#       CLR correctly handles dispatching to the base type when virtuals are removed.
-       public virtual void ImportParameters(ECParameters parameters);
-       public virtual ECParameters ExportParameters(bool includePrivateParameters);
-       public virtual ECParameters ExportExplicitParameters(bool includePrivateParameters);
-       public virtual void GenerateKey(ECCurve curve);
-       public virtual void ImportECPrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
-       public virtual byte[] ExportECPrivateKey();
-       public virtual bool TryExportECPrivateKey(Span<byte> destination, out int bytesWritten);

#      overrides that can be removed since they are now handled by the base
-      public override void ImportFromPem(ReadOnlySpan<char> input);
-      public override void ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source, out int bytesRead);
-      public override bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
-      public override void ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source, out int bytesRead);
-      public override void ImportPkcs8PrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
-      public override void TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

#      These are non virtual with identical implementations between ECDsa and ECDiffieHellman
#      They have not shipped in any .NET 7 so we can move them down if this proposal gets accepted for .NET 7.
-      public string ExportECPrivateKeyPem();
-      public bool TryExportECPrivateKeyPem(Span<char> destination, out int charsWritten);
    }

-   public abstract class ECDiffieHellman : AsymmetricAlgorithm
+   public abstract class ECDiffieHellman : ECAlgorithm
    {
#       existing virtuals that are now handled by the base class.
#       if we need the members to explicitly exist on this type, then they
#       can become overrides that simply call `base.` I've been told that the
#       CLR correctly handles dispatching to the base type when virtuals are removed.
-       public virtual void ImportParameters(ECParameters parameters);
-       public virtual ECParameters ExportParameters(bool includePrivateParameters);
-       public virtual ECParameters ExportExplicitParameters(bool includePrivateParameters);
-       public virtual void GenerateKey(ECCurve curve);
-       public virtual void ImportECPrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
-       public virtual byte[] ExportECPrivateKey();
-       public virtual bool TryExportECPrivateKey(Span<byte> destination, out int bytesWritten);

#      overrides that can be removed since they are now handled by the base
-      public override void ImportFromPem(ReadOnlySpan<char> input);
-      public override void ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source, out int bytesRead);
-      public override bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
-      public override void ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source, out int bytesRead);
-      public override void ImportPkcs8PrivateKey(ReadOnlySpan<byte> source, out int bytesRead);
-      public override void TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

#      These are non virtual with identical implementations between ECDsa and ECDiffieHellman
#      They have not shipped in any .NET 7 so we can move them down if this proposal gets accepted for .NET 7.
-      public string ExportECPrivateKeyPem();
-      public bool TryExportECPrivateKeyPem(Span<char> destination, out int charsWritten);
    }
}
```
## `ErrorProvider` extend with `HasErrors` property

**Approved** | [#winforms/5912](https://github.com/dotnet/winforms/issues/5912#issuecomment-970593748) | [Video](https://www.youtube.com/watch?v=d1hJwHDz5Wc&t=1h10m51s)

Looks good as proposed

```C#
namespace System.Windows.Forms {
   public class ErrorProvider {
       public bool HasErrors { get; }
   }
}
```
## Pass constants to parameters marked as [ConstantExpected]

**Approved** | [#runtime/33771](https://github.com/dotnet/runtime/issues/33771#issuecomment-970633087) | [Video](https://www.youtube.com/watch?v=d1hJwHDz5Wc&t=1h16m35s)

The attribute looks good as proposed (except we changed the namespace)

```C#
namespace System.Diagnostics.CodeAnalysis
{
    [AttributeUsage(AttributeTargets.Parameter, AllowMultiple = false, Inherited = false)]
    public sealed class ConstantExpectedAttribute : Attribute
    {
        public object? Min { get; set; }
        public object? Max { get; set; }
    }
}
```

For the analyzer:
* Category: Performance
* The default diagnostic for passing a non-constant value: Warning
* The default diagnostic for a constant that's out of range: Warning
* The default diagnostic for a constant that's out of range by composition with another ConstantExpected attribute: Warning
* The default diagnostic for passing min/max that make no sense (wrong type, inverted, is a reference type, etc): Error
* So far we've identified two distinct diagnostic IDs needed, one for "you used the attribute wrong" (the error) and one for "you're calling this method wrong" (all of the others).  They can, of course, be subsetted as needs demonstrate.

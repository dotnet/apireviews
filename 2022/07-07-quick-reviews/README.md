# API Review 07/07/2022

## Non-cooperative abortion of code execution

**Approved** | [#runtime/69622](https://github.com/dotnet/runtime/issues/69622#issuecomment-1178000827) | [Video](https://www.youtube.com/watch?v=kttLFq2Mh9k&t=0h0m0s)

* Is System.Runtime.CompilerServices the right namespace?
  * Changed to just System.Runtime
* We changed the instance ctor/Run()/Abort to a single static using CancellationToken
* To discourage people from calling it, we should declare it Obsolete out of the box, with a custom diagnostic ID (and message)
* ThreadAbortException should not leak out of Run, it should get normalized to OperationCancelledException.
  * Other exceptions can go through without normalization

```C#
namespace System.Runtime;

[Obsolete(some diag code)]
public static class ControlledExecution
{
    public static void Run(Action action, CancellationToken cancellationToken);
}
```
## Add Wrap and Unwrap methods to NegotiateAuthentication API

**Approved** | [#runtime/70909](https://github.com/dotnet/runtime/issues/70909#issuecomment-1178022831) | [Video](https://www.youtube.com/watch?v=kttLFq2Mh9k&t=0h49m34s)

* UnwrapInplace should have capital-Place
* We split Wrap's ref isEncrypted into requestEncryption and out isEncrypted
* Immo suggests renaming NegotiateAuthenticationStatusCode to NegotiateAuthenticationStatus (drop the "Code" suffix).  No one had strong opinions one way or the other.

```C#
namespace System.Net.Security;

/// <summary>
/// Represents a stateful authentication exchange that uses the Negotiate, NTLM or Kerberos security protocols
/// to authenticate the client or server, in client-server communication.
/// </summary>
public sealed class NegotiateAuthentication : IDisposable
{
    /// <summary>
    /// Wrap an input message with signature and optionally with an encryption.
    /// </summary>
    /// <param name="input">Input message to be wrapped.</param>
    /// <param name="outputWriter">Buffer writer where the wrapped message is written.</param>
    /// <param name="isEncrypted">
    /// On input specifies whether encryption is requested.
    /// On output specifies whether encryption was applied in the wrapping.
    /// </param>
    /// <returns>
    /// <see cref="NegotiateAuthenticationStatusCode.Completed" /> on success, other
    /// <see cref="NegotiateAuthenticationStatusCode" /> values on failure.
    /// </returns>
    /// <remarks>
    /// Like the <see href="https://datatracker.ietf.org/doc/html/rfc2743#page-65">GSS_Wrap</see> API
    /// the authentication protocol implementation may choose to override the requested value in the
    /// isEncrypted parameter. This may result in either downgrade or upgrade of the protection level.
    /// </remarks>
    /// <exception cref="InvalidOperationException">Authentication failed or has not occurred.</exception>
    public NegotiateAuthenticationStatusCode Wrap(ReadOnlySpan<byte> input, IBufferWriter<byte> outputWriter, bool requestEncryption, out bool isEncrypted);
    
    /// <summary>
    /// Unwrap an input message with signature or encryption applied by the other party.
    /// </summary>
    /// <param name="input">Input message to be unwrapped.</param>
    /// <param name="outputWriter">Buffer writter where the unwrapped message is written.</param>
    /// <param name="isEncrypted">
    /// On output specifies whether the wrapped message had encryption applied.
    /// </param>
    /// <returns>
    /// <see cref="NegotiateAuthenticationStatusCode.Completed" /> on success.
    /// <see cref="NegotiateAuthenticationStatusCode.MessageAltered" /> if the message signature was
    /// invalid.
    /// <see cref="NegotiateAuthenticationStatusCode.InvalidToken" /> if the wrapped message was
    /// in invalid format.
    /// Other <see cref="NegotiateAuthenticationStatusCode" /> values on failure.
    /// </returns>
    /// <exception cref="InvalidOperationException">Authentication failed or has not occurred.</exception>
    public NegotiateAuthenticationStatusCode Unwrap(ReadOnlySpan<byte> input, IBufferWriter<byte> outputWriter, out bool isEncrypted);

    /// <summary>
    /// Unwrap an input message with signature or encryption applied by the other party.
    /// </summary>
    /// <param name="input">Input message to be unwrapped. On output contains the decoded data.</param>
    /// <param name="unwrappedOffset">Offset in the input buffer where the unwrapped message was written.</param>
    /// <param name="unwrappedLength">Length of the unwrapped message.</param>
    /// <param name="isEncrypted">
    /// On output specifies whether the wrapped message had encryption applied.
    /// </param>
    /// <returns>
    /// <see cref="NegotiateAuthenticationStatusCode.Completed" /> on success.
    /// <see cref="NegotiateAuthenticationStatusCode.MessageAltered" /> if the message signature was
    /// invalid.
    /// <see cref="NegotiateAuthenticationStatusCode.InvalidToken" /> if the wrapped message was
    /// in invalid format.
    /// Other <see cref="NegotiateAuthenticationStatusCode" /> values on failure.
    /// </returns>
    /// <exception cref="InvalidOperationException">Authentication failed or has not occurred.</exception>
    public NegotiateAuthenticationStatusCode UnwrapInPlace(Span<byte> input, out int unwrappedOffset, out int unwrappedLength, out bool isEncrypted);
}

public partial class NegotiateAuthenticationClientOptions
{
    /// <summary>
    /// Indicates that mutual authentication is required between the client and server.
    /// </summary>
    public bool RequireMutualAuthentication { get; set; }
    
    /// <summary>
    /// One of the <see cref="TokenImpersonationLevel" /> values, indicating how the server
    /// can use the client's credentials to access resources.
    /// </summary>
    public System.Security.Principal.TokenImpersonationLevel AllowedImpersonationLevel { get; set; }
}

public partial class NegotiateAuthentication
{
    /// <summary>
    /// One of the <see cref="TokenImpersonationLevel" /> values, indicating the negotiated
    /// level of impresonation.
    /// </summary>
    public System.Security.Principal.TokenImpersonationLevel ImpersonationLevel { get; }
}

public partial class NegotiateAuthenticationServerOptions
{
    /// <summary>
    /// Indicates extended security and validation policies.
    /// </summary>
    public System.Security.Authentication.ExtendedProtectionPolicy? Policy { get; set; }

    /// <summary>
    /// One of the <see cref="TokenImpersonationLevel" /> values, indicating how the server
    /// can use the client's credentials to access resources.
    /// </summary>
    public System.Security.Principal.TokenImpersonationLevel RequiredImpersonationLevel { get; set; }
}

public partial enum NegotiateAuthenticationStatusCode
{
    /// <status>Validation of RequiredProtectionLevel against negotiated protection level failed.</status>
    /// <remarks>Part of original API proposal, just not enforced in the managed code yet</remarks>
    SecurityQosFailed,
    /// <status>Validation of the target name failed</status>
    TargetUnknown,
    /// <status>Validation of the impersonation level failed</status>
    ImpersonationValidationFailed,
}
```
## Add X509ChainPolicy to SslOptions

**Approved** | [#runtime/71191](https://github.com/dotnet/runtime/issues/71191#issuecomment-1178042823) | [Video](https://www.youtube.com/watch?v=kttLFq2Mh9k&t=1h5m49s)

* The new properties should be properties, not fields.
* We added some enhancements to X509ChainPolicy to increase the usability and maintainability here.
* Consider an analyzer that warns if both ValidationPolicy and one of the older properties (such as CertificateRevocationCheckMode) are both assigned

```C#
namespace System.Net.Security
{
    public partial class SslServerAuthenticationOptions
    {
        public X509ChainPolicy? ValidationPolicy { get; set; }
    }
    public class SslClientAuthenticationOptions
    {
        public X509ChainPolicy? ValidationPolicy { get; set; }
    }
}

namespace System.Security.Cryptography.X509ChainPolicy
{
    public partial class X509ChainPolicy
    {
        public X509ChainPolicy Clone();
        // Breaking change: This defaults to true (and gets set to false if set_VerificationTime is called)
        public bool VerificationTimeIgnored { get; set; }
    }
}
```
## Add APIs to JSON Contract customization

**Approved** | [#runtime/71123](https://github.com/dotnet/runtime/issues/71123#issuecomment-1178075692) | [Video](https://www.youtube.com/watch?v=kttLFq2Mh9k&t=1h22m48s)

Looks good as proposed.

* We discussed JsonPropertyInfo.IsExtensionData vs JsonTypeInfo.ExtensionDataProperty and felt that the JsonPropertyInfo version better reflects any existing attribute state during generation.
* We discussed the necessity of the Order property, and felt it made sense for the target scenarios.

```C#
namespace System.Text.Json.Serialization.Metadata
{
    public class JsonTypeInfo
    {
        // Maps to IJsonOnSerializing
        public Action<object>? OnSerializing { get; set; } = null;
        // Maps to IJsonOnSerialized
        public Action<object>? OnSerialized { get; set; } = null;
        // Maps to IJsonOnDeserializing
        public Action<object>? OnDeserializing { get; set; } = null;
        // Maps to IJsonOnDeserialized
        public Action<object>? OnDeserialized { get; set; } = null;
    }

    public abstract partial class JsonPropertyInfo
    {
        // Abstracts backing member, DefaultJsonTypeInfoResolver will return instance of PropertyInfo/FieldInfo
        public System.Reflection.ICustomAttributeProvider? AttributeProvider { get; set; } = null;

        // Maps to JsonPropertyOrderAttribute
        public int Order { get; set; } = 0;

        // Maps to JsonExtensionDataAttribute
        // Alternatively this could be a nullable JsonPropertyInfo on JsonTypeInfo.
        public bool IsExtensionData { get; set; } = false;
    }

    [EditorBrowsable(EditorBrowsableState.Never)]
    public static partial class JsonMetadataServices
    {
        public static JsonConverter<T?> GetNullableConverter<T>(JsonSerializerOptions options) where T : struct { throw null; }
    }
}

namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        // Existing GetConverter API
        public JsonConverter GetConverter(Type type);

        // Gets metadata that has been cached by this options instance
        // Can be used to plug metadata directly into JsonSerializer overloads that accept JsonTypeInfo<T>
        public JsonTypeInfo GetTypeInfo(Type type);
    }
}
```
## Generic Math - API tweaks

**Approved** | [#runtime/71648](https://github.com/dotnet/runtime/issues/71648#issuecomment-1178119500) | [Video](https://www.youtube.com/watch?v=kttLFq2Mh9k&t=1h47m57s)

## Unary Plus

Keep TResult, give up on the DIM

```C#
namespace System.Numerics;

// as-is
public interface IUnaryPlusOperators<TSelf, TResult>
    where TSelf : IUnaryPlusOperators<TSelf, TResult>
{
    static abstract TResult operator +(TSelf value);
}
```

## Comparison and Equality

Add TResult, lock TResult to bool at INumber.

```C#
namespace System.Numerics;

// add TResult
public interface IComparisonOperators<TSelf, TOther, TResult>
    : IComparable,
      IComparable<TOther>,
      IEqualityOperators<TSelf, TOther>
    where TSelf : IComparisonOperators<TSelf, TOther>
{
    static abstract TResult operator <(TSelf left, TOther right);
    static abstract TResult operator <=(TSelf left, TOther right);
    static abstract TResult operator >(TSelf left, TOther right);
    static abstract TResult operator >=(TSelf left, TOther right);
}

public interface IEqualityOperators<TSelf, TOther> : IEquatable<TOther>
    where TSelf : IEqualityOperators<TSelf, TOther>
{
    static abstract TResult operator ==(TSelf left, TOther right);
    static abstract TResult operator !=(TSelf left, TOther right);
}
```

## ITrigonometricFunctions

Add SinCosPi, move Atan2/Atan2Pi to the scalar-only interface

```C#
namespace System.Numerics;

public interface ITrigonometricFunctions<TSelf>
    where TSelf : ITrigonometricFunctions<TSelf>, IFloatingPoint<TSelf>
{
    static abstract TSelf Acos(TSelf x);
    static abstract TSelf AcosPi(TSelf x);
    static abstract TSelf Asin(TSelf x);
    static abstract TSelf AsinPi(TSelf x);
    static abstract TSelf Atan(TSelf x);
    static abstract TSelf AtanPi(TSelf x);
    static abstract TSelf Cos(TSelf x);
    static abstract TSelf CosPi(TSelf x);
    static abstract TSelf Sin(TSelf x);
    static abstract (TSelf Sin, TSelf Cos) SinCos(TSelf x);
    static abstract (TSelf SinPi, TSelf CosPi) SinCosPi(TSelf x);
    static abstract TSelf SinPi(TSelf x);
    static abstract TSelf Tan(TSelf x);
    static abstract TSelf TanPi(TSelf x);
}

partial interface IFloatingPointIeee754<TSelf>
{
    static abstract TSelf Atan2(TSelf y, TSelf x);
    static abstract TSelf Atan2Pi(TSelf y, TSelf x);
}
```

## IPowerFunctions and IRootFunctions

Match the C runtime with an "N" suffix when we're locking the second argument to int.

```C#
namespace System.Numerics;

public partial interface IRootFunctions<TSelf>
{
    static abstract TSelf RootN(TSelf x, int n);
}

```

## Math Constants

* Renamed IMathConstants to IFloatingPointConstants

```C#
namespace System.Numerics;

public interface IFloatingPointConstants<TSelf>
    where TSelf : IFloatingPointConstants<TSelf>, INumberBase<TSelf>
{
    public static TSelf E { get; }

    public static TSelf Pi { get; }

    public static TSelf Tau { get; }
}

partial interface IFloatingPoint<TSelf> : IFloatingPointConstants<TSelf> {}

// Decimal implements these explicitly
```

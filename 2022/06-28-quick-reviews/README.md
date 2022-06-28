# API Review 06/28/2022

## JavaScript interop with [JSImport] and [JSExport] attributes and Roslyn

**Approved** | [#runtime/70133](https://github.com/dotnet/runtime/issues/70133#issuecomment-1169038727) | [Video](https://www.youtube.com/watch?v=gGErXh4z9_Y&t=0h0m0s)

* `JSHost`
    - `Import` should be `ImportAsync` and take a `CancellationToken`
* `JSFunctionBinding`
    - `BindCSFunction` should be `BindManagedFunction`

```C#
namespace System.Runtime.InteropServices.JavaScript;

[AttributeUsage(AttributeTargets.Method, Inherited = false, AllowMultiple = false)]
[SupportedOSPlatform("browser")]
public sealed class JSImportAttribute : Attribute
{
    public JSImportAttribute(string functionName);
    public JSImportAttribute(string functionName, string moduleName);
    public string FunctionName { get; }
    public string? ModuleName { get; }
}

[AttributeUsage(AttributeTargets.Method, Inherited = false, AllowMultiple = false)]
[SupportedOSPlatform("browser")]
public sealed class JSExportAttribute : Attribute
{
    public JSExportAttribute();
}

[AttributeUsage(AttributeTargets.Parameter | AttributeTargets.ReturnValue, Inherited = false, AllowMultiple = false)]
[SupportedOSPlatform("browser")]
public sealed class JSMarshalAsAttribute<T> : Attribute where T : JSType
{
    public JSMarshalAsAttribute();
}

[SupportedOSPlatform("browser")]
public abstract class JSType
{
    internal JSType();
    public sealed class None : JSType
    {
        internal None();
    }
    public sealed class Void : JSType
    {
        internal Void();
    }
    public sealed class Discard : JSType
    {
        internal Discard();
    }
    public sealed class Boolean : JSType
    {
        internal Boolean();
    }
    public sealed class Number : JSType
    {
        internal Number();
    }
    public sealed class BigInt : JSType
    {
        internal BigInt();
    }
    public sealed class Date : JSType
    {
        internal Date();
    }
    public sealed class String : JSType
    {
        internal String();
    }
    public sealed class Object : JSType
    {
        internal Object();
    }
    public sealed class Error : JSType
    {
        internal Error();
    }
    public sealed class MemoryView : JSType
    {
        internal MemoryView();
    }
    public sealed class Array<T> : JSType where T : JSType
    {
        internal Array();
    }
    public sealed class Promise<T> : JSType where T : JSType
    {
        internal Promise();
    }
    public sealed class Function : JSType
    {
        internal Function();
    }
    public sealed class Function<T> : JSType where T : JSType
    {
        internal Function();
    }
    public sealed class Function<T1, T2> : JSType where T1 : JSType where T2 : JSType
    {
        internal Function();
    }
    public sealed class Function<T1, T2, T3> : JSType where T1 : JSType where T2 : JSType where T3 : JSType
    {
        internal Function();
    }
    public sealed class Function<T1, T2, T3, T4> : JSType where T1 : JSType where T2 : JSType where T3 : JSType where T4 : JSType
    {
        internal Function();
    }
    public sealed class Any : JSType
    {
        internal Any();
    }
}

[SupportedOSPlatform("browser")]
public class JSObject : IDisposable
{
    internal JSObject();
    public bool IsDisposed { get; }
    public void Dispose();

    public bool HasProperty(string propertyName);
    public string GetTypeOfProperty(string propertyName);

    public bool GetPropertyAsBoolean(string propertyName);
    public int GetPropertyAsInt32(string propertyName);
    public double GetPropertyAsDouble(string propertyName);
    public string? GetPropertyAsString(string propertyName);
    public JSObject? GetPropertyAsJSObject(string propertyName);
    public byte[]? GetPropertyAsByteArray(string propertyName);

    public void SetProperty(string propertyName, bool value);
    public void SetProperty(string propertyName, int value);
    public void SetProperty(string propertyName, double value);
    public void SetProperty(string propertyName, string? value);
    public void SetProperty(string propertyName, JSObject? value);
    public void SetProperty(string propertyName, byte[]? value);
}

[SupportedOSPlatform("browser")]
public sealed class JSException : Exception
{
    public JSException(string msg);
}

[SupportedOSPlatform("browser")]
public static class JSHost
{
    public static JSObject GlobalThis { get; }
    public static JSObject DotnetInstance { get; }
    public static Task<JSObject> ImportAsync(string moduleName, string moduleUrl, CancellationToken cancellationToken = default);
}

[SupportedOSPlatform("browser")]
[CLSCompliant(false)]
[EditorBrowsable(EditorBrowsableState.Never)]
public sealed class JSFunctionBinding
{
    public static void InvokeJS(JSFunctionBinding signature, Span<JSMarshalerArgument> arguments);
    public static JSFunctionBinding BindJSFunction(string functionName, string moduleName, ReadOnlySpan<JSMarshalerType> signatures);
    public static JSFunctionBinding BindManagedFunction(string fullyQualifiedName, int signatureHash, ReadOnlySpan<JSMarshalerType> signatures);
}

[SupportedOSPlatform("browser")]
[EditorBrowsable(EditorBrowsableState.Never)]
public sealed class JSMarshalerType
{
    private JSMarshalerType();
    public static JSMarshalerType Void { get; }
    public static JSMarshalerType Discard { get; }
    public static JSMarshalerType Boolean { get; }
    public static JSMarshalerType Byte { get; }
    public static JSMarshalerType Char { get; }
    public static JSMarshalerType Int16 { get; }
    public static JSMarshalerType Int32 { get; }
    public static JSMarshalerType Int52 { get; }
    public static JSMarshalerType BigInt64 { get; }
    public static JSMarshalerType Double { get; }
    public static JSMarshalerType Single { get; }
    public static JSMarshalerType IntPtr { get; }
    public static JSMarshalerType JSObject { get; }
    public static JSMarshalerType Object { get; }
    public static JSMarshalerType String { get; }
    public static JSMarshalerType Exception { get; }
    public static JSMarshalerType DateTime { get; }
    public static JSMarshalerType DateTimeOffset { get; }
    public static JSMarshalerType Nullable(JSMarshalerType primitive);
    public static JSMarshalerType Task();
    public static JSMarshalerType Task(JSMarshalerType result);
    public static JSMarshalerType Array(JSMarshalerType element);
    public static JSMarshalerType ArraySegment(JSMarshalerType element);
    public static JSMarshalerType Span(JSMarshalerType element);
    public static JSMarshalerType Action();
    public static JSMarshalerType Action(JSMarshalerType arg1);
    public static JSMarshalerType Action(JSMarshalerType arg1, JSMarshalerType arg2);
    public static JSMarshalerType Action(JSMarshalerType arg1, JSMarshalerType arg2, JSMarshalerType arg3);
    public static JSMarshalerType Function(JSMarshalerType result);
    public static JSMarshalerType Function(JSMarshalerType arg1, JSMarshalerType result);
    public static JSMarshalerType Function(JSMarshalerType arg1, JSMarshalerType arg2, JSMarshalerType result);
    public static JSMarshalerType Function(JSMarshalerType arg1, JSMarshalerType arg2, JSMarshalerType arg3, JSMarshalerType result);
}

[SupportedOSPlatform("browser")]
[CLSCompliant(false)]
[EditorBrowsable(EditorBrowsableState.Never)]
public struct JSMarshalerArgument
{
    public delegate void ArgumentToManagedCallback<T>(ref JSMarshalerArgument arg, out T value);
    public delegate void ArgumentToJSCallback<T>(ref JSMarshalerArgument arg, T value);
    public void Initialize();
    public void ToManaged(out bool value);
    public void ToJS(bool value);
    public void ToManaged(out bool? value);
    public void ToJS(bool? value);
    public void ToManaged(out byte value);
    public void ToJS(byte value);
    public void ToManaged(out byte? value);
    public void ToJS(byte? value);
    public void ToManaged(out byte[]? value);
    public void ToJS(byte[]? value);
    public void ToManaged(out char value);
    public void ToJS(char value);
    public void ToManaged(out char? value);
    public void ToJS(char? value);
    public void ToManaged(out short value);
    public void ToJS(short value);
    public void ToManaged(out short? value);
    public void ToJS(short? value);
    public void ToManaged(out int value);
    public void ToJS(int value);
    public void ToManaged(out int? value);
    public void ToJS(int? value);
    public void ToManaged(out int[]? value);
    public void ToJS(int[]? value);
    public void ToManaged(out long value);
    public void ToJS(long value);
    public void ToManaged(out long? value);
    public void ToJS(long? value);
    public void ToManagedBig(out long value);
    public void ToJSBig(long value);
    public void ToManagedBig(out long? value);
    public void ToJSBig(long? value);
    public void ToManaged(out float value);
    public void ToJS(float value);
    public void ToManaged(out float? value);
    public void ToJS(float? value);
    public void ToManaged(out double value);
    public void ToJS(double value);
    public void ToManaged(out double? value);
    public void ToJS(double? value);
    public void ToManaged(out double[]? value);
    public void ToJS(double[]? value);
    public void ToManaged(out IntPtr value);
    public void ToJS(IntPtr value);
    public void ToManaged(out IntPtr? value);
    public void ToJS(IntPtr? value);
    public void ToManaged(out DateTimeOffset value);
    public void ToJS(DateTimeOffset value);
    public void ToManaged(out DateTimeOffset? value);
    public void ToJS(DateTimeOffset? value);
    public void ToManaged(out DateTime value);
    public void ToJS(DateTime value);
    public void ToManaged(out DateTime? value);
    public void ToJS(DateTime? value);
    public void ToManaged(out string? value);
    public void ToJS(string? value);
    public void ToManaged(out string?[]? value);
    public void ToJS(string?[]? value);
    public void ToManaged(out Exception? value);
    public void ToJS(Exception? value);
    public void ToManaged(out object? value);
    public void ToJS(object? value);
    public void ToManaged(out object?[]? value);
    public void ToJS(object?[]? value);
    public void ToManaged(out JSObject? value);
    public void ToJS(JSObject? value);
    public void ToManaged(out JSObject?[]? value);
    public void ToJS(JSObject?[]? value);
    public void ToManaged(out Task? value);
    public void ToJS(Task? value);
    public void ToManaged<T>(out Task<T>? value, ArgumentToManagedCallback<T> marshaler);
    public void ToJS<T>(Task<T>? value, ArgumentToJSCallback<T> marshaler);
    public void ToManaged(out Action? value);
    public void ToJS(Action? value);
    public void ToManaged<T>(out Action<T>? value, ArgumentToJSCallback<T> arg1Marshaler);
    public void ToJS<T>(Action<T>? value, ArgumentToManagedCallback<T> arg1Marshaler);
    public void ToManaged<T1, T2>(out Action<T1, T2>? value, ArgumentToJSCallback<T1> arg1Marshaler, ArgumentToJSCallback<T2> arg2Marshaler);
    public void ToJS<T1, T2>(Action<T1, T2>? value, ArgumentToManagedCallback<T1> arg1Marshaler, ArgumentToManagedCallback<T2> arg2Marshaler);
    public void ToManaged<T1, T2, T3>(out Action<T1, T2, T3>? value, ArgumentToJSCallback<T1> arg1Marshaler, ArgumentToJSCallback<T2> arg2Marshaler, ArgumentToJSCallback<T3> arg3Marshaler);
    public void ToJS<T1, T2, T3>(Action<T1, T2, T3>? value, ArgumentToManagedCallback<T1> arg1Marshaler, ArgumentToManagedCallback<T2> arg2Marshaler, ArgumentToManagedCallback<T3> arg3Marshaler);
    public void ToManaged<TResult>(out Func<TResult>? value, ArgumentToManagedCallback<TResult> resMarshaler);
    public void ToJS<TResult>(Func<TResult>? value, ArgumentToJSCallback<TResult> resMarshaler);
    public void ToManaged<T, TResult>(out Func<T, TResult>? value, ArgumentToJSCallback<T> arg1Marshaler, ArgumentToManagedCallback<TResult> resMarshaler);
    public void ToJS<T, TResult>(Func<T, TResult>? value, ArgumentToManagedCallback<T> arg1Marshaler, ArgumentToJSCallback<TResult> resMarshaler);
    public void ToManaged<T1, T2, TResult>(out Func<T1, T2, TResult>? value, ArgumentToJSCallback<T1> arg1Marshaler, ArgumentToJSCallback<T2> arg2Marshaler, ArgumentToManagedCallback<TResult> resMarshaler);
    public void ToJS<T1, T2, TResult>(Func<T1, T2, TResult>? value, ArgumentToManagedCallback<T1> arg1Marshaler, ArgumentToManagedCallback<T2> arg2Marshaler, ArgumentToJSCallback<TResult> resMarshaler);
    public void ToManaged<T1, T2, T3, TResult>(out Func<T1, T2, T3, TResult>? value, ArgumentToJSCallback<T1> arg1Marshaler, ArgumentToJSCallback<T2> arg2Marshaler, ArgumentToJSCallback<T3> arg3Marshaler, ArgumentToManagedCallback<TResult> resMarshaler);
    public void ToJS<T1, T2, T3, TResult>(Func<T1, T2, T3, TResult>? value, ArgumentToManagedCallback<T1> arg1Marshaler, ArgumentToManagedCallback<T2> arg2Marshaler, ArgumentToManagedCallback<T3> arg3Marshaler, ArgumentToJSCallback<TResult> resMarshaler);
    public unsafe void ToManaged(out void* value);
    public unsafe void ToJS(void* value);
    public void ToManaged(out Span<byte> value);
    public void ToJS(Span<byte> value);
    public void ToManaged(out ArraySegment<byte> value);
    public void ToJS(ArraySegment<byte> value);
    public void ToManaged(out Span<int> value);
    public void ToJS(Span<int> value);
    public void ToManaged(out Span<double> value);
    public void ToJS(Span<double> value);
    public void ToManaged(out ArraySegment<int> value);
    public void ToJS(ArraySegment<int> value);
    public void ToManaged(out ArraySegment<double> value);
    public void ToJS(ArraySegment<double> value);
}
```
## [QUIC] QuicException

**Approved** | [#runtime/70669](https://github.com/dotnet/runtime/issues/70669#issuecomment-1169075291) | [Video](https://www.youtube.com/watch?v=gGErXh4z9_Y&t=0h45m59s)

* `QuickError`
    - It seems we're mapping `MsQuic` codes to `QuickError`. We should ensure that our API surface is general enough, ideally using terminology/concepts from the QUIC spec to ensure the managed API could be backed by another QUIC implementation.
    - We should a success code
* `QuicException`
    - The parameter `errorCode` should align with the property `ApplicationProtocolErrorCode` so either name both the long form or both the short form.

```C#
namespace System.Net.Quic;

public sealed class QuicException : IOException
{
    public QuicException(QuicError error, string message, long? applicationErrorCode, Exception? innerException);
    public QuicError QuicError { get; }
    public long? ApplicationErrorCode { get; }
}

public enum QuicError
{
    Success,
    InternalError, 
    ConnectionAborted,
    StreamAborted,
    AddressInUse,
    InvalidAddress,
    ConnectionTimeout,
    HostUnreachable,
    ConnectionRefused,
    VersionNegotiationError,
    ConnectionIdle,
    ProtocolError,
    OperationAborted, 
}
```
## Exposing HTTP/2 and HTTP/3 protocol error codes

**Approved** | [#runtime/70684](https://github.com/dotnet/runtime/issues/70684#issuecomment-1169102091) | [Video](https://www.youtube.com/watch?v=gGErXh4z9_Y&t=1h22m29s)

* Let's start with a simpler design that just exposes the code as a long
    - We can add enums for the different protocols and derived exceptions later

```C#
namespace System.Net.Http;

public sealed class HttpProtocolException : IOException
{
    public HttpProtocolException(long errorCode, string message, Exception? innerException);
    public long ErrorCode { get; }
}
```
## WebSockets over HTTP/2

**Approved** | [#runtime/69669](https://github.com/dotnet/runtime/issues/69669#issuecomment-1169114552) | [Video](https://www.youtube.com/watch?v=gGErXh4z9_Y&t=1h48m41s)

* Looks good as proposed

```C#
namespace System.Net.WebSockets
{
    public partial class ClientWebSocket : WebSocket
    {
        // Existing
        // public Task ConnectAsync(Uri uri, CancellationToken cancellationToken);
        public Task ConnectAsync(Uri uri, HttpMessageInvoker invoker, CancellationToken cancellationToken);
    }

    public partial class ClientWebSocketOptions
    {
        public Version HttpVersion { get; [UnsupportedOSPlatform("browser")] set; };
        public HttpVersionPolicy HttpVersionPolicy { get; [UnsupportedOSPlatform("browser")] set; };
    }
}
namespace System.Net.Http
{
    public partial class HttpMethod
    {
        public static HttpMethod Connect { get; }
    }
}
namespace System.Net.Http.Headers
{
    public partial class HttpRequestHeaders : HttpHeaders
    {
        public string? Protocol { get; set; };
    }
}
```

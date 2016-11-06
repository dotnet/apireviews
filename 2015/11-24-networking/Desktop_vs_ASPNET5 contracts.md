```diff
---C:\Windows\Microsoft.NET\assembly\GAC_MSIL\System\v4.0_4.0.0.0__b77a5c561934e089\System.dll
+++\\dshulman-repro1\SystemNet\System.Net.Http\4.0.1.0\System.Net.Http.dll
 namespace System.Net {
  public class CookieCollection : ICollection, IEnumerable {
-   public bool IsReadOnly { get; }
-   public bool IsSynchronized { get; }
-   public object SyncRoot { get; }
+   bool System.Collections.ICollection.IsSynchronized { get; }
+   object System.Collections.ICollection.SyncRoot { get; }
-   public Cookie this[int index] { get; }
-   public void CopyTo(Array array, int index);
-   public void CopyTo(Cookie[] array, int index);
+   void System.Collections.ICollection.CopyTo(Array array, int index);
  }
  public class CookieContainer {
-   public CookieContainer(int capacity);
-   public CookieContainer(int capacity, int perDomainCapacity, int maxCookieSize);
-   public void Add(Cookie cookie);
-   public void Add(CookieCollection cookies);
  }
  public class CookieException : FormatException, ISerializable {
-   protected CookieException(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public static class Dns {
-   public static IAsyncResult BeginGetHostAddresses(string hostNameOrAddress, AsyncCallback requestCallback, object state);
-   public static IAsyncResult BeginGetHostByName(string hostName, AsyncCallback requestCallback, object stateObject);
-   public static IAsyncResult BeginGetHostEntry(IPAddress address, AsyncCallback requestCallback, object stateObject);
-   public static IAsyncResult BeginGetHostEntry(string hostNameOrAddress, AsyncCallback requestCallback, object stateObject);
-   public static IAsyncResult BeginResolve(string hostName, AsyncCallback requestCallback, object stateObject);
-   public static IPAddress[] EndGetHostAddresses(IAsyncResult asyncResult);
-   public static IPHostEntry EndGetHostByName(IAsyncResult asyncResult);
-   public static IPHostEntry EndGetHostEntry(IAsyncResult asyncResult);
-   public static IPHostEntry EndResolve(IAsyncResult asyncResult);
-   public static IPAddress[] GetHostAddresses(string hostNameOrAddress);
-   public static IPHostEntry GetHostByAddress(IPAddress address);
-   public static IPHostEntry GetHostByAddress(string address);
-   public static IPHostEntry GetHostByName(string hostName);
-   public static IPHostEntry GetHostEntry(IPAddress address);
-   public static IPHostEntry GetHostEntry(string hostNameOrAddress);
-   public static IPHostEntry Resolve(string hostName);
  }
  public class HttpWebRequest : WebRequest, ISerializable {
-   public HttpWebRequest();
-   protected HttpWebRequest(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public Uri Address { get; }
-   public virtual bool AllowAutoRedirect { get; set; }
-   public virtual bool AllowWriteStreamBuffering { get; set; }
-   public DecompressionMethods AutomaticDecompression { get; set; }
-   public X509CertificateCollection ClientCertificates { get; set; }
-   public string Connection { get; set; }
-   public override string ConnectionGroupName { get; set; }
-   public override long ContentLength { get; set; }
-   public HttpContinueDelegate ContinueDelegate { get; set; }
-   public DateTime Date { get; set; }
-   public static new RequestCachePolicy DefaultCachePolicy { get; set; }
-   public static int DefaultMaximumErrorResponseLength { get; set; }
-   public static int DefaultMaximumResponseHeadersLength { get; set; }
-   public string Expect { get; set; }
-   public string Host { get; set; }
-   public DateTime IfModifiedSince { get; set; }
-   public bool KeepAlive { get; set; }
-   public int MaximumAutomaticRedirections { get; set; }
-   public int MaximumResponseHeadersLength { get; set; }
-   public string MediaType { get; set; }
-   public bool Pipelined { get; set; }
-   public override bool PreAuthenticate { get; set; }
-   public Version ProtocolVersion { get; set; }
-   public override IWebProxy Proxy { get; set; }
-   public int ReadWriteTimeout { get; set; }
-   public string Referer { get; set; }
-   public bool SendChunked { get; set; }
-   public RemoteCertificateValidationCallback ServerCertificateValidationCallback { get; set; }
-   public ServicePoint ServicePoint { get; }
-   public override int Timeout { get; set; }
-   public string TransferEncoding { get; set; }
-   public bool UnsafeAuthenticatedConnectionSharing { get; set; }
-   public string UserAgent { get; set; }
-   public void AddRange(int range);
-   public void AddRange(int from, int to);
-   public void AddRange(long range);
-   public void AddRange(long from, long to);
-   public void AddRange(string rangeSpecifier, int range);
-   public void AddRange(string rangeSpecifier, int from, int to);
-   public void AddRange(string rangeSpecifier, long range);
-   public void AddRange(string rangeSpecifier, long from, long to);
-   public Stream EndGetRequestStream(IAsyncResult asyncResult, out TransportContext context);
-   protected override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public override Stream GetRequestStream();
-   public Stream GetRequestStream(out TransportContext context);
-   public override WebResponse GetResponse();
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public class HttpWebResponse : WebResponse, ISerializable {
-   public HttpWebResponse();
-   protected HttpWebResponse(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public string CharacterSet { get; }
-   public string ContentEncoding { get; }
    public virtual CookieCollection Cookies { get; set; }
-   public override bool IsMutuallyAuthenticated { get; }
-   public DateTime LastModified { get; }
-   public Version ProtocolVersion { get; }
-   public string Server { get; }
-   public override void Close();
-   protected override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public string GetResponseHeader(string headerName);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public class IPAddress {
-   public long Address { get; set; }
  }
  public class NetworkCredential : ICredentials, ICredentialsByHost {
-   public NetworkCredential(string userName, SecureString password);
-   public NetworkCredential(string userName, SecureString password, string domain);
-   public SecureString SecurePassword { get; set; }
  }
  public class ProtocolViolationException : InvalidOperationException, ISerializable {
-   protected ProtocolViolationException(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public abstract class TransportContext {
-   public virtual IEnumerable<TokenBinding> GetTlsTokenBindings();
  }
  public class WebException : InvalidOperationException, ISerializable {
-   protected WebException(SerializationInfo serializationInfo, StreamingContext streamingContext);
    public WebException(string message, Exception innerExceptioninner);
    ^ Immo Landwerth: That will cause issues when compiled against the contracts
    public WebException(string message, Exception innerExceptioninner, WebExceptionStatus status, WebResponse response);
-   public override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public sealed class WebHeaderCollection : NameValueCollectionIEnumerable, ISerializable {
-   protected WebHeaderCollection(SerializationInfo serializationInfo, StreamingContext streamingContext);
    public override string[] AllKeys { get; }
    public override int Count { get; }
-   public override NameObjectCollectionBase.KeysCollection Keys { get; }
+   public string this[string name] { get; set; }
-   public void Add(HttpRequestHeader header, string value);
-   public void Add(HttpResponseHeader header, string value);
-   public void Add(string header);
-   public override void Add(string name, string value);
-   protected void AddWithoutValidate(string headerName, string headerValue);
-   public override void Clear();
-   public override string Get(int index);
-   public override string Get(string name);
-   public override IEnumerator GetEnumerator();
-   public override string GetKey(int index);
-   public override void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public override string[] GetValues(int index);
-   public override string[] GetValues(string header);
-   public static bool IsRestricted(string headerName);
-   public static bool IsRestricted(string headerName, bool response);
-   public override void OnDeserialization(object sender);
-   public void Remove(HttpRequestHeader header);
-   public void Remove(HttpResponseHeader header);
    public override void Remove(string name);
-   public void Set(HttpRequestHeader header, string value);
-   public void Set(HttpResponseHeader header, string value);
-   public override void Set(string name, string value);
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
    ^ Immo Landwerth: Should this be an implicit? It seems desktop does this already does that on NameValueCollection.
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public byte[] ToByteArray();
  }
  public abstract class WebRequest : MarshalByRefObject, ISerializable {
-   protected WebRequest(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public AuthenticationLevel AuthenticationLevel { get; set; }
-   public virtual RequestCachePolicy CachePolicy { get; set; }
-   public virtual string ConnectionGroupName { get; set; }
-   public virtual long ContentLength { get; set; }
    public virtualabstract string ContentType { get; set; }
-   public virtual IWebRequestCreate CreatorInstance { get; }
-   public static RequestCachePolicy DefaultCachePolicy { get; set; }
    public virtualabstract WebHeaderCollection Headers { get; set; }
-   public TokenImpersonationLevel ImpersonationLevel { get; set; }
    public virtualabstract string Method { get; set; }
-   public virtual bool PreAuthenticate { get; set; }
    public virtualabstract Uri RequestUri { get; }
-   public virtual int Timeout { get; set; }
    public virtualabstract void Abort();
    public virtualabstract IAsyncResult BeginGetRequestStream(AsyncCallback callback, object state);
    public virtualabstract IAsyncResult BeginGetResponse(AsyncCallback callback, object state);
-   public static WebRequest CreateDefault(Uri requestUri);
    public virtualabstract Stream EndGetRequestStream(IAsyncResult asyncResult);
    public virtualabstract WebResponse EndGetResponse(IAsyncResult asyncResult);
-   protected virtual void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public virtual Stream GetRequestStream();
-   public virtual WebResponse GetResponse();
-   public static IWebProxy GetSystemWebProxy();
-   public static void RegisterPortableWebRequestCreator(IWebRequestCreate creator);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public abstract class WebResponse : MarshalByRefObject, IDisposable, ISerializable {
-   protected WebResponse(SerializationInfo serializationInfo, StreamingContext streamingContext);
    public virtualabstract long ContentLength { get; set; }
    public virtualabstract string ContentType { get; set; }
-   public virtual bool IsFromCache { get; }
-   public virtual bool IsMutuallyAuthenticated { get; }
    public virtualabstract Uri ResponseUri { get; }
-   public virtual void Close();
-   protected virtual void GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
    public virtualabstract Stream GetResponseStream();
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
 }
 namespace System.Net.Http {
+ public enum CookieUsePolicy {
+   IgnoreCookies = 0,
+   UseInternalCookieStoreOnly = 1,
+   UseSpecifiedCookieContainer = 2,
  }
+ public enum WindowsProxyUsePolicy {
+   DoNotUseProxy = 0,
+   UseCustomProxy = 3,
+   UseWinHttpProxy = 1,
+   UseWinInetProxy = 2,
  }
+ public class WinHttpHandler : HttpMessageHandler {
+   public WinHttpHandler();
+   public DecompressionMethods AutomaticDecompression { get; set; }
+   public bool AutomaticRedirection { get; set; }
+   public bool CheckCertificateRevocationList { get; set; }
+   public ClientCertificateOption ClientCertificateOption { get; set; }
+   public X509Certificate2Collection ClientCertificates { get; }
+   public TimeSpan ConnectTimeout { get; set; }
+   public CookieContainer CookieContainer { get; set; }
+   public CookieUsePolicy CookieUsePolicy { get; set; }
+   public ICredentials DefaultProxyCredentials { get; set; }
+   public int MaxAutomaticRedirections { get; set; }
+   public int MaxConnectionsPerServer { get; set; }
+   public int MaxResponseDrainSize { get; set; }
+   public int MaxResponseHeadersLength { get; set; }
+   public bool PreAuthenticate { get; set; }
+   public IWebProxy Proxy { get; set; }
+   public TimeSpan ReceiveDataTimeout { get; set; }
+   public TimeSpan ReceiveHeadersTimeout { get; set; }
+   public TimeSpan SendTimeout { get; set; }
+   public Func<HttpRequestMessage, X509Certificate2, X509Chain, SslPolicyErrors, bool> ServerCertificateValidationCallback { get; set; }
+   public ICredentials ServerCredentials { get; set; }
+   public SslProtocols SslProtocols { get; set; }
+   public WindowsProxyUsePolicy WindowsProxyUsePolicy { get; set; }
+   protected override void Dispose(bool disposing);
+   protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
  }
 }
 namespace System.Net.Http.Headers {
  public class AuthenticationHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class CacheControlHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class ContentDispositionHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class ContentRangeHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class EntityTagHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class MediaTypeHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public sealed class MediaTypeWithQualityHeaderValue : MediaTypeHeaderValue, ICloneable {
-   object System.ICloneable.Clone();
  }
  public class NameValueHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class NameValueWithParametersHeaderValue : NameValueHeaderValue, ICloneable {
-   object System.ICloneable.Clone();
  }
  public class ProductHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class ProductInfoHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class RangeConditionHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class RangeHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class RangeItemHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class RetryConditionHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class StringWithQualityHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class TransferCodingHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public sealed class TransferCodingWithQualityHeaderValue : TransferCodingHeaderValue, ICloneable {
-   object System.ICloneable.Clone();
  }
  public class ViaHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
  public class WarningHeaderValue : ICloneable {
-   object System.ICloneable.Clone();
  }
 }
 namespace System.Net.NetworkInformation {
  public abstract class IPGlobalProperties {
-   public virtual IAsyncResult BeginGetUnicastAddresses(AsyncCallback callback, object state);
-   public virtual UnicastIPAddressInformationCollection EndGetUnicastAddresses(IAsyncResult asyncResult);
-   public virtual UnicastIPAddressInformationCollection GetUnicastAddresses();
  }
  public static class NetworkChange {
-   public NetworkChange();
-   public static event NetworkAvailabilityChangedEventHandler NetworkAvailabilityChanged;
-   public static void RegisterNetworkChange(NetworkChange nc);
  }
  public class NetworkInformationException : Win32ExceptionException {
-   protected NetworkInformationException(SerializationInfo serializationInfo, StreamingContext streamingContext);
    public override int ErrorCode { get; }
  }
  public abstract class NetworkInterface {
-   public virtual IPv4InterfaceStatistics GetIPv4Statistics();
  }
  public class Ping : ComponentIDisposable {
-   public event PingCompletedEventHandler PingCompleted;
+   public void Dispose();
    protected overridevirtual void Dispose(bool disposing);
-   protected void OnPingCompleted(PingCompletedEventArgs e);
-   public PingReply Send(IPAddress address);
-   public PingReply Send(IPAddress address, int timeout);
-   public PingReply Send(IPAddress address, int timeout, byte[] buffer);
-   public PingReply Send(IPAddress address, int timeout, byte[] buffer, PingOptions options);
-   public PingReply Send(string hostNameOrAddress);
-   public PingReply Send(string hostNameOrAddress, int timeout);
-   public PingReply Send(string hostNameOrAddress, int timeout, byte[] buffer);
-   public PingReply Send(string hostNameOrAddress, int timeout, byte[] buffer, PingOptions options);
-   public void SendAsync(IPAddress address, int timeout, byte[] buffer, PingOptions options, object userToken);
-   public void SendAsync(IPAddress address, int timeout, byte[] buffer, object userToken);
-   public void SendAsync(IPAddress address, int timeout, object userToken);
-   public void SendAsync(IPAddress address, object userToken);
-   public void SendAsync(string hostNameOrAddress, int timeout, byte[] buffer, PingOptions options, object userToken);
-   public void SendAsync(string hostNameOrAddress, int timeout, byte[] buffer, object userToken);
-   public void SendAsync(string hostNameOrAddress, int timeout, object userToken);
-   public void SendAsync(string hostNameOrAddress, object userToken);
-   public void SendAsyncCancel();
  }
  public class PingException : InvalidOperationException {
-   protected PingException(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
 }
 namespace System.Net.Security {
  public class NegotiateStream : AuthenticatedStream {
-   public virtual IAsyncResult BeginAuthenticateAsClient(AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsClient(NetworkCredential credential, ChannelBinding binding, string targetName, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsClient(NetworkCredential credential, ChannelBinding binding, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsClient(NetworkCredential credential, string targetName, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsClient(NetworkCredential credential, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(NetworkCredential credential, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(NetworkCredential credential, ExtendedProtectionPolicy policy, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(ExtendedProtectionPolicy policy, AsyncCallback asyncCallback, object asyncState);
-   public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback asyncCallback, object asyncState);
-   public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback asyncCallback, object asyncState);
-   protected override void Dispose(bool disposing);
-   public virtual void EndAuthenticateAsClient(IAsyncResult asyncResult);
-   public virtual void EndAuthenticateAsServer(IAsyncResult asyncResult);
-   public override int EndRead(IAsyncResult asyncResult);
-   public override void EndWrite(IAsyncResult asyncResult);
  }
  public class SslStream : AuthenticatedStream {
-   public virtual IAsyncResult BeginAuthenticateAsClient(string targetHost, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsClient(string targetHost, X509CertificateCollection clientCertificates, SslProtocols enabledSslProtocols, bool checkCertificateRevocation, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(X509Certificate serverCertificate, AsyncCallback asyncCallback, object asyncState);
-   public virtual IAsyncResult BeginAuthenticateAsServer(X509Certificate serverCertificate, bool clientCertificateRequired, SslProtocols enabledSslProtocols, bool checkCertificateRevocation, AsyncCallback asyncCallback, object asyncState);
-   public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback asyncCallback, object asyncState);
-   public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback asyncCallback, object asyncState);
-   protected override void Dispose(bool disposing);
-   public virtual void EndAuthenticateAsClient(IAsyncResult asyncResult);
-   public virtual void EndAuthenticateAsServer(IAsyncResult asyncResult);
-   public override int EndRead(IAsyncResult asyncResult);
-   public override void EndWrite(IAsyncResult asyncResult);
  }
 }
 namespace System.Net.Sockets {
  public enum AddressFamily {
-   Max = 29,
  }
  public class NetworkStream : Stream {
-   public NetworkStream(Socket socket, FileAccess access);
-   public NetworkStream(Socket socket, FileAccess access, bool ownsSocket);
-   protected bool Readable { get; set; }
-   protected Socket Socket { get; }
-   protected bool Writeable { get; set; }
-   public override IAsyncResult BeginRead(byte[] buffer, int offset, int size, AsyncCallback callback, object state);
-   public override IAsyncResult BeginWrite(byte[] buffer, int offset, int size, AsyncCallback callback, object state);
-   public void Close(int timeout);
-   public override int EndRead(IAsyncResult asyncResult);
-   public override void EndWrite(IAsyncResult asyncResult);
+   public override Task<int> ReadAsync(byte[] buffer, int offset, int size, CancellationToken cancellationToken);
+   public override Task WriteAsync(byte[] buffer, int offset, int size, CancellationToken cancellationToken);
  }
  public class Socket : IDisposable {
-   public Socket(SocketInformation socketInformation);
-   public IntPtr Handle { get; }
-   public static bool SupportsIPv4 { get; }
-   public static bool SupportsIPv6 { get; }
-   public bool UseOnlyOverlappedIO { get; set; }
-   public IAsyncResult BeginAccept(AsyncCallback callback, object state);
-   public IAsyncResult BeginAccept(int receiveSize, AsyncCallback callback, object state);
-   public IAsyncResult BeginAccept(Socket acceptSocket, int receiveSize, AsyncCallback callback, object state);
-   public IAsyncResult BeginConnect(EndPoint remoteEP, AsyncCallback callback, object state);
-   public IAsyncResult BeginConnect(IPAddress address, int port, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginConnect(IPAddress[] addresses, int port, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginConnect(string host, int port, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginDisconnect(bool reuseSocket, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceive(byte[] buffer, int offset, int size, SocketFlags socketFlags, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceive(byte[] buffer, int offset, int size, SocketFlags socketFlags, out SocketError errorCode, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceive(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceive(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, out SocketError errorCode, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceiveFrom(byte[] buffer, int offset, int size, SocketFlags socketFlags, ref EndPoint remoteEP, AsyncCallback callback, object state);
-   public IAsyncResult BeginReceiveMessageFrom(byte[] buffer, int offset, int size, SocketFlags socketFlags, ref EndPoint remoteEP, AsyncCallback callback, object state);
-   public IAsyncResult BeginSend(byte[] buffer, int offset, int size, SocketFlags socketFlags, AsyncCallback callback, object state);
-   public IAsyncResult BeginSend(byte[] buffer, int offset, int size, SocketFlags socketFlags, out SocketError errorCode, AsyncCallback callback, object state);
-   public IAsyncResult BeginSend(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, AsyncCallback callback, object state);
-   public IAsyncResult BeginSend(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, out SocketError errorCode, AsyncCallback callback, object state);
-   public IAsyncResult BeginSendFile(string fileName, AsyncCallback callback, object state);
-   public IAsyncResult BeginSendFile(string fileName, byte[] preBuffer, byte[] postBuffer, TransmitFileOptions flags, AsyncCallback callback, object state);
-   public IAsyncResult BeginSendTo(byte[] buffer, int offset, int size, SocketFlags socketFlags, EndPoint remoteEP, AsyncCallback callback, object state);
-   public void Close();
-   public void Close(int timeout);
-   public void Disconnect(bool reuseSocket);
-   public bool DisconnectAsync(SocketAsyncEventArgs e);
-   public SocketInformation DuplicateAndClose(int targetProcessId);
-   public Socket EndAccept(out byte[] buffer, IAsyncResult asyncResult);
-   public Socket EndAccept(out byte[] buffer, out int bytesTransferred, IAsyncResult asyncResult);
-   public Socket EndAccept(IAsyncResult asyncResult);
-   public void EndConnect(IAsyncResult asyncResult);
-   public void EndDisconnect(IAsyncResult asyncResult);
-   public int EndReceive(IAsyncResult asyncResult);
-   public int EndReceive(IAsyncResult asyncResult, out SocketError errorCode);
-   public int EndReceiveFrom(IAsyncResult asyncResult, ref EndPoint endPoint);
-   public int EndReceiveMessageFrom(IAsyncResult asyncResult, ref SocketFlags socketFlags, ref EndPoint endPoint, out IPPacketInformation ipPacketInformation);
-   public int EndSend(IAsyncResult asyncResult);
-   public int EndSend(IAsyncResult asyncResult, out SocketError errorCode);
-   public void EndSendFile(IAsyncResult asyncResult);
-   public int EndSendTo(IAsyncResult asyncResult);
-   public void SendFile(string fileName);
-   public void SendFile(string fileName, byte[] preBuffer, byte[] postBuffer, TransmitFileOptions flags);
-   public void SetIPProtectionLevel(IPProtectionLevel level);
  }
  public class SocketAsyncEventArgs : EventArgs, IDisposable {
-   public bool DisconnectReuseSocket { get; set; }
-   public TransmitFileOptions SendPacketsFlags { get; set; }
-   public SocketClientAccessPolicyProtocol SocketClientAccessPolicyProtocol { get; set; }
  }
  public class SocketException : Win32ExceptionException {
-   protected SocketException(SerializationInfo serializationInfo, StreamingContext streamingContext);
-   public override int ErrorCode { get; }
  }
  public enum SocketFlags {
    ^ Immo Landwerth: Leaving out is fine, but we should add a comment to the code base in .NET Core that says "Don't use slot 16"
-   MaxIOVectorLength = 16,
  }
  public enum SocketOptionName {
-   ReuseUnicastPort = 12295,
    ^ Immo Landwerth: We should add back. While it's a Windows specific API, it make sense to have this on .NET Core as well.
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct SocketReceiveFromResult {
    ^ Immo Landwerth: Should these be properties as opposed to fields?
+   public int ReceivedBytes;
+   public EndPoint RemoteEndPoint;
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct SocketReceiveMessageFromResult {
    ^ Immo Landwerth: Should these be properties as opposed to fields?
+   public int ReceivedBytes;
+   public EndPoint RemoteEndPoint;
+   public IPPacketInformation PacketInformation;
+   public SocketFlags SocketFlags;
  }
+ public static class SocketTaskExtensions {
+   public static Task<Socket> AcceptAsync(this Socket socket);
+   public static Task<Socket> AcceptAsync(this Socket socket, Socket acceptSocket);
+   public static Task ConnectAsync(this Socket socket, EndPoint remoteEP);
+   public static Task ConnectAsync(this Socket socket, IPAddress address, int port);
+   public static Task ConnectAsync(this Socket socket, IPAddress[] addresses, int port);
+   public static Task ConnectAsync(this Socket socket, string host, int port);
+   public static Task<int> ReceiveAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags);
+   public static Task<int> ReceiveAsync(this Socket socket, IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
+   public static Task<SocketReceiveFromResult> ReceiveFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
+   public static Task<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
+   public static Task<int> SendAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags);
+   public static Task<int> SendAsync(this Socket socket, IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
+   public static Task<int> SendToAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP);
  }
  public class TcpClient : IDisposable {
-   public TcpClient(IPEndPoint localEP);
-   public TcpClient(string hostname, int port);
-   public IAsyncResult BeginConnect(IPAddress address, int port, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginConnect(IPAddress[] addresses, int port, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginConnect(string host, int port, AsyncCallback requestCallback, object state);
-   public void Close();
-   public void Connect(IPAddress address, int port);
-   public void Connect(IPAddress[] ipAddresses, int port);
-   public void Connect(IPEndPoint remoteEP);
-   public void Connect(string hostname, int port);
-   public void EndConnect(IAsyncResult asyncResult);
  }
  public class TcpListener {
-   public TcpListener(int port);
-   public Socket AcceptSocket();
-   public TcpClient AcceptTcpClient();
-   public void AllowNatTraversal(bool allowed);
-   public IAsyncResult BeginAcceptSocket(AsyncCallback callback, object state);
-   public IAsyncResult BeginAcceptTcpClient(AsyncCallback callback, object state);
-   public static TcpListener Create(int port);
-   public Socket EndAcceptSocket(IAsyncResult asyncResult);
-   public TcpClient EndAcceptTcpClient(IAsyncResult asyncResult);
  }
  public class UdpClient : IDisposable {
-   public UdpClient(string hostname, int port);
-   public void AllowNatTraversal(bool allowed);
-   public IAsyncResult BeginReceive(AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginSend(byte[] datagram, int bytes, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginSend(byte[] datagram, int bytes, IPEndPoint endPoint, AsyncCallback requestCallback, object state);
-   public IAsyncResult BeginSend(byte[] datagram, int bytes, string hostname, int port, AsyncCallback requestCallback, object state);
-   public void Close();
-   public void Connect(IPAddress addr, int port);
-   public void Connect(IPEndPoint endPoint);
-   public void Connect(string hostname, int port);
-   public byte[] EndReceive(IAsyncResult asyncResult, ref IPEndPoint remoteEP);
-   public int EndSend(IAsyncResult asyncResult);
-   public byte[] Receive(ref IPEndPoint remoteEP);
-   public int Send(byte[] dgram, int bytes);
-   public int Send(byte[] dgram, int bytes, IPEndPoint endPoint);
-   public int Send(byte[] dgram, int bytes, string hostname, int port);
-   public Task<int> SendAsync(byte[] datagram, int bytes);
    ^ Immo Landwerth: Why was this removed?
    | Immo Landwerth: --> Because it doesn't take an end point and we removed the .ctor that provides a default end point.
  }
 }
 namespace System.Net.WebSockets {
  public sealed class ClientWebSocketOptions {
-   public bool UseDefaultCredentials { get; set; }
-   public void SetBuffer(int receiveBufferSize, int sendBufferSize);
-   public void SetBuffer(int receiveBufferSize, int sendBufferSize, ArraySegment<byte> buffer);
  }
  public abstract class WebSocket : IDisposable {
-   public static TimeSpan DefaultKeepAliveInterval { get; }
-   public static ArraySegment<byte> CreateClientBuffer(int receiveBufferSize, int sendBufferSize);
-   public static WebSocket CreateClientWebSocket(Stream innerStream, string subProtocol, int receiveBufferSize, int sendBufferSize, TimeSpan keepAliveInterval, bool useZeroMaskingKey, ArraySegment<byte> internalBuffer);
-   public static ArraySegment<byte> CreateServerBuffer(int receiveBufferSize);
-   public static bool IsApplicationTargeting45();
-   protected static bool IsStateTerminal(WebSocketState state);
-   public static void RegisterPrefixes();
-   protected static void ThrowOnInvalidState(WebSocketState state, params WebSocketState[] validStates);
  }
  public sealed class WebSocketException : Win32ExceptionException {
-   public WebSocketException();
    public override int ErrorCode { get; }
-   public override void GetObjectData(SerializationInfo info, StreamingContext context);
  }
 }
 namespace System.Security.Authentication {
  public class AuthenticationException : SystemExceptionException {
-   protected AuthenticationException(SerializationInfo serializationInfo, StreamingContext streamingContext);
  }
  public enum SslProtocols {
-   Default = 240,
  }
 }
 namespace System.Security.Authentication.ExtendedProtection {
  public abstract class ChannelBinding : SafeHandleZeroOrMinusOneIsInvalidSafeHandle {
  }
  public class ExtendedProtectionPolicy : ISerializable {
-   protected ExtendedProtectionPolicy(SerializationInfo info, StreamingContext context);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
  }
  public class ServiceNameCollection : ReadOnlyCollectionBaseICollection, IEnumerable {
+   public int Count { get; }
+   bool System.Collections.ICollection.IsSynchronized { get; }
+   object System.Collections.ICollection.SyncRoot { get; }
+   public IEnumerator GetEnumerator();
+   void System.Collections.ICollection.CopyTo(Array array, int index);
  }
 }
```

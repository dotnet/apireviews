```diff
---\\dshulman-repro1\SystemNet\System.Net.Http\4.0.1.0\System.Net.Http.dll
   assembly System.Net.Http {
  namespace System.Net.Http {
    public class ByteArrayContent : HttpContent {
      public ByteArrayContent(byte[] content);
      public ByteArrayContent(byte[] content, int offset, int count);
      protected override Task<Stream> CreateContentReadStreamAsync();
      protected override Task SerializeToStreamAsync(Stream stream, TransportContext context);
      protected internal override bool TryComputeLength(out long length);
    }
    public enum ClientCertificateOption {
      Automatic = 1,
      Manual = 0,
    }
    public abstract class DelegatingHandler : HttpMessageHandler {
      protected DelegatingHandler();
      protected DelegatingHandler(HttpMessageHandler innerHandler);
      public HttpMessageHandler InnerHandler { get; set; }
      protected override void Dispose(bool disposing);
      protected internal override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public class FormUrlEncodedContent : ByteArrayContent {
      public FormUrlEncodedContent(IEnumerable<KeyValuePair<string, string>> nameValueCollection);
    }
    public class HttpClient : HttpMessageInvoker {
      public HttpClient();
      public HttpClient(HttpMessageHandler handler);
      public HttpClient(HttpMessageHandler handler, bool disposeHandler);
      public Uri BaseAddress { get; set; }
      public HttpRequestHeaders DefaultRequestHeaders { get; }
      public long MaxResponseContentBufferSize { get; set; }
      public TimeSpan Timeout { get; set; }
      public void CancelPendingRequests();
      public Task<HttpResponseMessage> DeleteAsync(string requestUri);
      public Task<HttpResponseMessage> DeleteAsync(string requestUri, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> DeleteAsync(Uri requestUri);
      public Task<HttpResponseMessage> DeleteAsync(Uri requestUri, CancellationToken cancellationToken);
      protected override void Dispose(bool disposing);
      public Task<HttpResponseMessage> GetAsync(string requestUri);
      public Task<HttpResponseMessage> GetAsync(string requestUri, HttpCompletionOption completionOption);
      public Task<HttpResponseMessage> GetAsync(string requestUri, HttpCompletionOption completionOption, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> GetAsync(string requestUri, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> GetAsync(Uri requestUri);
      public Task<HttpResponseMessage> GetAsync(Uri requestUri, HttpCompletionOption completionOption);
      public Task<HttpResponseMessage> GetAsync(Uri requestUri, HttpCompletionOption completionOption, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> GetAsync(Uri requestUri, CancellationToken cancellationToken);
      public Task<byte[]> GetByteArrayAsync(string requestUri);
      public Task<byte[]> GetByteArrayAsync(Uri requestUri);
      public Task<Stream> GetStreamAsync(string requestUri);
      public Task<Stream> GetStreamAsync(Uri requestUri);
      public Task<string> GetStringAsync(string requestUri);
      public Task<string> GetStringAsync(Uri requestUri);
      public Task<HttpResponseMessage> PostAsync(string requestUri, HttpContent content);
      public Task<HttpResponseMessage> PostAsync(string requestUri, HttpContent content, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> PostAsync(Uri requestUri, HttpContent content);
      public Task<HttpResponseMessage> PostAsync(Uri requestUri, HttpContent content, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> PutAsync(string requestUri, HttpContent content);
      public Task<HttpResponseMessage> PutAsync(string requestUri, HttpContent content, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> PutAsync(Uri requestUri, HttpContent content);
      public Task<HttpResponseMessage> PutAsync(Uri requestUri, HttpContent content, CancellationToken cancellationToken);
      public Task<HttpResponseMessage> SendAsync(HttpRequestMessage request);
      public Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, HttpCompletionOption completionOption);
      public Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, HttpCompletionOption completionOption, CancellationToken cancellationToken);
      public override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public class HttpClientHandler : HttpMessageHandler {
      public HttpClientHandler();
      public bool AllowAutoRedirect { get; set; }
      public DecompressionMethods AutomaticDecompression { get; set; }
      public ClientCertificateOption ClientCertificateOptions { get; set; }
      public CookieContainer CookieContainer { get; set; }
      public ICredentials Credentials { get; set; }
      public int MaxAutomaticRedirections { get; set; }
      public long MaxRequestContentBufferSize { get; set; }
      public bool PreAuthenticate { get; set; }
      public IWebProxy Proxy { get; set; }
      public virtual bool SupportsAutomaticDecompression { get; }
      public virtual bool SupportsProxy { get; }
      public virtual bool SupportsRedirectConfiguration { get; }
      public bool UseCookies { get; set; }
      public bool UseDefaultCredentials { get; set; }
      public bool UseProxy { get; set; }
      protected override void Dispose(bool disposing);
      protected internal override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public enum HttpCompletionOption {
      ResponseContentRead = 0,
      ResponseHeadersRead = 1,
    }
    public abstract class HttpContent : IDisposable {
      protected HttpContent();
      public HttpContentHeaders Headers { get; }
      public Task CopyToAsync(Stream stream);
      public Task CopyToAsync(Stream stream, TransportContext context);
      protected virtual Task<Stream> CreateContentReadStreamAsync();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public Task LoadIntoBufferAsync();
      public Task LoadIntoBufferAsync(long maxBufferSize);
      public Task<byte[]> ReadAsByteArrayAsync();
      public Task<Stream> ReadAsStreamAsync();
      public Task<string> ReadAsStringAsync();
      protected abstract Task SerializeToStreamAsync(Stream stream, TransportContext context);
      protected internal abstract bool TryComputeLength(out long length);
    }
    public abstract class HttpMessageHandler : IDisposable {
      protected HttpMessageHandler();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      protected internal abstract Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public class HttpMessageInvoker : IDisposable {
      public HttpMessageInvoker(HttpMessageHandler handler);
      public HttpMessageInvoker(HttpMessageHandler handler, bool disposeHandler);
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public virtual Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public class HttpMethod : IEquatable<HttpMethod> {
      public HttpMethod(string method);
      public static HttpMethod Delete { get; }
      public static HttpMethod Get { get; }
      public static HttpMethod Head { get; }
      public string Method { get; }
      public static HttpMethod Options { get; }
      public static HttpMethod Post { get; }
      public static HttpMethod Put { get; }
      public static HttpMethod Trace { get; }
      public bool Equals(HttpMethod other);
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static bool operator ==(HttpMethod left, HttpMethod right);
      public static bool operator !=(HttpMethod left, HttpMethod right);
      public override string ToString();
    }
    public class HttpRequestException : Exception {
      public HttpRequestException();
      public HttpRequestException(string message);
      public HttpRequestException(string message, Exception inner);
    }
    public class HttpRequestMessage : IDisposable {
      public HttpRequestMessage();
      public HttpRequestMessage(HttpMethod method, string requestUri);
      public HttpRequestMessage(HttpMethod method, Uri requestUri);
      public HttpContent Content { get; set; }
      public HttpRequestHeaders Headers { get; }
      public HttpMethod Method { get; set; }
      public IDictionary<string, object> Properties { get; }
      public Uri RequestUri { get; set; }
      public Version Version { get; set; }
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public override string ToString();
    }
    public class HttpResponseMessage : IDisposable {
      public HttpResponseMessage();
      public HttpResponseMessage(HttpStatusCode statusCode);
      public HttpContent Content { get; set; }
      public HttpResponseHeaders Headers { get; }
      public bool IsSuccessStatusCode { get; }
      public string ReasonPhrase { get; set; }
      public HttpRequestMessage RequestMessage { get; set; }
      public HttpStatusCode StatusCode { get; set; }
      public Version Version { get; set; }
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public HttpResponseMessage EnsureSuccessStatusCode();
      public override string ToString();
    }
    public abstract class MessageProcessingHandler : DelegatingHandler {
      protected MessageProcessingHandler();
      protected MessageProcessingHandler(HttpMessageHandler innerHandler);
      protected abstract HttpRequestMessage ProcessRequest(HttpRequestMessage request, CancellationToken cancellationToken);
      protected abstract HttpResponseMessage ProcessResponse(HttpResponseMessage response, CancellationToken cancellationToken);
      protected internal sealed override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
    public class MultipartContent : HttpContent, IEnumerable, IEnumerable<HttpContent> {
      public MultipartContent();
      public MultipartContent(string subtype);
      public MultipartContent(string subtype, string boundary);
      public virtual void Add(HttpContent content);
      protected override void Dispose(bool disposing);
      public IEnumerator<HttpContent> GetEnumerator();
      protected override Task SerializeToStreamAsync(Stream stream, TransportContext context);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      protected internal override bool TryComputeLength(out long length);
    }
    public class MultipartFormDataContent : MultipartContent {
      public MultipartFormDataContent();
      public MultipartFormDataContent(string boundary);
      public override void Add(HttpContent content);
      public void Add(HttpContent content, string name);
      public void Add(HttpContent content, string name, string fileName);
    }
    public class StreamContent : HttpContent {
      public StreamContent(Stream content);
      public StreamContent(Stream content, int bufferSize);
      protected override Task<Stream> CreateContentReadStreamAsync();
      protected override void Dispose(bool disposing);
      protected override Task SerializeToStreamAsync(Stream stream, TransportContext context);
      protected internal override bool TryComputeLength(out long length);
    }
    public class StringContent : ByteArrayContent {
      public StringContent(string content);
      public StringContent(string content, Encoding encoding);
      public StringContent(string content, Encoding encoding, string mediaType);
    }
  }
  namespace System.Net.Http.Headers {
    public class AuthenticationHeaderValue {
      public AuthenticationHeaderValue(string scheme);
      public AuthenticationHeaderValue(string scheme, string parameter);
      public string Parameter { get; }
      public string Scheme { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static AuthenticationHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out AuthenticationHeaderValue parsedValue);
    }
    public class CacheControlHeaderValue {
      public CacheControlHeaderValue();
      public ICollection<NameValueHeaderValue> Extensions { get; }
      public Nullable<TimeSpan> MaxAge { get; set; }
      public bool MaxStale { get; set; }
      public Nullable<TimeSpan> MaxStaleLimit { get; set; }
      public Nullable<TimeSpan> MinFresh { get; set; }
      public bool MustRevalidate { get; set; }
      public bool NoCache { get; set; }
      public ICollection<string> NoCacheHeaders { get; }
      public bool NoStore { get; set; }
      public bool NoTransform { get; set; }
      public bool OnlyIfCached { get; set; }
      public bool Private { get; set; }
      public ICollection<string> PrivateHeaders { get; }
      public bool ProxyRevalidate { get; set; }
      public bool Public { get; set; }
      public Nullable<TimeSpan> SharedMaxAge { get; set; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static CacheControlHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out CacheControlHeaderValue parsedValue);
    }
    public class ContentDispositionHeaderValue {
      protected ContentDispositionHeaderValue(ContentDispositionHeaderValue source);
      public ContentDispositionHeaderValue(string dispositionType);
      public Nullable<DateTimeOffset> CreationDate { get; set; }
      public string DispositionType { get; set; }
      public string FileName { get; set; }
      public string FileNameStar { get; set; }
      public Nullable<DateTimeOffset> ModificationDate { get; set; }
      public string Name { get; set; }
      public ICollection<NameValueHeaderValue> Parameters { get; }
      public Nullable<DateTimeOffset> ReadDate { get; set; }
      public Nullable<long> Size { get; set; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static ContentDispositionHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out ContentDispositionHeaderValue parsedValue);
    }
    public class ContentRangeHeaderValue {
      public ContentRangeHeaderValue(long length);
      public ContentRangeHeaderValue(long from, long to);
      public ContentRangeHeaderValue(long from, long to, long length);
      public Nullable<long> From { get; }
      public bool HasLength { get; }
      public bool HasRange { get; }
      public Nullable<long> Length { get; }
      public Nullable<long> To { get; }
      public string Unit { get; set; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static ContentRangeHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out ContentRangeHeaderValue parsedValue);
    }
    public class EntityTagHeaderValue {
      public EntityTagHeaderValue(string tag);
      public EntityTagHeaderValue(string tag, bool isWeak);
      public static EntityTagHeaderValue Any { get; }
      public bool IsWeak { get; }
      public string Tag { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static EntityTagHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out EntityTagHeaderValue parsedValue);
    }
    public sealed class HttpContentHeaders : HttpHeaders {
      public ICollection<string> Allow { get; }
      public ContentDispositionHeaderValue ContentDisposition { get; set; }
      public ICollection<string> ContentEncoding { get; }
      public ICollection<string> ContentLanguage { get; }
      public Nullable<long> ContentLength { get; set; }
      public Uri ContentLocation { get; set; }
      public byte[] ContentMD5 { get; set; }
      public ContentRangeHeaderValue ContentRange { get; set; }
      public MediaTypeHeaderValue ContentType { get; set; }
      public Nullable<DateTimeOffset> Expires { get; set; }
      public Nullable<DateTimeOffset> LastModified { get; set; }
    }
    public abstract class HttpHeaders : IEnumerable, IEnumerable<KeyValuePair<string, IEnumerable<string>>> {
      protected HttpHeaders();
      public void Add(string name, IEnumerable<string> values);
      public void Add(string name, string value);
      public void Clear();
      public bool Contains(string name);
      public IEnumerator<KeyValuePair<string, IEnumerable<string>>> GetEnumerator();
      public IEnumerable<string> GetValues(string name);
      public bool Remove(string name);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public override string ToString();
      public bool TryAddWithoutValidation(string name, IEnumerable<string> values);
      public bool TryAddWithoutValidation(string name, string value);
      public bool TryGetValues(string name, out IEnumerable<string> values);
    }
    public sealed class HttpHeaderValueCollection<T> : ICollection<T>, IEnumerable, IEnumerable<T> where T : class {
      public int Count { get; }
      public bool IsReadOnly { get; }
      public void Add(T item);
      public void Clear();
      public bool Contains(T item);
      public void CopyTo(T[] array, int arrayIndex);
      public IEnumerator<T> GetEnumerator();
      public void ParseAdd(string input);
      public bool Remove(T item);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public override string ToString();
      public bool TryParseAdd(string input);
    }
    public sealed class HttpRequestHeaders : HttpHeaders {
      public HttpHeaderValueCollection<MediaTypeWithQualityHeaderValue> Accept { get; }
      public HttpHeaderValueCollection<StringWithQualityHeaderValue> AcceptCharset { get; }
      public HttpHeaderValueCollection<StringWithQualityHeaderValue> AcceptEncoding { get; }
      public HttpHeaderValueCollection<StringWithQualityHeaderValue> AcceptLanguage { get; }
      public AuthenticationHeaderValue Authorization { get; set; }
      public CacheControlHeaderValue CacheControl { get; set; }
      public HttpHeaderValueCollection<string> Connection { get; }
      public Nullable<bool> ConnectionClose { get; set; }
      public Nullable<DateTimeOffset> Date { get; set; }
      public HttpHeaderValueCollection<NameValueWithParametersHeaderValue> Expect { get; }
      public Nullable<bool> ExpectContinue { get; set; }
      public string From { get; set; }
      public string Host { get; set; }
      public HttpHeaderValueCollection<EntityTagHeaderValue> IfMatch { get; }
      public Nullable<DateTimeOffset> IfModifiedSince { get; set; }
      public HttpHeaderValueCollection<EntityTagHeaderValue> IfNoneMatch { get; }
      public RangeConditionHeaderValue IfRange { get; set; }
      public Nullable<DateTimeOffset> IfUnmodifiedSince { get; set; }
      public Nullable<int> MaxForwards { get; set; }
      public HttpHeaderValueCollection<NameValueHeaderValue> Pragma { get; }
      public AuthenticationHeaderValue ProxyAuthorization { get; set; }
      public RangeHeaderValue Range { get; set; }
      public Uri Referrer { get; set; }
      public HttpHeaderValueCollection<TransferCodingWithQualityHeaderValue> TE { get; }
      public HttpHeaderValueCollection<string> Trailer { get; }
      public HttpHeaderValueCollection<TransferCodingHeaderValue> TransferEncoding { get; }
      public Nullable<bool> TransferEncodingChunked { get; set; }
      public HttpHeaderValueCollection<ProductHeaderValue> Upgrade { get; }
      public HttpHeaderValueCollection<ProductInfoHeaderValue> UserAgent { get; }
      public HttpHeaderValueCollection<ViaHeaderValue> Via { get; }
      public HttpHeaderValueCollection<WarningHeaderValue> Warning { get; }
    }
    public sealed class HttpResponseHeaders : HttpHeaders {
      public HttpHeaderValueCollection<string> AcceptRanges { get; }
      public Nullable<TimeSpan> Age { get; set; }
      public CacheControlHeaderValue CacheControl { get; set; }
      public HttpHeaderValueCollection<string> Connection { get; }
      public Nullable<bool> ConnectionClose { get; set; }
      public Nullable<DateTimeOffset> Date { get; set; }
      public EntityTagHeaderValue ETag { get; set; }
      public Uri Location { get; set; }
      public HttpHeaderValueCollection<NameValueHeaderValue> Pragma { get; }
      public HttpHeaderValueCollection<AuthenticationHeaderValue> ProxyAuthenticate { get; }
      public RetryConditionHeaderValue RetryAfter { get; set; }
      public HttpHeaderValueCollection<ProductInfoHeaderValue> Server { get; }
      public HttpHeaderValueCollection<string> Trailer { get; }
      public HttpHeaderValueCollection<TransferCodingHeaderValue> TransferEncoding { get; }
      public Nullable<bool> TransferEncodingChunked { get; set; }
      public HttpHeaderValueCollection<ProductHeaderValue> Upgrade { get; }
      public HttpHeaderValueCollection<string> Vary { get; }
      public HttpHeaderValueCollection<ViaHeaderValue> Via { get; }
      public HttpHeaderValueCollection<WarningHeaderValue> Warning { get; }
      public HttpHeaderValueCollection<AuthenticationHeaderValue> WwwAuthenticate { get; }
    }
    public class MediaTypeHeaderValue {
      protected MediaTypeHeaderValue(MediaTypeHeaderValue source);
      public MediaTypeHeaderValue(string mediaType);
      public string CharSet { get; set; }
      public string MediaType { get; set; }
      public ICollection<NameValueHeaderValue> Parameters { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static MediaTypeHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out MediaTypeHeaderValue parsedValue);
    }
    public sealed class MediaTypeWithQualityHeaderValue : MediaTypeHeaderValue {
      public MediaTypeWithQualityHeaderValue(string mediaType);
      public MediaTypeWithQualityHeaderValue(string mediaType, double quality);
      public Nullable<double> Quality { get; set; }
      public static new MediaTypeWithQualityHeaderValue Parse(string input);
      public static bool TryParse(string input, out MediaTypeWithQualityHeaderValue parsedValue);
    }
    public class NameValueHeaderValue {
      protected NameValueHeaderValue(NameValueHeaderValue source);
      public NameValueHeaderValue(string name);
      public NameValueHeaderValue(string name, string value);
      public string Name { get; }
      public string Value { get; set; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static NameValueHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out NameValueHeaderValue parsedValue);
    }
    public class NameValueWithParametersHeaderValue : NameValueHeaderValue {
      protected NameValueWithParametersHeaderValue(NameValueWithParametersHeaderValue source);
      public NameValueWithParametersHeaderValue(string name);
      public NameValueWithParametersHeaderValue(string name, string value);
      public ICollection<NameValueHeaderValue> Parameters { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static new NameValueWithParametersHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out NameValueWithParametersHeaderValue parsedValue);
    }
    public class ProductHeaderValue {
      public ProductHeaderValue(string name);
      public ProductHeaderValue(string name, string version);
      public string Name { get; }
      public string Version { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static ProductHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out ProductHeaderValue parsedValue);
    }
    public class ProductInfoHeaderValue {
      public ProductInfoHeaderValue(ProductHeaderValue product);
      public ProductInfoHeaderValue(string comment);
      public ProductInfoHeaderValue(string productName, string productVersion);
      public string Comment { get; }
      public ProductHeaderValue Product { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static ProductInfoHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out ProductInfoHeaderValue parsedValue);
    }
    public class RangeConditionHeaderValue {
      public RangeConditionHeaderValue(DateTimeOffset date);
      public RangeConditionHeaderValue(EntityTagHeaderValue entityTag);
      public RangeConditionHeaderValue(string entityTag);
      public Nullable<DateTimeOffset> Date { get; }
      public EntityTagHeaderValue EntityTag { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static RangeConditionHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out RangeConditionHeaderValue parsedValue);
    }
    public class RangeHeaderValue {
      public RangeHeaderValue();
      public RangeHeaderValue(Nullable<long> from, Nullable<long> to);
      public ICollection<RangeItemHeaderValue> Ranges { get; }
      public string Unit { get; set; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static RangeHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out RangeHeaderValue parsedValue);
    }
    public class RangeItemHeaderValue {
      public RangeItemHeaderValue(Nullable<long> from, Nullable<long> to);
      public Nullable<long> From { get; }
      public Nullable<long> To { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public override string ToString();
    }
    public class RetryConditionHeaderValue {
      public RetryConditionHeaderValue(DateTimeOffset date);
      public RetryConditionHeaderValue(TimeSpan delta);
      public Nullable<DateTimeOffset> Date { get; }
      public Nullable<TimeSpan> Delta { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static RetryConditionHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out RetryConditionHeaderValue parsedValue);
    }
    public class StringWithQualityHeaderValue {
      public StringWithQualityHeaderValue(string value);
      public StringWithQualityHeaderValue(string value, double quality);
      public Nullable<double> Quality { get; }
      public string Value { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static StringWithQualityHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out StringWithQualityHeaderValue parsedValue);
    }
    public class TransferCodingHeaderValue {
      protected TransferCodingHeaderValue(TransferCodingHeaderValue source);
      public TransferCodingHeaderValue(string value);
      public ICollection<NameValueHeaderValue> Parameters { get; }
      public string Value { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static TransferCodingHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out TransferCodingHeaderValue parsedValue);
    }
    public sealed class TransferCodingWithQualityHeaderValue : TransferCodingHeaderValue {
      public TransferCodingWithQualityHeaderValue(string value);
      public TransferCodingWithQualityHeaderValue(string value, double quality);
      public Nullable<double> Quality { get; set; }
      public static new TransferCodingWithQualityHeaderValue Parse(string input);
      public static bool TryParse(string input, out TransferCodingWithQualityHeaderValue parsedValue);
    }
    public class ViaHeaderValue {
      public ViaHeaderValue(string protocolVersion, string receivedBy);
      public ViaHeaderValue(string protocolVersion, string receivedBy, string protocolName);
      public ViaHeaderValue(string protocolVersion, string receivedBy, string protocolName, string comment);
      public string Comment { get; }
      public string ProtocolName { get; }
      public string ProtocolVersion { get; }
      public string ReceivedBy { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static ViaHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out ViaHeaderValue parsedValue);
    }
    public class WarningHeaderValue {
      public WarningHeaderValue(int code, string agent, string text);
      public WarningHeaderValue(int code, string agent, string text, DateTimeOffset date);
      public string Agent { get; }
      public int Code { get; }
      public Nullable<DateTimeOffset> Date { get; }
      public string Text { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static WarningHeaderValue Parse(string input);
      public override string ToString();
      public static bool TryParse(string input, out WarningHeaderValue parsedValue);
    }
  }
 }
 assembly System.Net.Http.WinHttpHandler {
  namespace System.Net.Http {
    public enum CookieUsePolicy {
      IgnoreCookies = 0,
      UseInternalCookieStoreOnly = 1,
      UseSpecifiedCookieContainer = 2,
    }
    public enum WindowsProxyUsePolicy {
      DoNotUseProxy = 0,
      UseCustomProxy = 3,
      UseWinHttpProxy = 1,
      UseWinInetProxy = 2,
    }
    public class WinHttpHandler : HttpMessageHandler {
      public WinHttpHandler();
      public DecompressionMethods AutomaticDecompression { get; set; }
      public bool AutomaticRedirection { get; set; }
      public bool CheckCertificateRevocationList { get; set; }
      public ClientCertificateOption ClientCertificateOption { get; set; }
      public X509Certificate2Collection ClientCertificates { get; }
      public TimeSpan ConnectTimeout { get; set; }
      public CookieContainer CookieContainer { get; set; }
      public CookieUsePolicy CookieUsePolicy { get; set; }
      public ICredentials DefaultProxyCredentials { get; set; }
      public int MaxAutomaticRedirections { get; set; }
      public int MaxConnectionsPerServer { get; set; }
      public int MaxResponseDrainSize { get; set; }
      public int MaxResponseHeadersLength { get; set; }
      public bool PreAuthenticate { get; set; }
      public IWebProxy Proxy { get; set; }
      public TimeSpan ReceiveDataTimeout { get; set; }
      public TimeSpan ReceiveHeadersTimeout { get; set; }
      public TimeSpan SendTimeout { get; set; }
      public Func<HttpRequestMessage, X509Certificate2, X509Chain, SslPolicyErrors, bool> ServerCertificateValidationCallback { get; set; }
      public ICredentials ServerCredentials { get; set; }
      public SslProtocols SslProtocols { get; set; }
      public WindowsProxyUsePolicy WindowsProxyUsePolicy { get; set; }
      protected override void Dispose(bool disposing);
      protected override Task<HttpResponseMessage> SendAsync(HttpRequestMessage request, CancellationToken cancellationToken);
    }
  }
 }
 assembly System.Net.NameResolution {
  namespace System.Net {
    public static class Dns {
      public static Task<IPAddress[]> GetHostAddressesAsync(string hostNameOrAddress);
      public static Task<IPHostEntry> GetHostEntryAsync(IPAddress address);
      public static Task<IPHostEntry> GetHostEntryAsync(string hostNameOrAddress);
      public static string GetHostName();
    }
    public class IPHostEntry {
      public IPHostEntry();
      public IPAddress[] AddressList { get; set; }
      public string[] Aliases { get; set; }
      public string HostName { get; set; }
    }
  }
 }
 assembly System.Net.NetworkInformation {
  namespace System.Net.NetworkInformation {
    public enum DuplicateAddressDetectionState {
      Deprecated = 3,
      Duplicate = 2,
      Invalid = 0,
      Preferred = 4,
      Tentative = 1,
    }
    public abstract class GatewayIPAddressInformation {
      protected GatewayIPAddressInformation();
      public abstract IPAddress Address { get; }
    }
    public class GatewayIPAddressInformationCollection : ICollection<GatewayIPAddressInformation>, IEnumerable, IEnumerable<GatewayIPAddressInformation> {
      protected internal GatewayIPAddressInformationCollection();
      public virtual int Count { get; }
      public virtual bool IsReadOnly { get; }
      public virtual GatewayIPAddressInformation this[int index] { get; }
      public virtual void Add(GatewayIPAddressInformation address);
      public virtual void Clear();
      public virtual bool Contains(GatewayIPAddressInformation address);
      public virtual void CopyTo(GatewayIPAddressInformation[] array, int offset);
      public virtual IEnumerator<GatewayIPAddressInformation> GetEnumerator();
      public virtual bool Remove(GatewayIPAddressInformation address);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
    public abstract class IcmpV4Statistics {
      protected IcmpV4Statistics();
      public abstract long AddressMaskRepliesReceived { get; }
      public abstract long AddressMaskRepliesSent { get; }
      public abstract long AddressMaskRequestsReceived { get; }
      public abstract long AddressMaskRequestsSent { get; }
      public abstract long DestinationUnreachableMessagesReceived { get; }
      public abstract long DestinationUnreachableMessagesSent { get; }
      public abstract long EchoRepliesReceived { get; }
      public abstract long EchoRepliesSent { get; }
      public abstract long EchoRequestsReceived { get; }
      public abstract long EchoRequestsSent { get; }
      public abstract long ErrorsReceived { get; }
      public abstract long ErrorsSent { get; }
      public abstract long MessagesReceived { get; }
      public abstract long MessagesSent { get; }
      public abstract long ParameterProblemsReceived { get; }
      public abstract long ParameterProblemsSent { get; }
      public abstract long RedirectsReceived { get; }
      public abstract long RedirectsSent { get; }
      public abstract long SourceQuenchesReceived { get; }
      public abstract long SourceQuenchesSent { get; }
      public abstract long TimeExceededMessagesReceived { get; }
      public abstract long TimeExceededMessagesSent { get; }
      public abstract long TimestampRepliesReceived { get; }
      public abstract long TimestampRepliesSent { get; }
      public abstract long TimestampRequestsReceived { get; }
      public abstract long TimestampRequestsSent { get; }
    }
    public abstract class IcmpV6Statistics {
      protected IcmpV6Statistics();
      public abstract long DestinationUnreachableMessagesReceived { get; }
      public abstract long DestinationUnreachableMessagesSent { get; }
      public abstract long EchoRepliesReceived { get; }
      public abstract long EchoRepliesSent { get; }
      public abstract long EchoRequestsReceived { get; }
      public abstract long EchoRequestsSent { get; }
      public abstract long ErrorsReceived { get; }
      public abstract long ErrorsSent { get; }
      public abstract long MembershipQueriesReceived { get; }
      public abstract long MembershipQueriesSent { get; }
      public abstract long MembershipReductionsReceived { get; }
      public abstract long MembershipReductionsSent { get; }
      public abstract long MembershipReportsReceived { get; }
      public abstract long MembershipReportsSent { get; }
      public abstract long MessagesReceived { get; }
      public abstract long MessagesSent { get; }
      public abstract long NeighborAdvertisementsReceived { get; }
      public abstract long NeighborAdvertisementsSent { get; }
      public abstract long NeighborSolicitsReceived { get; }
      public abstract long NeighborSolicitsSent { get; }
      public abstract long PacketTooBigMessagesReceived { get; }
      public abstract long PacketTooBigMessagesSent { get; }
      public abstract long ParameterProblemsReceived { get; }
      public abstract long ParameterProblemsSent { get; }
      public abstract long RedirectsReceived { get; }
      public abstract long RedirectsSent { get; }
      public abstract long RouterAdvertisementsReceived { get; }
      public abstract long RouterAdvertisementsSent { get; }
      public abstract long RouterSolicitsReceived { get; }
      public abstract long RouterSolicitsSent { get; }
      public abstract long TimeExceededMessagesReceived { get; }
      public abstract long TimeExceededMessagesSent { get; }
    }
    public abstract class IPAddressInformation {
      protected IPAddressInformation();
      public abstract IPAddress Address { get; }
      public abstract bool IsDnsEligible { get; }
      public abstract bool IsTransient { get; }
    }
    public class IPAddressInformationCollection : ICollection<IPAddressInformation>, IEnumerable, IEnumerable<IPAddressInformation> {
      public virtual int Count { get; }
      public virtual bool IsReadOnly { get; }
      public virtual IPAddressInformation this[int index] { get; }
      public virtual void Add(IPAddressInformation address);
      public virtual void Clear();
      public virtual bool Contains(IPAddressInformation address);
      public virtual void CopyTo(IPAddressInformation[] array, int offset);
      public virtual IEnumerator<IPAddressInformation> GetEnumerator();
      public virtual bool Remove(IPAddressInformation address);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
    public abstract class IPGlobalProperties {
      protected IPGlobalProperties();
      public abstract string DhcpScopeName { get; }
      public abstract string DomainName { get; }
      public abstract string HostName { get; }
      public abstract bool IsWinsProxy { get; }
      public abstract NetBiosNodeType NodeType { get; }
      public abstract TcpConnectionInformation[] GetActiveTcpConnections();
      public abstract IPEndPoint[] GetActiveTcpListeners();
      public abstract IPEndPoint[] GetActiveUdpListeners();
      public abstract IcmpV4Statistics GetIcmpV4Statistics();
      public abstract IcmpV6Statistics GetIcmpV6Statistics();
      public static IPGlobalProperties GetIPGlobalProperties();
      public abstract IPGlobalStatistics GetIPv4GlobalStatistics();
      public abstract IPGlobalStatistics GetIPv6GlobalStatistics();
      public abstract TcpStatistics GetTcpIPv4Statistics();
      public abstract TcpStatistics GetTcpIPv6Statistics();
      public abstract UdpStatistics GetUdpIPv4Statistics();
      public abstract UdpStatistics GetUdpIPv6Statistics();
      public virtual Task<UnicastIPAddressInformationCollection> GetUnicastAddressesAsync();
    }
    public abstract class IPGlobalStatistics {
      protected IPGlobalStatistics();
      public abstract int DefaultTtl { get; }
      public abstract bool ForwardingEnabled { get; }
      public abstract int NumberOfInterfaces { get; }
      public abstract int NumberOfIPAddresses { get; }
      public abstract int NumberOfRoutes { get; }
      public abstract long OutputPacketRequests { get; }
      public abstract long OutputPacketRoutingDiscards { get; }
      public abstract long OutputPacketsDiscarded { get; }
      public abstract long OutputPacketsWithNoRoute { get; }
      public abstract long PacketFragmentFailures { get; }
      public abstract long PacketReassembliesRequired { get; }
      public abstract long PacketReassemblyFailures { get; }
      public abstract long PacketReassemblyTimeout { get; }
      public abstract long PacketsFragmented { get; }
      public abstract long PacketsReassembled { get; }
      public abstract long ReceivedPackets { get; }
      public abstract long ReceivedPacketsDelivered { get; }
      public abstract long ReceivedPacketsDiscarded { get; }
      public abstract long ReceivedPacketsForwarded { get; }
      public abstract long ReceivedPacketsWithAddressErrors { get; }
      public abstract long ReceivedPacketsWithHeadersErrors { get; }
      public abstract long ReceivedPacketsWithUnknownProtocol { get; }
    }
    public abstract class IPInterfaceProperties {
      protected IPInterfaceProperties();
      public abstract IPAddressInformationCollection AnycastAddresses { get; }
      public abstract IPAddressCollection DhcpServerAddresses { get; }
      public abstract IPAddressCollection DnsAddresses { get; }
      public abstract string DnsSuffix { get; }
      public abstract GatewayIPAddressInformationCollection GatewayAddresses { get; }
      public abstract bool IsDnsEnabled { get; }
      public abstract bool IsDynamicDnsEnabled { get; }
      public abstract MulticastIPAddressInformationCollection MulticastAddresses { get; }
      public abstract UnicastIPAddressInformationCollection UnicastAddresses { get; }
      public abstract IPAddressCollection WinsServersAddresses { get; }
      public abstract IPv4InterfaceProperties GetIPv4Properties();
      public abstract IPv6InterfaceProperties GetIPv6Properties();
    }
    public abstract class IPInterfaceStatistics {
      protected IPInterfaceStatistics();
      public abstract long BytesReceived { get; }
      public abstract long BytesSent { get; }
      public abstract long IncomingPacketsDiscarded { get; }
      public abstract long IncomingPacketsWithErrors { get; }
      public abstract long IncomingUnknownProtocolPackets { get; }
      public abstract long NonUnicastPacketsReceived { get; }
      public abstract long NonUnicastPacketsSent { get; }
      public abstract long OutgoingPacketsDiscarded { get; }
      public abstract long OutgoingPacketsWithErrors { get; }
      public abstract long OutputQueueLength { get; }
      public abstract long UnicastPacketsReceived { get; }
      public abstract long UnicastPacketsSent { get; }
    }
    public abstract class IPv4InterfaceProperties {
      protected IPv4InterfaceProperties();
      public abstract int Index { get; }
      public abstract bool IsAutomaticPrivateAddressingActive { get; }
      public abstract bool IsAutomaticPrivateAddressingEnabled { get; }
      public abstract bool IsDhcpEnabled { get; }
      public abstract bool IsForwardingEnabled { get; }
      public abstract int Mtu { get; }
      public abstract bool UsesWins { get; }
    }
    public abstract class IPv6InterfaceProperties {
      protected IPv6InterfaceProperties();
      public abstract int Index { get; }
      public abstract int Mtu { get; }
      public virtual long GetScopeId(ScopeLevel scopeLevel);
    }
    public abstract class MulticastIPAddressInformation : IPAddressInformation {
      protected MulticastIPAddressInformation();
      public abstract long AddressPreferredLifetime { get; }
      public abstract long AddressValidLifetime { get; }
      public abstract long DhcpLeaseLifetime { get; }
      public abstract DuplicateAddressDetectionState DuplicateAddressDetectionState { get; }
      public abstract PrefixOrigin PrefixOrigin { get; }
      public abstract SuffixOrigin SuffixOrigin { get; }
    }
    public class MulticastIPAddressInformationCollection : ICollection<MulticastIPAddressInformation>, IEnumerable, IEnumerable<MulticastIPAddressInformation> {
      protected internal MulticastIPAddressInformationCollection();
      public virtual int Count { get; }
      public virtual bool IsReadOnly { get; }
      public virtual MulticastIPAddressInformation this[int index] { get; }
      public virtual void Add(MulticastIPAddressInformation address);
      public virtual void Clear();
      public virtual bool Contains(MulticastIPAddressInformation address);
      public virtual void CopyTo(MulticastIPAddressInformation[] array, int offset);
      public virtual IEnumerator<MulticastIPAddressInformation> GetEnumerator();
      public virtual bool Remove(MulticastIPAddressInformation address);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
    public enum NetBiosNodeType {
      Broadcast = 1,
      Hybrid = 8,
      Mixed = 4,
      Peer2Peer = 2,
      Unknown = 0,
    }
    public delegate void NetworkAddressChangedEventHandler(object sender, EventArgs e);
    public static class NetworkChange {
      public static event NetworkAddressChangedEventHandler NetworkAddressChanged;
    }
    public class NetworkInformationException : Exception {
      public NetworkInformationException();
      public NetworkInformationException(int errorCode);
      public int ErrorCode { get; }
    }
    public abstract class NetworkInterface {
      protected NetworkInterface();
      public virtual string Description { get; }
      public virtual string Id { get; }
      public static int IPv6LoopbackInterfaceIndex { get; }
      public virtual bool IsReceiveOnly { get; }
      public static int LoopbackInterfaceIndex { get; }
      public virtual string Name { get; }
      public virtual NetworkInterfaceType NetworkInterfaceType { get; }
      public virtual OperationalStatus OperationalStatus { get; }
      public virtual long Speed { get; }
      public virtual bool SupportsMulticast { get; }
      public static NetworkInterface[] GetAllNetworkInterfaces();
      public virtual IPInterfaceProperties GetIPProperties();
      public virtual IPInterfaceStatistics GetIPStatistics();
      public static bool GetIsNetworkAvailable();
      public virtual PhysicalAddress GetPhysicalAddress();
      public virtual bool Supports(NetworkInterfaceComponent networkInterfaceComponent);
    }
    public enum NetworkInterfaceComponent {
      IPv4 = 0,
      IPv6 = 1,
    }
    public enum NetworkInterfaceType {
      AsymmetricDsl = 94,
      Atm = 37,
      BasicIsdn = 20,
      Ethernet = 6,
      Ethernet3Megabit = 26,
      FastEthernetFx = 69,
      FastEthernetT = 62,
      Fddi = 15,
      GenericModem = 48,
      GigabitEthernet = 117,
      HighPerformanceSerialBus = 144,
      IPOverAtm = 114,
      Isdn = 63,
      Loopback = 24,
      MultiRateSymmetricDsl = 143,
      Ppp = 23,
      PrimaryIsdn = 21,
      RateAdaptDsl = 95,
      Slip = 28,
      SymmetricDsl = 96,
      TokenRing = 9,
      Tunnel = 131,
      Unknown = 1,
      VeryHighSpeedDsl = 97,
      Wireless80211 = 71,
      Wman = 237,
      Wwanpp = 243,
      Wwanpp2 = 244,
    }
    public enum OperationalStatus {
      Dormant = 5,
      Down = 2,
      LowerLayerDown = 7,
      NotPresent = 6,
      Testing = 3,
      Unknown = 4,
      Up = 1,
    }
    public class PhysicalAddress {
      public static readonly PhysicalAddress None;
      public PhysicalAddress(byte[] address);
      public override bool Equals(object comparand);
      public byte[] GetAddressBytes();
      public override int GetHashCode();
      public static PhysicalAddress Parse(string address);
      public override string ToString();
    }
    public enum PrefixOrigin {
      Dhcp = 3,
      Manual = 1,
      Other = 0,
      RouterAdvertisement = 4,
      WellKnown = 2,
    }
    public enum ScopeLevel {
      Admin = 4,
      Global = 14,
      Interface = 1,
      Link = 2,
      None = 0,
      Organization = 8,
      Site = 5,
      Subnet = 3,
    }
    public enum SuffixOrigin {
      LinkLayerAddress = 4,
      Manual = 1,
      OriginDhcp = 3,
      Other = 0,
      Random = 5,
      WellKnown = 2,
    }
    public abstract class TcpConnectionInformation {
      protected TcpConnectionInformation();
      public abstract IPEndPoint LocalEndPoint { get; }
      public abstract IPEndPoint RemoteEndPoint { get; }
      public abstract TcpState State { get; }
    }
    public enum TcpState {
      Closed = 1,
      CloseWait = 8,
      Closing = 9,
      DeleteTcb = 12,
      Established = 5,
      FinWait1 = 6,
      FinWait2 = 7,
      LastAck = 10,
      Listen = 2,
      SynReceived = 4,
      SynSent = 3,
      TimeWait = 11,
      Unknown = 0,
    }
    public abstract class TcpStatistics {
      protected TcpStatistics();
      public abstract long ConnectionsAccepted { get; }
      public abstract long ConnectionsInitiated { get; }
      public abstract long CumulativeConnections { get; }
      public abstract long CurrentConnections { get; }
      public abstract long ErrorsReceived { get; }
      public abstract long FailedConnectionAttempts { get; }
      public abstract long MaximumConnections { get; }
      public abstract long MaximumTransmissionTimeout { get; }
      public abstract long MinimumTransmissionTimeout { get; }
      public abstract long ResetConnections { get; }
      public abstract long ResetsSent { get; }
      public abstract long SegmentsReceived { get; }
      public abstract long SegmentsResent { get; }
      public abstract long SegmentsSent { get; }
    }
    public abstract class UdpStatistics {
      protected UdpStatistics();
      public abstract long DatagramsReceived { get; }
      public abstract long DatagramsSent { get; }
      public abstract long IncomingDatagramsDiscarded { get; }
      public abstract long IncomingDatagramsWithErrors { get; }
      public abstract int UdpListeners { get; }
    }
    public abstract class UnicastIPAddressInformation : IPAddressInformation {
      protected UnicastIPAddressInformation();
      public abstract long AddressPreferredLifetime { get; }
      public abstract long AddressValidLifetime { get; }
      public abstract long DhcpLeaseLifetime { get; }
      public abstract DuplicateAddressDetectionState DuplicateAddressDetectionState { get; }
      public abstract IPAddress IPv4Mask { get; }
      public virtual int PrefixLength { get; }
      public abstract PrefixOrigin PrefixOrigin { get; }
      public abstract SuffixOrigin SuffixOrigin { get; }
    }
    public class UnicastIPAddressInformationCollection : ICollection<UnicastIPAddressInformation>, IEnumerable, IEnumerable<UnicastIPAddressInformation> {
      protected internal UnicastIPAddressInformationCollection();
      public virtual int Count { get; }
      public virtual bool IsReadOnly { get; }
      public virtual UnicastIPAddressInformation this[int index] { get; }
      public virtual void Add(UnicastIPAddressInformation address);
      public virtual void Clear();
      public virtual bool Contains(UnicastIPAddressInformation address);
      public virtual void CopyTo(UnicastIPAddressInformation[] array, int offset);
      public virtual IEnumerator<UnicastIPAddressInformation> GetEnumerator();
      public virtual bool Remove(UnicastIPAddressInformation address);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
  }
 }
 assembly System.Net.Primitives {
  namespace System.Net {
    public enum AuthenticationSchemes {
      Anonymous = 32768,
      Basic = 8,
      Digest = 1,
      IntegratedWindowsAuthentication = 6,
      Negotiate = 2,
      None = 0,
      Ntlm = 4,
    }
    public sealed class Cookie {
      public Cookie();
      public Cookie(string name, string value);
      public Cookie(string name, string value, string path);
      public Cookie(string name, string value, string path, string domain);
      public string Comment { get; set; }
      public Uri CommentUri { get; set; }
      public bool Discard { get; set; }
      public string Domain { get; set; }
      public bool Expired { get; set; }
      public DateTime Expires { get; set; }
      public bool HttpOnly { get; set; }
      public string Name { get; set; }
      public string Path { get; set; }
      public string Port { get; set; }
      public bool Secure { get; set; }
      public DateTime TimeStamp { get; }
      public string Value { get; set; }
      public int Version { get; set; }
      public override bool Equals(object comparand);
      public override int GetHashCode();
      public override string ToString();
    }
    public class CookieCollection : ICollection, IEnumerable {
      public CookieCollection();
      public int Count { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public Cookie this[string name] { get; }
      public void Add(Cookie cookie);
      public void Add(CookieCollection cookies);
      public IEnumerator GetEnumerator();
      void System.Collections.ICollection.CopyTo(Array array, int index);
    }
    public class CookieContainer {
      public const int DefaultCookieLengthLimit = 4096;
      public const int DefaultCookieLimit = 300;
      public const int DefaultPerDomainCookieLimit = 20;
      public CookieContainer();
      public int Capacity { get; set; }
      public int Count { get; }
      public int MaxCookieSize { get; set; }
      public int PerDomainCapacity { get; set; }
      public void Add(Uri uri, Cookie cookie);
      public void Add(Uri uri, CookieCollection cookies);
      public string GetCookieHeader(Uri uri);
      public CookieCollection GetCookies(Uri uri);
      public void SetCookies(Uri uri, string cookieHeader);
    }
    public class CookieException : FormatException {
      public CookieException();
    }
    public class CredentialCache : ICredentials, ICredentialsByHost, IEnumerable {
      public CredentialCache();
      public static ICredentials DefaultCredentials { get; }
      public static NetworkCredential DefaultNetworkCredentials { get; }
      public void Add(string host, int port, string authenticationType, NetworkCredential credential);
      public void Add(Uri uriPrefix, string authType, NetworkCredential cred);
      public NetworkCredential GetCredential(string host, int port, string authenticationType);
      public NetworkCredential GetCredential(Uri uriPrefix, string authType);
      public IEnumerator GetEnumerator();
      public void Remove(string host, int port, string authenticationType);
      public void Remove(Uri uriPrefix, string authType);
    }
    public enum DecompressionMethods {
      Deflate = 2,
      GZip = 1,
      None = 0,
    }
    public class DnsEndPoint : EndPoint {
      public DnsEndPoint(string host, int port);
      public DnsEndPoint(string host, int port, AddressFamily addressFamily);
      public override AddressFamily AddressFamily { get; }
      public string Host { get; }
      public int Port { get; }
      public override bool Equals(object comparand);
      public override int GetHashCode();
      public override string ToString();
    }
    public abstract class EndPoint {
      protected EndPoint();
      public virtual AddressFamily AddressFamily { get; }
      public virtual EndPoint Create(SocketAddress socketAddress);
      public virtual SocketAddress Serialize();
    }
    public enum HttpStatusCode {
      Accepted = 202,
      Ambiguous = 300,
      BadGateway = 502,
      BadRequest = 400,
      Conflict = 409,
      Continue = 100,
      Created = 201,
      ExpectationFailed = 417,
      Forbidden = 403,
      Found = 302,
      GatewayTimeout = 504,
      Gone = 410,
      HttpVersionNotSupported = 505,
      InternalServerError = 500,
      LengthRequired = 411,
      MethodNotAllowed = 405,
      Moved = 301,
      MovedPermanently = 301,
      MultipleChoices = 300,
      NoContent = 204,
      NonAuthoritativeInformation = 203,
      NotAcceptable = 406,
      NotFound = 404,
      NotImplemented = 501,
      NotModified = 304,
      OK = 200,
      PartialContent = 206,
      PaymentRequired = 402,
      PreconditionFailed = 412,
      ProxyAuthenticationRequired = 407,
      Redirect = 302,
      RedirectKeepVerb = 307,
      RedirectMethod = 303,
      RequestedRangeNotSatisfiable = 416,
      RequestEntityTooLarge = 413,
      RequestTimeout = 408,
      RequestUriTooLong = 414,
      ResetContent = 205,
      SeeOther = 303,
      ServiceUnavailable = 503,
      SwitchingProtocols = 101,
      TemporaryRedirect = 307,
      Unauthorized = 401,
      UnsupportedMediaType = 415,
      Unused = 306,
      UpgradeRequired = 426,
      UseProxy = 305,
    }
    public interface ICredentials {
      NetworkCredential GetCredential(Uri uri, string authType);
    }
    public interface ICredentialsByHost {
      NetworkCredential GetCredential(string host, int port, string authenticationType);
    }
    public class IPAddress {
      public static readonly IPAddress Any;
      public static readonly IPAddress Broadcast;
      public static readonly IPAddress IPv6Any;
      public static readonly IPAddress IPv6Loopback;
      public static readonly IPAddress IPv6None;
      public static readonly IPAddress Loopback;
      public static readonly IPAddress None;
      public IPAddress(byte[] address);
      public IPAddress(byte[] address, long scopeid);
      public IPAddress(long newAddress);
      public AddressFamily AddressFamily { get; }
      public bool IsIPv4MappedToIPv6 { get; }
      public bool IsIPv6LinkLocal { get; }
      public bool IsIPv6Multicast { get; }
      public bool IsIPv6SiteLocal { get; }
      public bool IsIPv6Teredo { get; }
      public long ScopeId { get; set; }
      public override bool Equals(object comparand);
      public byte[] GetAddressBytes();
      public override int GetHashCode();
      public static short HostToNetworkOrder(short host);
      public static int HostToNetworkOrder(int host);
      public static long HostToNetworkOrder(long host);
      public static bool IsLoopback(IPAddress address);
      public IPAddress MapToIPv4();
      public IPAddress MapToIPv6();
      public static short NetworkToHostOrder(short network);
      public static int NetworkToHostOrder(int network);
      public static long NetworkToHostOrder(long network);
      public static IPAddress Parse(string ipString);
      public override string ToString();
      public static bool TryParse(string ipString, out IPAddress address);
    }
    public class IPEndPoint : EndPoint {
      public const int MaxPort = 65535;
      public const int MinPort = 0;
      public IPEndPoint(long address, int port);
      public IPEndPoint(IPAddress address, int port);
      public IPAddress Address { get; set; }
      public override AddressFamily AddressFamily { get; }
      public int Port { get; set; }
      public override EndPoint Create(SocketAddress socketAddress);
      public override bool Equals(object comparand);
      public override int GetHashCode();
      public override SocketAddress Serialize();
      public override string ToString();
    }
    public interface IWebProxy {
      ICredentials Credentials { get; set; }
      Uri GetProxy(Uri destination);
      bool IsBypassed(Uri host);
    }
    public class NetworkCredential : ICredentials, ICredentialsByHost {
      public NetworkCredential();
      public NetworkCredential(string userName, string password);
      public NetworkCredential(string userName, string password, string domain);
      public string Domain { get; set; }
      public string Password { get; set; }
      public string UserName { get; set; }
      public NetworkCredential GetCredential(string host, int port, string authenticationType);
      public NetworkCredential GetCredential(Uri uri, string authType);
    }
    public class SocketAddress {
      public SocketAddress(AddressFamily family);
      public SocketAddress(AddressFamily family, int size);
      public AddressFamily Family { get; }
      public int Size { get; }
      public byte this[int offset] { get; set; }
      public override bool Equals(object comparand);
      public override int GetHashCode();
      public override string ToString();
    }
    public abstract class TransportContext {
      protected TransportContext();
      public abstract ChannelBinding GetChannelBinding(ChannelBindingKind kind);
    }
  }
  namespace System.Net.NetworkInformation {
    public class IPAddressCollection : ICollection<IPAddress>, IEnumerable, IEnumerable<IPAddress> {
      protected internal IPAddressCollection();
      public virtual int Count { get; }
      public virtual bool IsReadOnly { get; }
      public virtual IPAddress this[int index] { get; }
      public virtual void Add(IPAddress address);
      public virtual void Clear();
      public virtual bool Contains(IPAddress address);
      public virtual void CopyTo(IPAddress[] array, int offset);
      public virtual IEnumerator<IPAddress> GetEnumerator();
      public virtual bool Remove(IPAddress address);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
  }
  namespace System.Net.Security {
    public enum AuthenticationLevel {
      MutualAuthRequested = 1,
      MutualAuthRequired = 2,
      None = 0,
    }
    public enum SslPolicyErrors {
      None = 0,
      RemoteCertificateChainErrors = 4,
      RemoteCertificateNameMismatch = 2,
      RemoteCertificateNotAvailable = 1,
    }
  }
  namespace System.Net.Sockets {
    public enum AddressFamily {
      AppleTalk = 16,
      Atm = 22,
      Banyan = 21,
      Ccitt = 10,
      Chaos = 5,
      Cluster = 24,
      DataKit = 9,
      DataLink = 13,
      DecNet = 12,
      Ecma = 8,
      FireFox = 19,
      HyperChannel = 15,
      Ieee12844 = 25,
      ImpLink = 3,
      InterNetwork = 2,
      InterNetworkV6 = 23,
      Ipx = 6,
      Irda = 26,
      Iso = 7,
      Lat = 14,
      NetBios = 17,
      NetworkDesigners = 28,
      NS = 6,
      Osi = 7,
      Pup = 4,
      Sna = 11,
      Unix = 1,
      Unknown = -1,
      Unspecified = 0,
      VoiceView = 18,
    }
    public enum SocketError {
      AccessDenied = 10013,
      AddressAlreadyInUse = 10048,
      AddressFamilyNotSupported = 10047,
      AddressNotAvailable = 10049,
      AlreadyInProgress = 10037,
      ConnectionAborted = 10053,
      ConnectionRefused = 10061,
      ConnectionReset = 10054,
      DestinationAddressRequired = 10039,
      Disconnecting = 10101,
      Fault = 10014,
      HostDown = 10064,
      HostNotFound = 11001,
      HostUnreachable = 10065,
      InProgress = 10036,
      Interrupted = 10004,
      InvalidArgument = 10022,
      IOPending = 997,
      IsConnected = 10056,
      MessageSize = 10040,
      NetworkDown = 10050,
      NetworkReset = 10052,
      NetworkUnreachable = 10051,
      NoBufferSpaceAvailable = 10055,
      NoData = 11004,
      NoRecovery = 11003,
      NotConnected = 10057,
      NotInitialized = 10093,
      NotSocket = 10038,
      OperationAborted = 995,
      OperationNotSupported = 10045,
      ProcessLimit = 10067,
      ProtocolFamilyNotSupported = 10046,
      ProtocolNotSupported = 10043,
      ProtocolOption = 10042,
      ProtocolType = 10041,
      Shutdown = 10058,
      SocketError = -1,
      SocketNotSupported = 10044,
      Success = 0,
      SystemNotReady = 10091,
      TimedOut = 10060,
      TooManyOpenSockets = 10024,
      TryAgain = 11002,
      TypeNotFound = 10109,
      VersionNotSupported = 10092,
      WouldBlock = 10035,
    }
    public class SocketException : Exception {
      public SocketException();
      public SocketException(int errorCode);
      public override string Message { get; }
      public SocketError SocketErrorCode { get; }
    }
  }
  namespace System.Security.Authentication {
    public enum CipherAlgorithmType {
      Aes = 26129,
      Aes128 = 26126,
      Aes192 = 26127,
      Aes256 = 26128,
      Des = 26113,
      None = 0,
      Null = 24576,
      Rc2 = 26114,
      Rc4 = 26625,
      TripleDes = 26115,
    }
    public enum ExchangeAlgorithmType {
      DiffieHellman = 43522,
      None = 0,
      RsaKeyX = 41984,
      RsaSign = 9216,
    }
    public enum HashAlgorithmType {
      Md5 = 32771,
      None = 0,
      Sha1 = 32772,
    }
    public enum SslProtocols {
      None = 0,
      Ssl2 = 12,
      Ssl3 = 48,
      Tls = 192,
      Tls11 = 768,
      Tls12 = 3072,
    }
  }
  namespace System.Security.Authentication.ExtendedProtection {
    public abstract class ChannelBinding : SafeHandle {
      protected ChannelBinding();
      protected ChannelBinding(bool ownsHandle);
      public abstract int Size { get; }
    }
    public enum ChannelBindingKind {
      Endpoint = 26,
      Unique = 25,
      Unknown = 0,
    }
  }
 }
 assembly System.Net.Requests {
  namespace System.Net {
    public enum HttpRequestHeader {
      Accept = 20,
      AcceptCharset = 21,
      AcceptEncoding = 22,
      AcceptLanguage = 23,
      Allow = 10,
      Authorization = 24,
      CacheControl = 0,
      Connection = 1,
      ContentEncoding = 13,
      ContentLanguage = 14,
      ContentLength = 11,
      ContentLocation = 15,
      ContentMd5 = 16,
      ContentRange = 17,
      ContentType = 12,
      Cookie = 25,
      Date = 2,
      Expect = 26,
      Expires = 18,
      From = 27,
      Host = 28,
      IfMatch = 29,
      IfModifiedSince = 30,
      IfNoneMatch = 31,
      IfRange = 32,
      IfUnmodifiedSince = 33,
      KeepAlive = 3,
      LastModified = 19,
      MaxForwards = 34,
      Pragma = 4,
      ProxyAuthorization = 35,
      Range = 37,
      Referer = 36,
      Te = 38,
      Trailer = 5,
      TransferEncoding = 6,
      Translate = 39,
      Upgrade = 7,
      UserAgent = 40,
      Via = 8,
      Warning = 9,
    }
    public class HttpWebRequest : WebRequest {
      public string Accept { get; set; }
      public virtual bool AllowReadStreamBuffering { get; set; }
      public override string ContentType { get; set; }
      public int ContinueTimeout { get; set; }
      public virtual CookieContainer CookieContainer { get; set; }
      public override ICredentials Credentials { get; set; }
      public virtual bool HaveResponse { get; }
      public override WebHeaderCollection Headers { get; set; }
      public override string Method { get; set; }
      public override Uri RequestUri { get; }
      public virtual bool SupportsCookieContainer { get; }
      public override bool UseDefaultCredentials { get; set; }
      public override void Abort();
      public override IAsyncResult BeginGetRequestStream(AsyncCallback callback, object state);
      public override IAsyncResult BeginGetResponse(AsyncCallback callback, object state);
      public override Stream EndGetRequestStream(IAsyncResult asyncResult);
      public override WebResponse EndGetResponse(IAsyncResult asyncResult);
    }
    public class HttpWebResponse : WebResponse {
      public override long ContentLength { get; }
      public override string ContentType { get; }
      public virtual CookieCollection Cookies { get; }
      public override WebHeaderCollection Headers { get; }
      public virtual string Method { get; }
      public override Uri ResponseUri { get; }
      public virtual HttpStatusCode StatusCode { get; }
      public virtual string StatusDescription { get; }
      public override bool SupportsHeaders { get; }
      protected override void Dispose(bool disposing);
      public override Stream GetResponseStream();
    }
    public interface IWebRequestCreate {
      WebRequest Create(Uri uri);
    }
    public class ProtocolViolationException : InvalidOperationException {
      public ProtocolViolationException();
      public ProtocolViolationException(string message);
    }
    public class WebException : InvalidOperationException {
      public WebException();
      public WebException(string message);
      public WebException(string message, Exception inner);
      public WebException(string message, Exception inner, WebExceptionStatus status, WebResponse response);
      public WebException(string message, WebExceptionStatus status);
      public WebResponse Response { get; }
      public WebExceptionStatus Status { get; }
    }
    public enum WebExceptionStatus {
      CacheEntryNotFound = 18,
      ConnectFailure = 2,
      ConnectionClosed = 8,
      KeepAliveFailure = 12,
      MessageLengthLimitExceeded = 17,
      NameResolutionFailure = 1,
      Pending = 13,
      PipelineFailure = 5,
      ProtocolError = 7,
      ProxyNameResolutionFailure = 15,
      ReceiveFailure = 3,
      RequestCanceled = 6,
      RequestProhibitedByCachePolicy = 19,
      RequestProhibitedByProxy = 20,
      SecureChannelFailure = 10,
      SendFailure = 4,
      ServerProtocolViolation = 11,
      Success = 0,
      Timeout = 14,
      TrustFailure = 9,
      UnknownError = 16,
    }
    public sealed class WebHeaderCollection : IEnumerable {
      public WebHeaderCollection();
      public string[] AllKeys { get; }
      public int Count { get; }
      public string this[HttpRequestHeader header] { get; set; }
      public string this[HttpResponseHeader header] { get; set; }
      public string this[string name] { get; set; }
      public void Remove(string name);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public override string ToString();
    }
    public abstract class WebRequest {
      protected WebRequest();
      public abstract string ContentType { get; set; }
      public virtual ICredentials Credentials { get; set; }
      public static IWebProxy DefaultWebProxy { get; set; }
      public abstract WebHeaderCollection Headers { get; set; }
      public abstract string Method { get; set; }
      public virtual IWebProxy Proxy { get; set; }
      public abstract Uri RequestUri { get; }
      public virtual bool UseDefaultCredentials { get; set; }
      public abstract void Abort();
      public abstract IAsyncResult BeginGetRequestStream(AsyncCallback callback, object state);
      public abstract IAsyncResult BeginGetResponse(AsyncCallback callback, object state);
      public static WebRequest Create(string requestUriString);
      public static WebRequest Create(Uri requestUri);
      public static HttpWebRequest CreateHttp(string requestUriString);
      public static HttpWebRequest CreateHttp(Uri requestUri);
      public abstract Stream EndGetRequestStream(IAsyncResult asyncResult);
      public abstract WebResponse EndGetResponse(IAsyncResult asyncResult);
      public virtual Task<Stream> GetRequestStreamAsync();
      public virtual Task<WebResponse> GetResponseAsync();
      public static bool RegisterPrefix(string prefix, IWebRequestCreate creator);
    }
    public abstract class WebResponse : IDisposable {
      protected WebResponse();
      public abstract long ContentLength { get; }
      public abstract string ContentType { get; }
      public virtual WebHeaderCollection Headers { get; }
      public abstract Uri ResponseUri { get; }
      public virtual bool SupportsHeaders { get; }
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public abstract Stream GetResponseStream();
    }
  }
 }
 assembly System.Net.Security {
  namespace System.Net.Security {
    public abstract class AuthenticatedStream : Stream {
      protected AuthenticatedStream(Stream innerStream, bool leaveInnerStreamOpen);
      protected Stream InnerStream { get; }
      public abstract bool IsAuthenticated { get; }
      public abstract bool IsEncrypted { get; }
      public abstract bool IsMutuallyAuthenticated { get; }
      public abstract bool IsServer { get; }
      public abstract bool IsSigned { get; }
      public bool LeaveInnerStreamOpen { get; }
      protected override void Dispose(bool disposing);
    }
    public enum EncryptionPolicy {
      AllowNoEncryption = 1,
      NoEncryption = 2,
      RequireEncryption = 0,
    }
    public delegate X509Certificate LocalCertificateSelectionCallback(object sender, string targetHost, X509CertificateCollection localCertificates, X509Certificate remoteCertificate, string[] acceptableIssuers);
    public class NegotiateStream : AuthenticatedStream {
      public NegotiateStream(Stream innerStream);
      public NegotiateStream(Stream innerStream, bool leaveInnerStreamOpen);
      public override bool CanRead { get; }
      public override bool CanSeek { get; }
      public override bool CanTimeout { get; }
      public override bool CanWrite { get; }
      public virtual TokenImpersonationLevel ImpersonationLevel { get; }
      public override bool IsAuthenticated { get; }
      public override bool IsEncrypted { get; }
      public override bool IsMutuallyAuthenticated { get; }
      public override bool IsServer { get; }
      public override bool IsSigned { get; }
      public override long Length { get; }
      public override long Position { get; set; }
      public override int ReadTimeout { get; set; }
      public virtual IIdentity RemoteIdentity { get; }
      public override int WriteTimeout { get; set; }
      public virtual void AuthenticateAsClient();
      ^ Immo Landwerth: Remove. We decided that it's best to only have async APIs.
      public virtual void AuthenticateAsClient(NetworkCredential credential, ChannelBinding binding, string targetName);
      public virtual void AuthenticateAsClient(NetworkCredential credential, ChannelBinding binding, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel);
      public virtual void AuthenticateAsClient(NetworkCredential credential, string targetName);
      public virtual void AuthenticateAsClient(NetworkCredential credential, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel);
      public virtual Task AuthenticateAsClientAsync();
      public virtual Task AuthenticateAsClientAsync(NetworkCredential credential, ChannelBinding binding, string targetName);
      public virtual Task AuthenticateAsClientAsync(NetworkCredential credential, ChannelBinding binding, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel);
      public virtual Task AuthenticateAsClientAsync(NetworkCredential credential, string targetName);
      public virtual Task AuthenticateAsClientAsync(NetworkCredential credential, string targetName, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel allowedImpersonationLevel);
      public virtual void AuthenticateAsServer();
      ^ Immo Landwerth: Remove. We decided that it's best to only have async APIs.
      public virtual void AuthenticateAsServer(NetworkCredential credential, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel);
      public virtual void AuthenticateAsServer(NetworkCredential credential, ExtendedProtectionPolicy policy, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel);
      public virtual void AuthenticateAsServer(ExtendedProtectionPolicy policy);
      public virtual Task AuthenticateAsServerAsync();
      public virtual Task AuthenticateAsServerAsync(NetworkCredential credential, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel);
      public virtual Task AuthenticateAsServerAsync(NetworkCredential credential, ExtendedProtectionPolicy policy, ProtectionLevel requiredProtectionLevel, TokenImpersonationLevel requiredImpersonationLevel);
      public virtual Task AuthenticateAsServerAsync(ExtendedProtectionPolicy policy);
      public override void Flush();
      public override int Read(byte[] buffer, int offset, int count);
      public override long Seek(long offset, SeekOrigin origin);
      public override void SetLength(long value);
      public override void Write(byte[] buffer, int offset, int count);
    }
    public enum ProtectionLevel {
      EncryptAndSign = 2,
      None = 0,
      Sign = 1,
    }
    public delegate bool RemoteCertificateValidationCallback(object sender, X509Certificate certificate, X509Chain chain, SslPolicyErrors sslPolicyErrors);
    public class SslStream : AuthenticatedStream {
      public SslStream(Stream innerStream);
      public SslStream(Stream innerStream, bool leaveInnerStreamOpen);
      public SslStream(Stream innerStream, bool leaveInnerStreamOpen, RemoteCertificateValidationCallback userCertificateValidationCallback);
      public SslStream(Stream innerStream, bool leaveInnerStreamOpen, RemoteCertificateValidationCallback userCertificateValidationCallback, LocalCertificateSelectionCallback userCertificateSelectionCallback);
      public SslStream(Stream innerStream, bool leaveInnerStreamOpen, RemoteCertificateValidationCallback userCertificateValidationCallback, LocalCertificateSelectionCallback userCertificateSelectionCallback, EncryptionPolicy encryptionPolicy);
      public override bool CanRead { get; }
      public override bool CanSeek { get; }
      public override bool CanTimeout { get; }
      public override bool CanWrite { get; }
      public virtual bool CheckCertRevocationStatus { get; }
      public virtual CipherAlgorithmType CipherAlgorithm { get; }
      public virtual int CipherStrength { get; }
      public virtual HashAlgorithmType HashAlgorithm { get; }
      public virtual int HashStrength { get; }
      public override bool IsAuthenticated { get; }
      public override bool IsEncrypted { get; }
      public override bool IsMutuallyAuthenticated { get; }
      public override bool IsServer { get; }
      public override bool IsSigned { get; }
      public virtual ExchangeAlgorithmType KeyExchangeAlgorithm { get; }
      public virtual int KeyExchangeStrength { get; }
      public override long Length { get; }
      public virtual X509Certificate LocalCertificate { get; }
      public override long Position { get; set; }
      public override int ReadTimeout { get; set; }
      public virtual X509Certificate RemoteCertificate { get; }
      public virtual SslProtocols SslProtocol { get; }
      public TransportContext TransportContext { get; }
      public override int WriteTimeout { get; set; }
      public virtual void AuthenticateAsClient(string targetHost);
      ^ Immo Landwerth: Remove. We decided that it's best to only have async APIs.
      public virtual void AuthenticateAsClient(string targetHost, X509CertificateCollection clientCertificates, SslProtocols enabledSslProtocols, bool checkCertificateRevocation);
      public virtual Task AuthenticateAsClientAsync(string targetHost);
      public virtual Task AuthenticateAsClientAsync(string targetHost, X509CertificateCollection clientCertificates, SslProtocols enabledSslProtocols, bool checkCertificateRevocation);
      public virtual void AuthenticateAsServer(X509Certificate serverCertificate);
      ^ Immo Landwerth: Remove. Same here.
      public virtual void AuthenticateAsServer(X509Certificate serverCertificate, bool clientCertificateRequired, SslProtocols enabledSslProtocols, bool checkCertificateRevocation);
      public virtual Task AuthenticateAsServerAsync(X509Certificate serverCertificate);
      public virtual Task AuthenticateAsServerAsync(X509Certificate serverCertificate, bool clientCertificateRequired, SslProtocols enabledSslProtocols, bool checkCertificateRevocation);
      public override void Flush();
      public override int Read(byte[] buffer, int offset, int count);
      public override long Seek(long offset, SeekOrigin origin);
      public override void SetLength(long value);
      public void Write(byte[] buffer);
      public override void Write(byte[] buffer, int offset, int count);
    }
  }
  namespace System.Security.Authentication {
    public class AuthenticationException : Exception {
      public AuthenticationException();
      public AuthenticationException(string message);
      public AuthenticationException(string message, Exception innerException);
    }
  }
  namespace System.Security.Authentication.ExtendedProtection {
    public class ExtendedProtectionPolicy {
      public ExtendedProtectionPolicy(PolicyEnforcement policyEnforcement);
      public ExtendedProtectionPolicy(PolicyEnforcement policyEnforcement, ChannelBinding customChannelBinding);
      public ExtendedProtectionPolicy(PolicyEnforcement policyEnforcement, ProtectionScenario protectionScenario, ICollection customServiceNames);
      public ExtendedProtectionPolicy(PolicyEnforcement policyEnforcement, ProtectionScenario protectionScenario, ServiceNameCollection customServiceNames);
      public ChannelBinding CustomChannelBinding { get; }
      public ServiceNameCollection CustomServiceNames { get; }
      public static bool OSSupportsExtendedProtection { get; }
      public PolicyEnforcement PolicyEnforcement { get; }
      public ProtectionScenario ProtectionScenario { get; }
      public override string ToString();
    }
    public enum PolicyEnforcement {
      Always = 2,
      Never = 0,
      WhenSupported = 1,
    }
    public enum ProtectionScenario {
      TransportSelected = 0,
      TrustedProxy = 1,
    }
    public class ServiceNameCollection : ICollection, IEnumerable {
      public ServiceNameCollection(ICollection items);
      public int Count { get; }
      bool System.Collections.ICollection.IsSynchronized { get; }
      object System.Collections.ICollection.SyncRoot { get; }
      public bool Contains(string searchServiceName);
      public IEnumerator GetEnumerator();
      public ServiceNameCollection Merge(IEnumerable serviceNames);
      public ServiceNameCollection Merge(string serviceName);
      void System.Collections.ICollection.CopyTo(Array array, int index);
    }
  }
 }
 assembly System.Net.Sockets {
  namespace System.Net.Sockets {
    public enum IOControlCode : long {
      AbsorbRouterAlert = (long)2550136837,
      AddMulticastGroupOnInterface = (long)2550136842,
      AddressListChange = (long)671088663,
      AddressListQuery = (long)1207959574,
      AddressListSort = (long)3355443225,
      AssociateHandle = (long)2281701377,
      AsyncIO = (long)2147772029,
      BindToInterface = (long)2550136840,
      DataToRead = (long)1074030207,
      DeleteMulticastGroupFromInterface = (long)2550136843,
      EnableCircularQueuing = (long)671088642,
      Flush = (long)671088644,
      GetBroadcastAddress = (long)1207959557,
      GetExtensionFunctionPointer = (long)3355443206,
      GetGroupQos = (long)3355443208,
      GetQos = (long)3355443207,
      KeepAliveValues = (long)2550136836,
      LimitBroadcasts = (long)2550136839,
      MulticastInterface = (long)2550136841,
      MulticastScope = (long)2281701386,
      MultipointLoopback = (long)2281701385,
      NamespaceChange = (long)2281701401,
      NonBlockingIO = (long)2147772030,
      OobDataRead = (long)1074033415,
      QueryTargetPnpHandle = (long)1207959576,
      ReceiveAll = (long)2550136833,
      ReceiveAllIgmpMulticast = (long)2550136835,
      ReceiveAllMulticast = (long)2550136834,
      RoutingInterfaceChange = (long)2281701397,
      RoutingInterfaceQuery = (long)3355443220,
      SetGroupQos = (long)2281701388,
      SetQos = (long)2281701387,
      TranslateHandle = (long)3355443213,
      UnicastInterface = (long)2550136838,
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct IPPacketInformation {
      public IPAddress Address { get; }
      public int Interface { get; }
      public override bool Equals(object comparand);
      public override int GetHashCode();
      public static bool operator ==(IPPacketInformation packetInformation1, IPPacketInformation packetInformation2);
      public static bool operator !=(IPPacketInformation packetInformation1, IPPacketInformation packetInformation2);
    }
    public enum IPProtectionLevel {
      EdgeRestricted = 20,
      Restricted = 30,
      Unrestricted = 10,
      Unspecified = -1,
    }
    public class IPv6MulticastOption {
      public IPv6MulticastOption(IPAddress group);
      public IPv6MulticastOption(IPAddress group, long ifindex);
      public IPAddress Group { get; set; }
      public long InterfaceIndex { get; set; }
    }
    public class LingerOption {
      public LingerOption(bool enable, int seconds);
      public bool Enabled { get; set; }
      public int LingerTime { get; set; }
    }
    public class MulticastOption {
      public MulticastOption(IPAddress group);
      public MulticastOption(IPAddress group, int interfaceIndex);
      public MulticastOption(IPAddress group, IPAddress mcint);
      public IPAddress Group { get; set; }
      public int InterfaceIndex { get; set; }
      public IPAddress LocalAddress { get; set; }
    }
    public class NetworkStream : Stream {
      public NetworkStream(Socket socket);
      public NetworkStream(Socket socket, bool ownsSocket);
      public override bool CanRead { get; }
      public override bool CanSeek { get; }
      public override bool CanTimeout { get; }
      public override bool CanWrite { get; }
      public virtual bool DataAvailable { get; }
      public override long Length { get; }
      public override long Position { get; set; }
      public override int ReadTimeout { get; set; }
      public override int WriteTimeout { get; set; }
      protected override void Dispose(bool disposing);
      ~NetworkStream();
      public override void Flush();
      public override Task FlushAsync(CancellationToken cancellationToken);
      public override int Read(byte[] buffer, int offset, int size);
      public override Task<int> ReadAsync(byte[] buffer, int offset, int size, CancellationToken cancellationToken);
      public override long Seek(long offset, SeekOrigin origin);
      public override void SetLength(long value);
      public override void Write(byte[] buffer, int offset, int size);
      public override Task WriteAsync(byte[] buffer, int offset, int size, CancellationToken cancellationToken);
    }
    public enum ProtocolType {
      Ggp = 3,
      Icmp = 1,
      IcmpV6 = 58,
      Idp = 22,
      Igmp = 2,
      IP = 0,
      IPSecAuthenticationHeader = 51,
      IPSecEncapsulatingSecurityPayload = 50,
      IPv4 = 4,
      IPv6 = 41,
      IPv6DestinationOptions = 60,
      IPv6FragmentHeader = 44,
      IPv6HopByHopOptions = 0,
      IPv6NoNextHeader = 59,
      IPv6RoutingHeader = 43,
      Ipx = 1000,
      ND = 77,
      Pup = 12,
      Raw = 255,
      Spx = 1256,
      SpxII = 1257,
      Tcp = 6,
      Udp = 17,
      Unknown = -1,
      Unspecified = 0,
    }
    public enum SelectMode {
      SelectError = 2,
      SelectRead = 0,
      SelectWrite = 1,
    }
    public class SendPacketsElement {
      public SendPacketsElement(byte[] buffer);
      public SendPacketsElement(byte[] buffer, int offset, int count);
      public SendPacketsElement(byte[] buffer, int offset, int count, bool endOfPacket);
      public SendPacketsElement(string filepath);
      public SendPacketsElement(string filepath, int offset, int count);
      public SendPacketsElement(string filepath, int offset, int count, bool endOfPacket);
      public byte[] Buffer { get; }
      public int Count { get; }
      public bool EndOfPacket { get; }
      public string FilePath { get; }
      public int Offset { get; }
    }
    public class Socket : IDisposable {
      public Socket(AddressFamily addressFamily, SocketType socketType, ProtocolType protocolType);
      public Socket(SocketType socketType, ProtocolType protocolType);
      public AddressFamily AddressFamily { get; }
      public int Available { get; }
      public bool Blocking { get; set; }
      public bool Connected { get; }
      public bool DontFragment { get; set; }
      public bool DualMode { get; set; }
      public bool EnableBroadcast { get; set; }
      public bool ExclusiveAddressUse { get; set; }
      public bool IsBound { get; }
      public LingerOption LingerState { get; set; }
      public EndPoint LocalEndPoint { get; }
      public bool MulticastLoopback { get; set; }
      public bool NoDelay { get; set; }
      public static bool OSSupportsIPv4 { get; }
      public static bool OSSupportsIPv6 { get; }
      public ProtocolType ProtocolType { get; }
      public int ReceiveBufferSize { get; set; }
      public int ReceiveTimeout { get; set; }
      public EndPoint RemoteEndPoint { get; }
      public int SendBufferSize { get; set; }
      public int SendTimeout { get; set; }
      public SocketType SocketType { get; }
      public short Ttl { get; set; }
      public Socket Accept();
      public bool AcceptAsync(SocketAsyncEventArgs e);
      public void Bind(EndPoint localEP);
      public static void CancelConnectAsync(SocketAsyncEventArgs e);
      public void Connect(EndPoint remoteEP);
      public void Connect(IPAddress address, int port);
      public void Connect(IPAddress[] addresses, int port);
      public void Connect(string host, int port);
      public bool ConnectAsync(SocketAsyncEventArgs e);
      public static bool ConnectAsync(SocketType socketType, ProtocolType protocolType, SocketAsyncEventArgs e);
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      ~Socket();
      public object GetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName);
      public void GetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, byte[] optionValue);
      public byte[] GetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, int optionLength);
      public int IOControl(int ioControlCode, byte[] optionInValue, byte[] optionOutValue);
      public int IOControl(IOControlCode ioControlCode, byte[] optionInValue, byte[] optionOutValue);
      public void Listen(int backlog);
      public bool Poll(int microSeconds, SelectMode mode);
      public int Receive(byte[] buffer);
      public int Receive(byte[] buffer, int offset, int size, SocketFlags socketFlags);
      public int Receive(byte[] buffer, int offset, int size, SocketFlags socketFlags, out SocketError errorCode);
      public int Receive(byte[] buffer, int size, SocketFlags socketFlags);
      public int Receive(byte[] buffer, SocketFlags socketFlags);
      public int Receive(IList<ArraySegment<byte>> buffers);
      public int Receive(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
      public int Receive(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, out SocketError errorCode);
      public bool ReceiveAsync(SocketAsyncEventArgs e);
      public int ReceiveFrom(byte[] buffer, int offset, int size, SocketFlags socketFlags, ref EndPoint remoteEP);
      public int ReceiveFrom(byte[] buffer, int size, SocketFlags socketFlags, ref EndPoint remoteEP);
      public int ReceiveFrom(byte[] buffer, ref EndPoint remoteEP);
      public int ReceiveFrom(byte[] buffer, SocketFlags socketFlags, ref EndPoint remoteEP);
      public bool ReceiveFromAsync(SocketAsyncEventArgs e);
      public int ReceiveMessageFrom(byte[] buffer, int offset, int size, ref SocketFlags socketFlags, ref EndPoint remoteEP, out IPPacketInformation ipPacketInformation);
      public bool ReceiveMessageFromAsync(SocketAsyncEventArgs e);
      public static void Select(IList checkRead, IList checkWrite, IList checkError, int microSeconds);
      public int Send(byte[] buffer);
      public int Send(byte[] buffer, int offset, int size, SocketFlags socketFlags);
      public int Send(byte[] buffer, int offset, int size, SocketFlags socketFlags, out SocketError errorCode);
      public int Send(byte[] buffer, int size, SocketFlags socketFlags);
      public int Send(byte[] buffer, SocketFlags socketFlags);
      public int Send(IList<ArraySegment<byte>> buffers);
      public int Send(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
      public int Send(IList<ArraySegment<byte>> buffers, SocketFlags socketFlags, out SocketError errorCode);
      public bool SendAsync(SocketAsyncEventArgs e);
      public bool SendPacketsAsync(SocketAsyncEventArgs e);
      public int SendTo(byte[] buffer, int offset, int size, SocketFlags socketFlags, EndPoint remoteEP);
      public int SendTo(byte[] buffer, int size, SocketFlags socketFlags, EndPoint remoteEP);
      public int SendTo(byte[] buffer, EndPoint remoteEP);
      public int SendTo(byte[] buffer, SocketFlags socketFlags, EndPoint remoteEP);
      public bool SendToAsync(SocketAsyncEventArgs e);
      public void SetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, bool optionValue);
      public void SetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, byte[] optionValue);
      public void SetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, int optionValue);
      public void SetSocketOption(SocketOptionLevel optionLevel, SocketOptionName optionName, object optionValue);
      public void Shutdown(SocketShutdown how);
    }
    public class SocketAsyncEventArgs : EventArgs, IDisposable {
      public SocketAsyncEventArgs();
      public Socket AcceptSocket { get; set; }
      public byte[] Buffer { get; }
      public IList<ArraySegment<byte>> BufferList { get; set; }
      public int BytesTransferred { get; }
      public Exception ConnectByNameError { get; }
      public Socket ConnectSocket { get; }
      public int Count { get; }
      public SocketAsyncOperation LastOperation { get; }
      public int Offset { get; }
      public IPPacketInformation ReceiveMessageFromPacketInfo { get; }
      public EndPoint RemoteEndPoint { get; set; }
      public SendPacketsElement[] SendPacketsElements { get; set; }
      public int SendPacketsSendSize { get; set; }
      public SocketError SocketError { get; set; }
      public SocketFlags SocketFlags { get; set; }
      public object UserToken { get; set; }
      public event EventHandler<SocketAsyncEventArgs> Completed;
      public void Dispose();
      ~SocketAsyncEventArgs();
      protected virtual void OnCompleted(SocketAsyncEventArgs e);
      public void SetBuffer(byte[] buffer, int offset, int count);
      public void SetBuffer(int offset, int count);
    }
    public enum SocketAsyncOperation {
      Accept = 1,
      Connect = 2,
      Disconnect = 3,
      None = 0,
      Receive = 4,
      ReceiveFrom = 5,
      ReceiveMessageFrom = 6,
      Send = 7,
      SendPackets = 8,
      SendTo = 9,
    }
    public enum SocketFlags {
      Broadcast = 1024,
      ControlDataTruncated = 512,
      DontRoute = 4,
      Multicast = 2048,
      None = 0,
      OutOfBand = 1,
      Partial = 32768,
      Peek = 2,
      Truncated = 256,
    }
    public enum SocketOptionLevel {
      IP = 0,
      IPv6 = 41,
      Socket = 65535,
      Tcp = 6,
      Udp = 17,
    }
    public enum SocketOptionName {
      AcceptConnection = 2,
      AddMembership = 12,
      AddSourceMembership = 15,
      BlockSource = 17,
      Broadcast = 32,
      BsdUrgent = 2,
      ChecksumCoverage = 20,
      Debug = 1,
      DontFragment = 14,
      DontLinger = -129,
      DontRoute = 16,
      DropMembership = 13,
      DropSourceMembership = 16,
      Error = 4103,
      ExclusiveAddressUse = -5,
      Expedited = 2,
      HeaderIncluded = 2,
      HopLimit = 21,
      IPOptions = 1,
      IPProtectionLevel = 23,
      IpTimeToLive = 4,
      IPv6Only = 27,
      KeepAlive = 8,
      Linger = 128,
      MaxConnections = 2147483647,
      MulticastInterface = 9,
      MulticastLoopback = 11,
      MulticastTimeToLive = 10,
      NoChecksum = 1,
      NoDelay = 1,
      OutOfBandInline = 256,
      PacketInformation = 19,
      ReceiveBuffer = 4098,
      ReceiveLowWater = 4100,
      ReceiveTimeout = 4102,
      ReuseAddress = 4,
      SendBuffer = 4097,
      SendLowWater = 4099,
      SendTimeout = 4101,
      Type = 4104,
      TypeOfService = 3,
      UnblockSource = 18,
      UpdateAcceptContext = 28683,
      UpdateConnectContext = 28688,
      UseLoopback = 64,
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct SocketReceiveFromResult {
      public int ReceivedBytes;
      public EndPoint RemoteEndPoint;
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct SocketReceiveMessageFromResult {
      public int ReceivedBytes;
      public EndPoint RemoteEndPoint;
      public IPPacketInformation PacketInformation;
      public SocketFlags SocketFlags;
    }
    public enum SocketShutdown {
      Both = 2,
      Receive = 0,
      Send = 1,
    }
    public static class SocketTaskExtensions {
      public static Task<Socket> AcceptAsync(this Socket socket);
      public static Task<Socket> AcceptAsync(this Socket socket, Socket acceptSocket);
      public static Task ConnectAsync(this Socket socket, EndPoint remoteEP);
      public static Task ConnectAsync(this Socket socket, IPAddress address, int port);
      public static Task ConnectAsync(this Socket socket, IPAddress[] addresses, int port);
      public static Task ConnectAsync(this Socket socket, string host, int port);
      public static Task<int> ReceiveAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags);
      public static Task<int> ReceiveAsync(this Socket socket, IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
      public static Task<SocketReceiveFromResult> ReceiveFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
      public static Task<SocketReceiveMessageFromResult> ReceiveMessageFromAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEndPoint);
      public static Task<int> SendAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags);
      public static Task<int> SendAsync(this Socket socket, IList<ArraySegment<byte>> buffers, SocketFlags socketFlags);
      public static Task<int> SendToAsync(this Socket socket, ArraySegment<byte> buffer, SocketFlags socketFlags, EndPoint remoteEP);
    }
    public enum SocketType {
      Dgram = 2,
      Raw = 3,
      Rdm = 4,
      Seqpacket = 5,
      Stream = 1,
      Unknown = -1,
    }
    public class TcpClient : IDisposable {
      public TcpClient();
      public TcpClient(AddressFamily family);
      protected bool Active { get; set; }
      public int Available { get; }
      public Socket Client { get; set; }
      public bool Connected { get; }
      public bool ExclusiveAddressUse { get; set; }
      public LingerOption LingerState { get; set; }
      public bool NoDelay { get; set; }
      public int ReceiveBufferSize { get; set; }
      public int ReceiveTimeout { get; set; }
      public int SendBufferSize { get; set; }
      public int SendTimeout { get; set; }
      public Task ConnectAsync(IPAddress address, int port);
      public Task ConnectAsync(IPAddress[] addresses, int port);
      public Task ConnectAsync(string host, int port);
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      ~TcpClient();
      public NetworkStream GetStream();
    }
    public class TcpListener {
      public TcpListener(IPAddress localaddr, int port);
      public TcpListener(IPEndPoint localEP);
      protected bool Active { get; }
      public bool ExclusiveAddressUse { get; set; }
      public EndPoint LocalEndpoint { get; }
      public Socket Server { get; }
      public Task<Socket> AcceptSocketAsync();
      public Task<TcpClient> AcceptTcpClientAsync();
      public bool Pending();
      public void Start();
      public void Start(int backlog);
      public void Stop();
    }
    public class UdpClient : IDisposable {
      public UdpClient();
      public UdpClient(int port);
      public UdpClient(int port, AddressFamily family);
      public UdpClient(IPEndPoint localEP);
      public UdpClient(AddressFamily family);
      protected bool Active { get; set; }
      public int Available { get; }
      public Socket Client { get; set; }
      public bool DontFragment { get; set; }
      public bool EnableBroadcast { get; set; }
      public bool ExclusiveAddressUse { get; set; }
      public bool MulticastLoopback { get; set; }
      public short Ttl { get; set; }
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public void DropMulticastGroup(IPAddress multicastAddr);
      public void DropMulticastGroup(IPAddress multicastAddr, int ifindex);
      public void JoinMulticastGroup(int ifindex, IPAddress multicastAddr);
      public void JoinMulticastGroup(IPAddress multicastAddr);
      public void JoinMulticastGroup(IPAddress multicastAddr, int timeToLive);
      public void JoinMulticastGroup(IPAddress multicastAddr, IPAddress localAddress);
      public Task<UdpReceiveResult> ReceiveAsync();
      public Task<int> SendAsync(byte[] datagram, int bytes, IPEndPoint endPoint);
      public Task<int> SendAsync(byte[] datagram, int bytes, string hostname, int port);
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct UdpReceiveResult : IEquatable<UdpReceiveResult> {
      public UdpReceiveResult(byte[] buffer, IPEndPoint remoteEndPoint);
      public byte[] Buffer { get; }
      public IPEndPoint RemoteEndPoint { get; }
      public bool Equals(UdpReceiveResult other);
      public override bool Equals(object obj);
      public override int GetHashCode();
      public static bool operator ==(UdpReceiveResult left, UdpReceiveResult right);
      public static bool operator !=(UdpReceiveResult left, UdpReceiveResult right);
    }
  }
 }
 assembly System.Net.Utilities {
  namespace System.Net.NetworkInformation {
    public enum IPStatus {
      ^ Immo Landwerth: Should this be in System.Net.Primitives?
      | Immo Landwerth:
      | Immo Landwerth: --> Up to the networking team.
      | Immo Landwerth: --> We can always type forward it down to S.N.Primitives, if neccessary.
      BadDestination = 11018,
      BadHeader = 11042,
      BadOption = 11007,
      BadRoute = 11012,
      DestinationHostUnreachable = 11003,
      DestinationNetworkUnreachable = 11002,
      DestinationPortUnreachable = 11005,
      DestinationProhibited = 11004,
      DestinationProtocolUnreachable = 11004,
      DestinationScopeMismatch = 11045,
      DestinationUnreachable = 11040,
      HardwareError = 11008,
      IcmpError = 11044,
      NoResources = 11006,
      PacketTooBig = 11009,
      ParameterProblem = 11015,
      SourceQuench = 11016,
      Success = 0,
      TimedOut = 11010,
      TimeExceeded = 11041,
      TtlExpired = 11013,
      TtlReassemblyTimeExceeded = 11014,
      Unknown = -1,
      UnrecognizedNextHeader = 11043,
    }
    public class Ping : IDisposable {
      public Ping();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public Task<PingReply> SendPingAsync(IPAddress address);
      public Task<PingReply> SendPingAsync(IPAddress address, int timeout);
      public Task<PingReply> SendPingAsync(IPAddress address, int timeout, byte[] buffer);
      public Task<PingReply> SendPingAsync(IPAddress address, int timeout, byte[] buffer, PingOptions options);
      public Task<PingReply> SendPingAsync(string hostNameOrAddress);
      public Task<PingReply> SendPingAsync(string hostNameOrAddress, int timeout);
      public Task<PingReply> SendPingAsync(string hostNameOrAddress, int timeout, byte[] buffer);
      public Task<PingReply> SendPingAsync(string hostNameOrAddress, int timeout, byte[] buffer, PingOptions options);
    }
    public class PingException : InvalidOperationException {
      public PingException(string message);
      public PingException(string message, Exception innerException);
    }
    public class PingOptions {
      public PingOptions();
      public PingOptions(int ttl, bool dontFragment);
      public bool DontFragment { get; set; }
      public int Ttl { get; set; }
    }
    public class PingReply {
      public IPAddress Address { get; }
      public byte[] Buffer { get; }
      public PingOptions Options { get; }
      public long RoundtripTime { get; }
      public IPStatus Status { get; }
    }
  }
 }
 assembly System.Net.WebHeaderCollection {
  namespace System.Net {
    public enum HttpRequestHeader {
      Accept = 20,
      AcceptCharset = 21,
      AcceptEncoding = 22,
      AcceptLanguage = 23,
      Allow = 10,
      Authorization = 24,
      CacheControl = 0,
      Connection = 1,
      ContentEncoding = 13,
      ContentLanguage = 14,
      ContentLength = 11,
      ContentLocation = 15,
      ContentMd5 = 16,
      ContentRange = 17,
      ContentType = 12,
      Cookie = 25,
      Date = 2,
      Expect = 26,
      Expires = 18,
      From = 27,
      Host = 28,
      IfMatch = 29,
      IfModifiedSince = 30,
      IfNoneMatch = 31,
      IfRange = 32,
      IfUnmodifiedSince = 33,
      KeepAlive = 3,
      LastModified = 19,
      MaxForwards = 34,
      Pragma = 4,
      ProxyAuthorization = 35,
      Range = 37,
      Referer = 36,
      Te = 38,
      Trailer = 5,
      TransferEncoding = 6,
      Translate = 39,
      Upgrade = 7,
      UserAgent = 40,
      Via = 8,
      Warning = 9,
    }
    public enum HttpResponseHeader {
      AcceptRanges = 20,
      Age = 21,
      Allow = 10,
      CacheControl = 0,
      Connection = 1,
      ContentEncoding = 13,
      ContentLanguage = 14,
      ContentLength = 11,
      ContentLocation = 15,
      ContentMd5 = 16,
      ContentRange = 17,
      ContentType = 12,
      Date = 2,
      ETag = 22,
      Expires = 18,
      KeepAlive = 3,
      LastModified = 19,
      Location = 23,
      Pragma = 4,
      ProxyAuthenticate = 24,
      RetryAfter = 25,
      Server = 26,
      SetCookie = 27,
      Trailer = 5,
      TransferEncoding = 6,
      Upgrade = 7,
      Vary = 28,
      Via = 8,
      Warning = 9,
      WwwAuthenticate = 29,
    }
    public sealed class WebHeaderCollection : IEnumerable {
      public WebHeaderCollection();
      public string[] AllKeys { get; }
      public int Count { get; }
      public string this[HttpRequestHeader header] { get; set; }
      public string this[HttpResponseHeader header] { get; set; }
      public string this[string name] { get; set; }
      public void Remove(string name);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
      public override string ToString();
    }
  }
 }
 assembly System.Net.WebSockets {
  namespace System.Net.WebSockets {
    public abstract class WebSocket : IDisposable {
      protected WebSocket();
      public abstract Nullable<WebSocketCloseStatus> CloseStatus { get; }
      public abstract string CloseStatusDescription { get; }
      public abstract WebSocketState State { get; }
      public abstract string SubProtocol { get; }
      public abstract void Abort();
      public abstract Task CloseAsync(WebSocketCloseStatus closeStatus, string statusDescription, CancellationToken cancellationToken);
      public abstract Task CloseOutputAsync(WebSocketCloseStatus closeStatus, string statusDescription, CancellationToken cancellationToken);
      public abstract void Dispose();
      public abstract Task<WebSocketReceiveResult> ReceiveAsync(ArraySegment<byte> buffer, CancellationToken cancellationToken);
      public abstract Task SendAsync(ArraySegment<byte> buffer, WebSocketMessageType messageType, bool endOfMessage, CancellationToken cancellationToken);
    }
    public enum WebSocketCloseStatus {
      Empty = 1005,
      EndpointUnavailable = 1001,
      InternalServerError = 1011,
      InvalidMessageType = 1003,
      InvalidPayloadData = 1007,
      MandatoryExtension = 1010,
      MessageTooBig = 1009,
      NormalClosure = 1000,
      PolicyViolation = 1008,
      ProtocolError = 1002,
    }
    public enum WebSocketError {
      ConnectionClosedPrematurely = 8,
      Faulted = 2,
      HeaderError = 7,
      InvalidMessageType = 1,
      InvalidState = 9,
      NativeError = 3,
      NotAWebSocket = 4,
      Success = 0,
      UnsupportedProtocol = 6,
      UnsupportedVersion = 5,
    }
    public sealed class WebSocketException : Exception {
      public WebSocketException(int nativeError);
      public WebSocketException(int nativeError, Exception innerException);
      public WebSocketException(int nativeError, string message);
      public WebSocketException(WebSocketError error);
      public WebSocketException(WebSocketError error, Exception innerException);
      public WebSocketException(WebSocketError error, int nativeError);
      public WebSocketException(WebSocketError error, int nativeError, Exception innerException);
      public WebSocketException(WebSocketError error, int nativeError, string message);
      public WebSocketException(WebSocketError error, int nativeError, string message, Exception innerException);
      public WebSocketException(WebSocketError error, string message);
      public WebSocketException(WebSocketError error, string message, Exception innerException);
      public WebSocketException(string message);
      public WebSocketException(string message, Exception innerException);
      public int ErrorCode { get; }
      public WebSocketError WebSocketErrorCode { get; }
    }
    public enum WebSocketMessageType {
      Binary = 1,
      Close = 2,
      Text = 0,
    }
    public class WebSocketReceiveResult {
      public WebSocketReceiveResult(int count, WebSocketMessageType messageType, bool endOfMessage);
      public WebSocketReceiveResult(int count, WebSocketMessageType messageType, bool endOfMessage, Nullable<WebSocketCloseStatus> closeStatus, string closeStatusDescription);
      public Nullable<WebSocketCloseStatus> CloseStatus { get; }
      public string CloseStatusDescription { get; }
      public int Count { get; }
      public bool EndOfMessage { get; }
      public WebSocketMessageType MessageType { get; }
    }
    public enum WebSocketState {
      Aborted = 6,
      Closed = 5,
      CloseReceived = 4,
      CloseSent = 3,
      Connecting = 1,
      None = 0,
      Open = 2,
    }
  }
 }
 assembly System.Net.WebSockets.Client {
  namespace System.Net.WebSockets {
    public sealed class ClientWebSocket : WebSocket {
      public ClientWebSocket();
      public override Nullable<WebSocketCloseStatus> CloseStatus { get; }
      public override string CloseStatusDescription { get; }
      public ClientWebSocketOptions Options { get; }
      public override WebSocketState State { get; }
      public override string SubProtocol { get; }
      public override void Abort();
      public override Task CloseAsync(WebSocketCloseStatus closeStatus, string statusDescription, CancellationToken cancellationToken);
      public override Task CloseOutputAsync(WebSocketCloseStatus closeStatus, string statusDescription, CancellationToken cancellationToken);
      public Task ConnectAsync(Uri uri, CancellationToken cancellationToken);
      public override void Dispose();
      public override Task<WebSocketReceiveResult> ReceiveAsync(ArraySegment<byte> buffer, CancellationToken cancellationToken);
      public override Task SendAsync(ArraySegment<byte> buffer, WebSocketMessageType messageType, bool endOfMessage, CancellationToken cancellationToken);
    }
    public sealed class ClientWebSocketOptions {
      public X509CertificateCollection ClientCertificates { get; set; }
      public CookieContainer Cookies { get; set; }
      public ICredentials Credentials { get; set; }
      public TimeSpan KeepAliveInterval { get; set; }
      public IWebProxy Proxy { get; set; }
      public void AddSubProtocol(string subProtocol);
      public void SetRequestHeader(string headerName, string headerValue);
    }
  }
 }
```

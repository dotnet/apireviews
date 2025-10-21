# API Review 10/21/2025

## DataIngestion: Pipelines

**Approved** | [#extensions/6895](https://github.com/dotnet/extensions/issues/6895#issuecomment-3428564724) | [Video](https://www.youtube.com/watch?v=Gzuo8S2H-bM&t=0h0m0s)


* Valid, non-zero default parameters are generally disrecommended, so we updated them to use nullable (default default)
* IngestionChunk.TokenCount seemed weird as nullable.  The two stances were either mandatory, or remove it.  Either is fine.
  * For purposes of completion it has been marked as mandatory here.
* ChunkerOptions gains a Tokenizer property, the Chunker types lose it as a ctor parameter.
* Added `ReadOnlySpan<string>` overloads to string[]-accepting ctors.
* Changed IngestionChunk.Metadata from concrete Dictionary to IDictionary, per guidelines.

```csharp
namespace Microsoft.Extensions.DataIngestion;

public abstract class IngestionDocumentReader
{
    public Task<IngestionDocument> ReadAsync(FileInfo source, CancellationToken cancellationToken = default)
        => ReadAsync(source, source.FullName, cancellationToken);

    public virtual Task<IngestionDocument> ReadAsync(FileInfo source, string identifier, string? mediaType = null, CancellationToken cancellationToken = default);

    public abstract Task<IngestionDocument> ReadAsync(Stream source, string mediaType, string identifier, CancellationToken cancellationToken = default);
}

public abstract class IngestionDocumentProcessor
{
    public abstract Task<IngestionDocument> ProcessAsync(IngestionDocument document, CancellationToken cancellationToken = default);
}

public sealed class ImageAlternativeTextEnricher : IngestionDocumentProcessor
{
    public ImageAlternativeTextEnricher(IChatClient chatClient, ChatOptions? chatOptions = null);
}

public sealed class IngestionChunk<T>
{
    public T Content { get; }
    public IngestionDocument Document { get; }
    public int TokenCount { get; }
    public string? Context { get; }
    public IDictionary<string, object> Metadata { get; }
    public bool HasMetadata { get; }

    public IngestionChunk(T content, Document document, int tokenCount, string? context = null);
}

public abstract class DocumentChunker<T>
{
    public abstract IAsyncEnumerable<IngestionChunk<T>> ProcessAsync(IngestionDocument document, CancellationToken cancellationToken = default);
}

// the class is not sealed on purpose, so creating a derived type specific for given chunker is possible
public class ChunkerOptions
{
    public ChunkerOptions(Tokenizer tokenizer);

    // Default values come from https://learn.microsoft.com/en-us/azure/search/vector-search-how-to-chunk-documents#text-split-skill-example

    /// <summary>
    /// The maximum number of tokens allowed in each chunk. Default is 2000.
    /// </summary>
    public int MaxTokensPerChunk { get; set; } = 2_000;
    
    /// <summary>
    /// The number of overlapping tokens between consecutive chunks. Default is 500.
    /// </summary>
    public int OverlapTokens { get; set; } = 500;

    public Tokenizer Tokenizer { get; }
}

// Splits a Document into chunks based on semantic similarity between its elements.
public sealed class SemanticSimilarityChunker : DocumentChunker<string>
{
    public SemanticChunker(
        IEmbeddingGenerator<string, Embedding<float>> embeddingGenerator,
        ChunkerOptions options,
        float? thresholdPercentile = default);
}

// Splits documents into chunks based on headers and their corresponding levels, preserving the header context.
public sealed class HeaderChunker : DocumentChunker<string>
{
    public HeaderChunker(ChunkerOptions options);
}

// Treats each section in a Document as a separate entity.
public sealed class SectionChunker : DocumentChunker<string>
{
    public SectionChunker(ChunkerOptions options);
}

public abstract class ChunkProcessor<T>
{
    public abstract IAsyncEnumerable<IngestionChunk<T>> ProcessAsync(IAsyncEnumerable<IngestionChunk<T>> chunks, CancellationToken cancellationToken = default);
}

// Enriches chunks with summary text using an AI chat model.
public sealed class SummaryEnricher : ChunkProcessor<string>
{
    public SummaryEnricher(IChatClient chatClient, ChatOptions? chatOptions = null, int? maxWordCount = default);
    
    public static string MetadataKey => "summary";
}

// Enriches chunks with sentiment analysis using an AI chat model.
public sealed class SentimentEnricher : ChunkProcessor<string>
{
    public SentimentEnricher(IChatClient chatClient, ChatOptions? chatOptions = null, double? confidenceThreshold = default);
    
    public static string MetadataKey => "sentiment";
}

// Enriches document chunks with a classification label based on their content.
public sealed class ClassificationEnricher : ChunkProcessor<string>
{
    public ClassificationEnricher(IChatClient chatClient, string[] predefinedClasses,
        ChatOptions? chatOptions = null, string? fallbackClass = null);

    public ClassificationEnricher(IChatClient chatClient, ReadOnlySpan<string> predefinedClasses,
        ChatOptions? chatOptions = null, string? fallbackClass = null);
        
    public static string MetadataKey => "classification";
}

// Enriches chunks with keyword extraction using an AI chat model.
public sealed class KeywordEnricher : ChunkProcessor<string>
{
    // predefinedKeywords needs to be provided in explicit way, so the user is encouraged to think about it.
    // And for example provide a closed set, so the results are more predictable.
    public KeywordEnricher(IChatClient chatClient, string[]? predefinedKeywords,
        ChatOptions? chatOptions = null, int? maxKeywords = default, double? confidenceThreshold = default);

    public KeywordEnricher(IChatClient chatClient, ReadOnlySpan<string> predefinedKeywords,
        ChatOptions? chatOptions = null, int? maxKeywords = default, double? confidenceThreshold = default);

    public static string MetadataKey => "keywords";
}

public abstract class IngestionChunkWriter<T> : IDisposable
{
    public abstract Task WriteAsync(IAsyncEnumerable<IngestionChunk<T>> chunks, CancellationToken cancellationToken = default);

    public void Dispose();
    protected virtual void Dispose(bool disposing);
}

public sealed class VectorStoreWriterOptions
{
    public string CollectionName { get; set; } = "chunks";

    // The distance function to use when creating the collection. When not provided, the default specific to given database will be used.
    public string? DistanceFunction { get; set; }

    // When enabled, the writer will delete the chunks for given document before inserting the new ones.
    // So the ingestion will "replace" the existing chunks for the document with the new ones.
    public bool IncrementalIngestion { get; set; } = false;
}

public sealed class VectorStoreWriter<T> : IngestionChunkWriter<T>
{
    public VectorStoreWriter(VectorStore vectorStore, int dimensionCount, VectorStoreWriterOptions? options = default);

    public VectorStoreCollection<object, Dictionary<string, object?>> VectorStoreCollection { get; }
}

public sealed class IngestionPipelineOptions
{
    public string ActivitySourceName { get; set; } = "Experimental.Microsoft.Extensions.DataIngestion";
    
    // In the future, we might extend it with sth like this:
    // public int MaxDegreeOfParallelism { get; set; } = Environment.ProcessorCount;
}

public sealed class IngestionPipeline<T> : IDisposable
{
    public IngestionPipeline(
        IngestionDocumentReader reader,
        IngestionChunker<T> chunker,
        IngestionChunkWriter<T> writer,
        IngestionPipelineOptions? options = default,
        ILoggerFactory? loggerFactory = default);

    public IList<IngestionDocumentProcessor> DocumentProcessors { get; }
    public IList<IngestionChunkProcessor<T>> ChunkProcessors { get; }

    public async Task ProcessAsync(DirectoryInfo directory, string searchPattern = "*.*", SearchOption searchOption = SearchOption.TopDirectoryOnly, CancellationToken cancellationToken = default);
    public async Task ProcessAsync(IEnumerable<FileInfo> files, CancellationToken cancellationToken = default);
    public void Dispose();
}
```
## Kubernetes specialized Resource Monitoring

**NeedsWork** | [#extensions/6496](https://github.com/dotnet/extensions/issues/6496#issuecomment-3428799239) | [Video](https://www.youtube.com/watch?v=Gzuo8S2H-bM&t=1h6m50s)


* Changed AddKubernetesResourceMonitoring environmentVariablePrefix to be nullable, default null.
* KubernetesResourceQuotaServiceCollectionsExtensions => KubernetesResourceQuotaServiceCollectionExtensions (removed the "s" on Collection(s))
* `LimitsMemory` sounds like a boolean, and does not convey a unit; it needs a better name.  (e.g. `MemoryLimitInBytes`)
    * The other names also need improvement, but we couldn't come up with names during the meeting.
* ResourceQuota and IResourceQuotaProvider should go into the existing package, not create a new package for these two types
* Changed IResourceQuotaProvider to abstract ResourceQuotaProvider

```csharp
namespace Microsoft.Extensions.Diagnostics.ResourceMonitoring
{
    public abstract class ResourceQuotaProvider
    {
        public abstract ResourceQuota GetResourceQuota();
    }

    public sealed class ResourceQuota
    {
        public ulong LimitsMemory { get; set; }
        public double LimitsCpu { get; set; }
        public ulong RequestsMemory { get; set; }
        public double RequestsCpu { get; set; }
    }
}

namespace Microsoft.Extensions.DependencyInjection
{
    public static class KubernetesResourceQuotaServiceCollectionExtensions
    {
        public static IServiceCollection AddKubernetesResourceMonitoring(
            this IServiceCollection services,
            string? environmentVariablePrefix = null);
    }
}
```
## Public abstract HttpDependencyMetadataResolver for custom request classification strategies

**Approved** | [#extensions/6840](https://github.com/dotnet/extensions/issues/6840#issuecomment-3428907729) | [Video](https://www.youtube.com/watch?v=Gzuo8S2H-bM&t=1h36m35s)

* We removed the conditional compilation. We decided to keep only the HttpRequestMessage
* Consider the template method pattern in lieu of `public virtual`

```csharp
namespace Microsoft.Extensions.Http.Diagnostics;

public abstract class HttpDependencyMetadataResolver
{
    public virtual RequestMetadata? GetRequestMetadata(HttpRequestMessage requestMessage);
}
```

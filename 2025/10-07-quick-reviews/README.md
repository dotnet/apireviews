# API Review 10/07/2025

## DataIngestion: Document representation

**Approved** | [#extensions/6893](https://github.com/dotnet/extensions/issues/6893#issuecomment-3378036294)

* `Markdown { get; }` => `GetMarkdown()` because it isn't O(1) (DocumentSection does a string.Concat)
* Added `bool HasMetadata { get; }` to allow avoiding instantiating the dictionary to find out it's empty
* The types are very generally named, but very scoped in purpose.  To fix it, we'll prepend all type names with `Ingestion`
* The table.Cells property changed from string to IngestionDocumentElement
* Removed Document.Markdown as unnecessary/redundant/confusing.
* Changed Document from being IEnumerable to having an EnumerateContent method, both for discoverability and to have a place to explain that DocumentSection values are not (yield) returned.
  * It's felt that EnumerateContent isn't needed on DocumentSection at this time.

```csharp
namespace Microsoft.Extensions.DataIngestion;

public abstract class IngestionDocumentElement
{
    protected internal IngestionDocumentElement(string markdown);

    protected internal IngestionDocumentElement(); // ctor used by Section where providing Markdown up-front is not mandatory
 
    public virtual string GetMarkdown();

    public string? Text { get; set; }

    public int? PageNumber { get; set; }

    public bool HasMetadata { get; }
    public Dictionary<string, object?> Metadata { get; }
}

public sealed class IngestionDocumentSection : IngestionDocumentElement
{
    public IngestionDocumentSection(string markdown) : base(markdown);

    public IngestionDocumentSection() : base();

    public List<IngestionDocumentElement> Elements { get; };

    public override string GetMarkdown();
}

public sealed class IngestionDocumentParagraph : IngestionDocumentElement
{
    public IngestionDocumentParagraph(string markdown) : base(markdown);
}

public sealed class IngestionDocumentHeader : IngestionDocumentElement
{
    public IngestionDocumentHeader(string markdown) : base(markdown);

    public int? Level { get; set; }
}

public sealed class IngestionDocumentFooter : IngestionDocumentElement
{
    public IngestionDocumentFooter(string markdown) : base(markdown);
}

public sealed class IngestionDocumentTable : IngestionDocumentElement
{
    public IngestionDocumentTable(string markdown, IngestionDocumentElement[,] cells) : base(markdown);

    // This information is useful when chunking large tables that exceed token count limit.
    public IngestionDocumentElement[,] Cells { get; }
}

public sealed class IngestionDocumentImage : IngestionDocumentElement
{
    public IngestionDocumentImage(string markdown) : base(markdown);

    public ReadOnlyMemory<byte>? Content { get; set; }

    public string? MediaType { get; set; }

    public string? AlternativeText { get; set; }
}

public sealed class IngestionDocument
{
    public IngestionDocument(string identifier);
    
    public string Identifier { get; }

    public List<DocumentSection> Sections { get; }

    public IEnumerable<IngestionDocumentElement> EnumerateContent();
}
```
## DataIngestion: Pipelines

**NeedsWork** | [#extensions/6895](https://github.com/dotnet/extensions/issues/6895#issuecomment-3378339836)

* Changed `string filePath` to `FileInfo source` because of file/URI semantic distinction needs.
* Added Ingestion prefix to be consistent with the other half of the proposal
* Changed the DocumentReader Uri-based ctors to a (Stream, mediaType) ctor, and made the file one virtual.
  * URI fetching comes with a lot of policy that can't be expressed, such as certificate revocation, or custom trust lists.
* interface IDocumentProcessor => abstract class IngestionDocumentProcessor
* AlternativeTextEnricher => ImageAlternativeTextEnricher, to help convey it is for img.alt, not a general summarizer.
* DocumentChunk => IngestionChunk
* While discussing the name of RemovalProcessor, we mused on whether these types should be encapsulated as methods, similar to the SelectEnumerable from Linq is Enumerable.Select

```csharp
namespace Microsoft.Extensions.DataIngestion;

// DocumentReader is an abstraction for reading Documents from file paths and/or URIs.
// Streams are not supported, as some services require information not provided by the Stream like Content Type.
public abstract class IngestionDocumentReader
{
    public Task<IngestionDocument> ReadAsync(FileInfo source, CancellationToken cancellationToken = default)
        => ReadAsync(source, source.Name, cancellationToken);

    public virtual Task<IngestionDocument> ReadAsync(FileInfo source, string identifier, string? mediaType = null, CancellationToken cancellationToken = default);

    public abstract Task<IngestionDocument> ReadAsync(Stream source, string mediaType, string identifier, CancellationToken cancellationToken = default);
}

public abstract class IngestionDocumentProcessor
{
    Task<IngestionDocument> ProcessAsync(IngestionDocument document, CancellationToken cancellationToken = default);
}

public sealed class ImageAlternativeTextEnricher : IngestionDocumentProcessor
{
    public ImageAlternativeTextEnricher(IChatClient chatClient, ChatOptions? chatOptions = null);
}

// Represents a processor that removes specific elements from a document based on a provided predicate.
public sealed class RemovalProcessor : IngestionDocumentProcessor
{
    public static RemovalProcessor Footers { get; }
    public static RemovalProcessor EmptySections { get; }
    
    public RemovalProcessor(Predicate<DocumentElement> filter);
}

public sealed class IngestionChunk
{
    public string Content { get; }
    public IngestionDocument Document { get; }
    public int? TokenCount { get; }
    public string? Context { get; }
    public Dictionary<string, object> Metadata { get; }

    public DocumentChunk(string content, Document document, int? tokenCount = null, string? context = null);
}

public interface IDocumentChunker
{
    Task<List<IngestionChunk>> ProcessAsync(IngestionDocument document, CancellationToken cancellationToken = default);
}

// the class is not sealed on purpose, so creating a derived type specific for given chunker is possible
public class ChunkerOptions
{
    // Default values come from https://learn.microsoft.com/en-us/azure/search/vector-search-how-to-chunk-documents#text-split-skill-example

    /// <summary>
    /// The maximum number of tokens allowed in each chunk. Default is 2000.
    /// </summary>
    public int MaxTokensPerChunk { get; set; } = 2_000;
    
    /// <summary>
    /// The number of overlapping tokens between consecutive chunks. Default is 500.
    /// </summary>
    public int OverlapTokens { get; set; } = 500;
}

// Splits a Document into chunks based on semantic similarity between its elements.
public sealed class SemanticSimilarityChunker : IDocumentChunker
{
    public SemanticChunker(
        IEmbeddingGenerator<string, Embedding<float>> embeddingGenerator,
        Tokenizer tokenizer, ChunkerOptions? options = default,
        float thresholdPercentile = 95.0f);
}

// Splits documents into chunks based on headers and their corresponding levels, preserving the header context.
public sealed class HeaderChunker : IDocumentChunker
{
    public HeaderChunker(Tokenizer tokenizer, ChunkerOptions? options = default);
}

// Treats each section in a Document as a separate entity.
public sealed class SectionChunker : IDocumentChunker
{
    public SectionChunker(Tokenizer tokenizer, ChunkerOptions? options = default);
}

public interface IChunkProcessor
{
    Task<List<DocumentChunk>> ProcessAsync(List<DocumentChunk> chunks, CancellationToken cancellationToken = default);
}

// Enriches chunks with summary text using an AI chat model.
public sealed class SummaryEnricher : IChunkProcessor
{
    public SummaryEnricher(IChatClient chatClient, ChatOptions? chatOptions = null, int maxWordCount = 100);
    
    public static string MetadataKey => "summary";
}

// Enriches chunks with sentiment analysis using an AI chat model.
public sealed class SentimentEnricher : IChunkProcessor
{
    public SentimentEnricher(IChatClient chatClient, ChatOptions? chatOptions = null, double confidenceThreshold = 0.7);
    
    public static string MetadataKey => "sentiment";
}

// Enriches document chunks with a classification label based on their content.
public class ClassificationEnricher : IChunkProcessor
{
    public ClassificationEnricher(IChatClient chatClient, string[] predefinedClasses,
        ChatOptions? chatOptions = null, string fallbackClass = "Unknown");
        
    public static string MetadataKey => "classification";
}

// Enriches chunks with keyword extraction using an AI chat model.
public sealed class KeywordEnricher : IChunkProcessor
{
    // predefinedKeywords needs to be provided in explicit way, so the user is encouraged to think about it.
    // And for example provide a closed set, so the results are more predictable.
    public KeywordEnricher(IChatClient chatClient, string[]? predefinedKeywords,
        ChatOptions? chatOptions = null, int maxKeywords = 5, double confidenceThreshold = 0.7)

    public static string MetadataKey => "keywords";
}

public interface IDocumentChunkWriter : IDisposable
{
    Task WriteAsync(IReadOnlyList<DocumentChunk> chunks, CancellationToken cancellationToken = default);
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

public sealed class VectorStoreWriter : IDocumentChunkWriter
{
    public VectorStoreWriter(VectorStore vectorStore, int dimensionCount, VectorStoreWriterOptions? options = default);

    public VectorStoreCollection<object, Dictionary<string, object?>> VectorStoreCollection { get; }
}

public interface IDocumentPipeline : IDisposable
{
    Task ProcessAsync(DirectoryInfo directory, string searchPattern = "*.*", SearchOption searchOption = SearchOption.TopDirectoryOnly, CancellationToken cancellationToken = default);
    Task ProcessAsync(IEnumerable<string> filePaths, CancellationToken cancellationToken = default);
    Task ProcessAsync(IEnumerable<Uri> sources, CancellationToken cancellationToken = default);
}

public sealed class DocumentPipeline : IDocumentPipeline
{
    public DocumentPipeline(
        DocumentReader reader,
        IReadOnlyList<IDocumentProcessor> documentProcessors,
        IDocumentChunker chunker,
        IReadOnlyList<IChunkProcessor> chunkProcessors,
        IDocumentChunkWriter writer,
        ILoggerFactory? loggerFactory = default,
        string? sourceName = default);
}
```

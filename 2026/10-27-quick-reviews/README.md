# API Review 10/27/2026

## IDocumentExtractionClient — a document-extraction capability as its own peer library (Microsoft.Extensions.DocumentExtraction)

**NeedsWork** | [#extensions/7587](https://github.com/dotnet/extensions/issues/7587#issuecomment-6451085323) | [Video](https://www.youtube.com/watch?v=fUnxoxt8rJ4&t=0h0m0s)


* We try to reserve single-word names for the System namespace.  So `Document` alone is a concern.  Perhaps `ExtractedDocument`? `IngestionDocument`? `DataIngestionDocument`?
* Rather than exposing the extracted content as `string Markdown`, consider changing to the `AIContent` type, a parallelish type tree, or something like `string Data` and a string MIME type
  * That may have impact on DocumentSection, e.g. what do you use for an AIContent of a table?  Perhaps make the content property on the DocumentElement be nullable.
  * The proposal having the `string Markdown` property has an ultimately N-squared memory consumption.  Consider a document that is just an H1 with an H2 with an H3 with an H4, ...; every nesting has the text of all of the children, so the innermost H is repeated N times.
* IList doesn't seem warranted (not a private type, no runtime variance, not settable properties), so just be List.
  * Same for IDictionary
* Since Document is sealed, and is just data, IDisposable doesn't seem warranted.
* The Options.`ModelId` property seems biased toward LLM-selection, but the described scenarios included non-LLM usages.  Consider a less domain-based name, like `ExtractorName`
* The type name being ExtractedDocument and the options being DocumentExtractionOptions was fine for some people, and rubbed others the wrong way.  So it's a thing to think about.
* Standard caveat about interfaces for IDocumentExtractionClient.  E.g. as an interface it's hard to add support for Pipes in parallel with Streams.
* DocumentSourcePage.pageNumber doesn't seem to make sense for things like OneNote "Pages", consider nullable.
* Consider whether you actually want public `record` types (we recommend against).

```csharp
namespace Microsoft.Extensions.Documents
{
    public abstract class ExtractedDocumentElement
    {
        protected ExtractedDocumentElement(string markdown);
        public List<ExtractedDocumentSourceReference> Sources { get; }
        // See notes re: markdown vs AIContent
        public string Markdown { get; set; }
        public virtual string ToMarkdown();
    }

    public sealed class ExtractedDocumentSection : DocumentElement
    {
        public ExtractedDocumentSection(string markdown);
        public List<ExtractedDocumentElement> Elements { get; }
    }

    public sealed class ExtractedDocument
    {
        public ExtractedDocument(string identifier, string? markdown = null);
        public string Identifier { get; }
        public List<ExtractedDocumentSection> Sections { get; }
    }
}

namespace Microsoft.Extensions.DocumentExtraction
{
    public sealed class DocumentExtractionOptions
    {
        public DocumentExtractionOptions();
        public string? ModelId { get; set; }
        public bool CaptureRawContent { get; set; }
        public IDictionary<string, object?> AdditionalProperties { get; }
        public DocumentExtractionOptions Clone();
    }

    public sealed class DocumentExtractionPage
    {
        public DocumentExtractionPage(
            DocumentSourcePage sourcePage, DocumentSection content);
        public DocumentSourcePage SourcePage { get; }
        public DocumentSection Content { get; }
    }
}
```

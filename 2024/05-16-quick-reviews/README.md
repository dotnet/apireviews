# API Review 05/16/2024

## Tokenizers Library Design

**NeedsWork** | [#machinelearning/7144](https://github.com/dotnet/machinelearning/issues/7144#issuecomment-2115988535)

* Let's exclude `MapTokenToId`
    - It seems confusing that some tokens will return `null` for cases where one token is mapped to more than one ID
    - If this is needed, they can call `EncodeToIds`
* Let's move the factories as `Create`/`CreateAsync` to the concrete types
    - For example `Tokenizer.CreateTiktokenAsync()` would become `TiktokenTokenizer.CreateAsync()`
* We should avoid single word types if the word is very generic
    - `Token` -> `EncodedToken`
    - We could also go with `TextToken`/`TextTokenizer`
* `Token`
    - Let's downlevel `Index` and `Range` and use those
* The lower case / upper case normalizers should have a a singleton instance
* The tokenizers should be suffixed with `Tokenizer` for consistency and discoverability
* `Bpe`    
    - Consider async factories as this does IO
    - Consider having a simple overload that only takes the required members
    - Consider returning `FrozenDictionary<,>`
* `Tiktoken`
    - Let's omit `Encoder` and `Decoder` as it's not clear how someone would use it
* `Vocab` -> `Vocabulary`

```C#
namespace Microsoft.ML.Tokenizers;

public struct EncodeResults<T>
{
    public IReadOnlyList<T> Tokens { get; set; }
    public string? NormalizedText { get; set; }
}

public struct EncodeSettings
{
    public bool ConsiderNormalization { get; set; }
    public bool ConsiderPreTokenization { get; set; }
    public bool ProduceNormalizedString { get; set; }
    public int MaxTokenCount { get; set; }
}

public abstract partial class Tokenizer
{
    protected Tokenizer();

    public virtual Normalizer? Normalizer { get; }

    public virtual PreTokenizer? PreTokenizer { get; }

    public IReadOnlyList<int> EncodeToIds(string text, bool considerPreTokenization = true, bool considerNormalization = true);
    public IReadOnlyList<int> EncodeToIds(ReadOnlySpan<char> text, bool considerPreTokenization = true, bool considerNormalization = true);

    public IReadOnlyList<int> EncodeToIds(string text, int maxTokenCount, out string? normalizedText, out int textLength, bool considerPreTokenization = true, bool considerNormalization = true);
    public IReadOnlyList<int> EncodeToIds(ReadOnlySpan<char> text, int maxTokenCount, out string? normalizedText, out int textLength, bool considerPreTokenization = true, bool considerNormalization = true);

    protected abstract EncodeResults<int> EncodeToIds(string? text, ReadOnlySpan<char> textSpan, EncodeSettings settings);

    public IReadOnlyList<Token> EncodeToTokens(string text, out string? normalizedString, bool considerPreTokenization = true, bool considerNormalization = true);
    public IReadOnlyList<Token> EncodeToTokens(ReadOnlySpan<char> text, out string? normalizedString, bool considerPreTokenization = true, bool considerNormalization = true);

    protected abstract EncodeResults<Token> EncodeToTokens(string? text, ReadOnlySpan<char> textSpan, EncodeSettings settings);

    public int CountTokens(string text, bool considerPreTokenization = true, bool considerNormalization = true);
    public int CountTokens(ReadOnlySpan<char> text, bool considerPreTokenization = true, bool considerNormalization = true);

    protected abstract int CountTokens(string? text, ReadOnlySpan<char> textSpan, EncodeSettings settings);

    public int GetIndexByTokenCount(string text, int maxTokenCount, out string? normalizedString, out int tokenCount, bool considerPreTokenization = true, bool considerNormalization = true);
    public int GetIndexByTokenCount(ReadOnlySpan<char> text, int maxTokenCount, out string? normalizedString, out int tokenCount, bool considerPreTokenization = true, bool considerNormalization = true);

    public int GetIndexByTokenCountFromEnd(string text, int maxTokenCount, out string? normalizedString, out int tokenCount, bool considerPreTokenization = true, bool considerNormalization = true);
    public int GetIndexByTokenCountFromEnd(ReadOnlySpan<char> text, int maxTokenCount, out string? normalizedString, out int tokenCount, bool considerPreTokenization = true, bool considerNormalization = true);

    protected abstract int GetIndexByTokenCount(string? text, ReadOnlySpan<char> textSpan, EncodeSettings settings, bool fromEnd, out string? normalizedString, out int tokenCount);

    public abstract string? Decode(IEnumerable<int> ids);
    public abstract OperationStatus Decode(IEnumerable<int> ids, Span<char> destination, out int idsConsumed, out int charsWritten);
}

public abstract partial class Normalizer
{
    protected Normalizer();

    public string Normalize(string text);
    public string Normalize(ReadOnlySpan<char> text);

    protected abstract string Normalize(string? text, ReadOnlySpan<char> textSpan);
}

public abstract partial class PreTokenizer
{
    protected PreTokenizer();

    public IEnumerable<Range> PreTokenize(string text);
    public IEnumerable<Range> PreTokenize(ReadOnlySpan<char> text);

    protected abstract IEnumerable<Range> PreTokenize(string? text, ReadOnlySpan<char> textSpan);
}

public readonly struct EncodedToken
{
    public Token(int id, string value, Range range);
    public int Id { get; }
    public Range Range { get; }
    public string Value { get; }
}

public sealed partial class LowerCaseNormalizer : Normalizer
{
    public static LowerCaseNormalizer Instance { get; }
    public override string Normalize(ReadOnlySpan<char> original);
    public override string Normalize(string original);
}

public sealed partial class UpperCaseNormalizer : Normalizer
{
    public static UpperCaseNormalizer Instance { get; }
    public override string Normalize(ReadOnlySpan<char> original);
    public override string Normalize(string original);
}

public sealed partial class SentencePieceNormalizer : Normalizer
{
    public SentencePieceNormalizer(bool removeExtraWhitespace, bool addDummyPrefix, bool escapeWhitespace, bool treatWhitespaceAsSuffix);
    public bool AddDummyPrefix { get; }
    public bool EscapeWhitespace { get; }
    public bool RemoveExtraWhitespace { get; }
    public bool TreatWhitespaceAsSuffix { get; }
    public override string Normalize(ReadOnlySpan<char> original);
    public override string Normalize(string original);
}

public sealed partial class TiktokenPreTokenizer : PreTokenizer
{
    public static TiktokenPreTokenizer Create(Regex regex, IReadOnlyDictionary<string, int> specialTokensEncoder);
}

public sealed partial class WhitespacePreTokenizer : PreTokenizer
{
    public static Whitespace Instance { get; }
}

public sealed partial class RobertaPreTokenizer : PreTokenizer
{
    public static RobertaPreTokenizer Instance { get; }
}

public sealed partial class BpeTokenizer : Tokenizer
{
    public static Create(string vocabFile, string? mergesFile, PreTokenizer? preTokenizer = null, Normalizer? normalizer = null, string? unknownToken = null, 
                         string? continuingSubwordPrefix = null, string? endOfWordSuffix = null, bool? fuseUnknownTokens = false);

    public static Create(Stream vocabStream, Stream? mergesStream, PreTokenizer? preTokenizer = null, Normalizer? normalizer = null, string? unknownToken = null, 
                         string? continuingSubwordPrefix = null, string? endOfWordSuffix = null, bool? fuseUnknownTokens = false);

    public string? ContinuingSubwordPrefix { get; }
    public string? EndOfWordSuffix { get; }
    public bool? FuseUnknownTokens { get; }
    public string? UnknownToken { get; }

    public IReadOnlyDictionary<string, int> Vocab { get; }

    public string? Decode(IEnumerable<int> ids, bool considerSpecialTokens);
}

public sealed partial class TiktokenTokenizer : Tokenizer
{
    public static TiktokenTokenizer Create(string vocabFilePath, PreTokenizer? preTokenizer, IReadOnlyDictionary<string, int> specialTokens = null, Normalizer? normalizer = null, int? cacheSize = 8192);
    public static TiktokenTokenizer Create(Stream vocabStream, PreTokenizer? preTokenizer, IReadOnlyDictionary<string, int> specialTokens = null, Normalizer? normalizer = null, int? cacheSize = 8192);

    // public IReadOnlyDictionary<int, ReadOnlyMemory<byte>> Decoder { get; }
    // public IReadOnlyDictionary<ReadOnlyMemory<byte>, int> Encoder { get; }
    public IReadOnlyDictionary<string, int> SpecialTokens { get; }
    public IReadOnlyDictionary<string, int> Vocab { get; }
}
```

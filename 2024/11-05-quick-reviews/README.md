# API Review 11/05/2024

## Fix default HTTP resilience not to retry non-idempotent requests (POST, PUT, etc.)

**Approved** | [#extensions/5248](https://github.com/dotnet/extensions/issues/5248#issuecomment-2457909322)

* Looks good as proposed
* The template team needs to sign off too, we believe this needs to be in the template to be useful.
* We discussed whether changing the default would be viable but it seems we don't believe that to be possible.
* We might want to be make them chainable.

```C#
namespace Microsoft.Extensions.Http.Resilience;

public static class HttpRetryStrategyOptionsExtensions
{
    public static void DisableForUnsafeHttpMethods(this HttpRetryStrategyOptions option);
    public static void DisableFor(this HttpRetryStrategyOptions options, params HttpMethod[] methods);
}
```
## WordPiece and Bert Tokenizer Design review

**NeedsWork** | [#machinelearning/7281](https://github.com/dotnet/machinelearning/issues/7281#issuecomment-2458046828)

* We should consider option types to avoid having to add multiple overloads whenever we add a new parameter

### WordPieceTokenizer

```C#
namespace Microsoft.ML.Tokenizers
{
    public partial class WordPieceTokenizer : Tokenizer
    {
        public static WordPieceTokenizer Create(
            string vocabFilePath,
            PreTokenizer? preTokenizer = null,
            Normalizer? normalizer = null,
            IReadOnlyDictionary<string, int>? specialTokens = null,
            string unknownToken = "[UNK]",
            string continuingSubwordPrefix = DefaultContinuingSubwordPrefix,
            int maxInputCharsPerWord = DefaultMaxInputCharsPerWord
        );

        public static WordPieceTokenizer Create(
            Stream vocabStream,
            PreTokenizer? preTokenizer = null,
            Normalizer? normalizer = null,
            IReadOnlyDictionary<string, int>? specialTokens = null,
            string unknownToken = "[UNK]",
            string continuingSubwordPrefix = DefaultContinuingSubwordPrefix,
            int maxInputCharsPerWord = DefaultMaxInputCharsPerWord
        );

        public static async Task<WordPieceTokenizer> CreateAsync(
            Stream vocabStream,
            PreTokenizer? preTokenizer = null,
            Normalizer? normalizer = null,
            IReadOnlyDictionary<string, int>? specialTokens = null,
            string unknownToken = "[UNK]",
            string continuingSubwordPrefix = DefaultContinuingSubwordPrefix,
            int maxInputCharsPerWord = DefaultMaxInputCharsPerWord,
            CancellationToken cancellationToken = default
        );

        public string UnknownToken { get; }
        public int UnknownTokenId { get; }
        public string ContinuingSubwordPrefix { get; }
        public int MaxInputCharsPerWord { get; }

        public IReadOnlyDictionary<string, int>? SpecialTokens { get; }
        public string Decode(IEnumerable<int> ids, bool skipSpecialTokens);

        public OperationStatus Decode(
            IEnumerable<int> ids,
            Span<char> destination,
            bool skipSpecialTokens, 
            out int idsConsumed,
            out int charsWritten
        );
    }
}
```

### BertTokenizer

* We should spell out `sep`, `pad`, `cls`
* We should replace `tokenizeChineseChars` to something more accurate, such as `tokenizeCjkChars`
* Is `stripAccents` correct? Discussion seems to indicate that we might strip more. Also, replace `strip` with `remove`.
* We should remove the `do` prefix
    - `doLowerCase` -> `convertToLowerCase`
    - `doBasicTokenization` -> `applyBasicTokenization`
* `BuildInputsWithSpecialTokens` / `GetSpecialTokensMask` / `CreateTokenTypeIdsFromSequences`
    - `tokenIds0` -> `tokenIds`
    - `tokenIds1` -> `additionalTokenIds`
    - `buffer` -> `destination`
    - `written` -> `valuesWritten`

```C#
namespace Microsoft.ML.Tokenizers;

public sealed partial class BertTokenizer : WordPieceTokenizer
{
    public static BertTokenizer Create(
        string vocabFilePath,
        bool doLowerCase = true,
        bool doBasicTokenization = true,
        bool splitOnSpecialTokens = true,
        string unknownToken = "[UNK]",
        string sepToken = "[SEP]",
        string padToken = "[PAD]",
        string clsToken = "[CLS]",
        string maskToken = "[MASK]",
        bool tokenizeChineseChars = true,
        bool stripAccents = false
    );  

    public static BertTokenizer Create(
        Stream vocabStream,
        bool doLowerCase = true,
        bool doBasicTokenization = true,
        bool splitOnSpecialTokens = true,
        string unknownToken = "[UNK]",
        string sepToken = "[SEP]",
        string padToken = "[PAD]",
        string clsToken = "[CLS]",
        string maskToken = "[MASK]",
        bool tokenizeChineseChars = true,
        bool stripAccents = false
    );

    public static async Task<BertTokenizer> CreateAsync(
        Stream vocabStream,
        bool doLowerCase = true,
        bool doBasicTokenization = true,
        bool splitOnSpecialTokens = true,
        string unknownToken = "[UNK]",
        string sepToken = "[SEP]",
        string padToken = "[PAD]",
        string clsToken = "[CLS]",
        string maskToken = "[MASK]",
        bool tokenizeChineseChars = true,
        bool stripAccents = false
    );

    public bool DoLowerCase { get; }
    public bool DoBasicTokenization { get; }
    public bool SplitOnSpecialTokens { get; }
    public string SepToken { get; }
    public int SepTokenId { get; }
    public string PadToken { get; }
    public int PadTokenId { get; }
    public string ClsToken { get; }
    public int ClsTokenId { get; }
    public string MaskToken { get; }
    public int MaskTokenId { get; }
    public bool TokenizeChineseChars { get; }
    public bool StripAccents { get; }

    public IReadOnlyList<int> EncodeToIds(
        string text,
        bool addSpecialTokens, 
        bool considerPreTokenization = true,
        bool considerNormalization = true
    );

    public IReadOnlyList<int> EncodeToIds(
        ReadOnlySpan<char> text,
        bool addSpecialTokens, 
        bool considerPreTokenization = true,
        bool considerNormalization = true
    );

    public IReadOnlyList<int> EncodeToIds(
        string text,
        int maxTokenCount,
        bool addSpecialTokens,
        out string? normalizedText,        
        out int charsConsumed,
        bool considerPreTokenization = true,
        bool considerNormalization = true
    );

    public IReadOnlyList<int> EncodeToIds(
        ReadOnlySpan<char> text,
        int maxTokenCount,
        bool addSpecialTokens,        
        out string? normalizedText,
        out int charsConsumed,
        bool considerPreTokenization = true,
        bool considerNormalization = true
    );

    public IReadOnlyList<int> BuildInputsWithSpecialTokens(
        IEnumerable<int> tokenIds0,
        IEnumerable<int>? tokenIds1 = null
    );

    public OperationStatus BuildInputsWithSpecialTokens(
        IEnumerable<int> tokenIds0,
        Span<int> buffer,
        out int written,        
        IEnumerable<int>? tokenIds1 = null
    );

    public IReadOnlyList<int> GetSpecialTokensMask(
        IEnumerable<int> tokenIds0,
        IEnumerable<int>? tokenIds1 = null, 
        bool alreadyHasSpecialTokens = false
    );

    public OperationStatus GetSpecialTokensMask(
        IEnumerable<int> tokenIds0,
        Span<int> buffer,
        out int written,        
        IEnumerable<int>? tokenIds1 = null,
        bool alreadyHasSpecialTokens = false
    );

    public IReadOnlyList<int> CreateTokenTypeIdsFromSequences(
        IEnumerable<int> tokenIds0,
        IEnumerable<int>? tokenIds1 = null
    );

    public OperationStatus CreateTokenTypeIdsFromSequences(
        IEnumerable<int> tokenIds0,
        Span<int> buffer,
        out int written,        
        IEnumerable<int>? tokenIds1 = null
    );
}
```

### PreTokenizer Factory methods

* Let's leave off the `PreTokenizer` suffix
* `CreateWhiteSpaceOrPunctuation` -> `CreateWordOrPunctuation`
* `specialTokensEncoder` -> `specialTokens`

```C#
namespace Microsoft.ML.Tokenizers;

public partial class PreTokenizer
{
    public static PreTokenizer CreateWordOrPunctuation(IReadOnlyDictionary<string, int>? specialTokens = null);
    public static PreTokenizer CreateWordOrNonWord(IReadOnlyDictionary<string, int>? specialTokens = null);
    public static PreTokenizer CreateWhitespace(IReadOnlyDictionary<string, int>? specialTokens = null);
}
```

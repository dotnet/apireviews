# API Review 09/30/2025

## Added Tokenizer's APIs for v2

**Approved** | [#machinelearning/7512](https://github.com/dotnet/machinelearning/issues/7512#issuecomment-3353250992) | [Video](https://www.youtube.com/watch?v=_-hBZhtIR7Q&t=0h0m0s)


* Changed BpeOptions..ctor's parameter from `IEnumerable<ValueTuple>` to `IEnumerable<KeyValuePair>`
* Added `BpeOptions(string vocabFile, string? mergesFile = null)`
* Fixed up some nullabilty errors.
* SentencePieceTokenizer.Create's `addBeginOfSentence` parameter => `addBeginningOfSentence`
* We had a lively discussion for the name of `CompositePreTokenizer`, and decided it's different from `Linked-`, `Delegating-`, et cetera, so `Composite-` was the locally best name.

```csharp

namespace Microsoft.ML.Tokenizers
{
    public sealed partial class BpeTokenizer : Tokenizer
    {
        public static BpeTokenizer Create(BpeOptions options);
         
        public bool ByteLevel { get; }
        public string? BeginningOfSentenceToken { get; }
    }

    public sealed class BpeOptions
    {
        public BpeOptions(IEnumerable<KeyValuePair<string, int>> vocabulary);
        public BpeOptions(string vocabFile, string? mergesFile = null);

        public string? BeginningOfSentenceToken { get; set; }
        public string? ContinuingSubwordPrefix { get; set; }
        public string? EndOfSentenceToken { get; set; }
        public string? EndOfWordSuffix { get; set; }
        public bool FuseUnknownTokens { get; set; }
        public IEnumerable<string>? Merges { get; set; }
        public Normalizer? Normalizer { get; set; }
        public PreTokenizer? PreTokenizer { get; set; }
        public IReadOnlyDictionary<string, int>? SpecialTokens { get; set; }
        public string? UnknownToken { get; set; }
        public IEnumerable<KeyValuePair<string, int>> Vocabulary { get; }
        public bool ByteLevel { get; set; }
    }

    public class SentencePieceTokenizer : Microsoft.ML.Tokenizers.Tokenizer
    {
        public static SentencePieceTokenizer Create(
            Stream modelStream,
            bool addBeginningOfSentence = true,
            bool addEndOfSentence = false,
            IReadOnlyDictionary<string, int>? specialTokens = null);
    }

    public class CompositePreTokenizer : PreTokenizer
    {
        public CompositePreTokenizer(
            IReadOnlyList<PreTokenizer> preTokenizers,
            IReadOnlyDictionary<string, int>? specialTokens = null);

        public IReadOnlyList<PreTokenizer> PreTokenizers { get; }
    }
}
```
## Bit manipulation method for BFloat16

**Approved** | [#runtime/119874](https://github.com/dotnet/runtime/issues/119874#issuecomment-3353285046) | [Video](https://www.youtube.com/watch?v=_-hBZhtIR7Q&t=0h51m41s)

* Corrected a pattern mismatch/typo: BFloat16BitsToInt16 => BFloat16ToInt16Bits
* We cut BinaryReader/BinaryWriter, pending further requests. (We didn't add Int128, no one asked for it)

```csharp
namespace System
{
    public partial class BitConverter
    {
        public short BFloat16ToInt16Bits(BFloat16 value);
        public ushort BFloat16ToUInt16Bits(BFloat16 value);
        public static BFloat16 Int16BitsToBFloat16(short value);
        public static BFloat16 UInt16BitsToBFloat16(ushort value);
    }
}

namespace System.Buffers.Binary
{
    public class BinaryPrimitives
    {
        public static void WriteBFloat16BigEndian(Span<byte> destination, BFloat16 value);
        public static void WriteBFloat16LittleEndian(Span<byte> destination, BFloat16 value);
        public static bool TryWriteBFloat16BigEndian(Span<byte> destination, BFloat16 value);
        public static bool TryWriteBFloat16LittleEndian(Span<byte> destination, BFloat16 value);
        public static void ReadBFloat16BigEndian(ReadOnlySpan<byte> source, out BFloat16 value);
        public static void ReadBFloat16LittleEndian(ReadOnlySpan<byte> source, out BFloat16 value);
        public static bool TryReadBFloat16BigEndian(ReadOnlySpan<byte> source, out BFloat16 value);
        public static bool TryReadBFloat16LittleEndian(ReadOnlySpan<byte> source, out BFloat16 value);
    }
}
```
## Add missing overloads to "Flow System.Text.Rune through more APIs" proposal

**Approved** | [#runtime/120015](https://github.com/dotnet/runtime/issues/120015#issuecomment-3353338775) | [Video](https://www.youtube.com/watch?v=_-hBZhtIR7Q&t=1h3m33s)

Looks good as proposed.

```csharp
namespace System
{
    public partial sealed class String
    {
        public int IndexOf(Rune value, int startIndex, StringComparison comparisonType);
        public int IndexOf(Rune value, int startIndex, int count, StringComparison comparisonType);
        public int IndexOf(char value, int startIndex, StringComparison comparisonType);
        public int IndexOf(char value, int startIndex, int count, StringComparison comparisonType);

        public int LastIndexOf(Rune value, int startIndex, StringComparison comparisonType);
        public int LastIndexOf(Rune value, int startIndex, int count, StringComparison comparisonType);
        public int LastIndexOf(char value, int startIndex, StringComparison comparisonType);
        public int LastIndexOf(char value, int startIndex, int count, StringComparison comparisonType);
    }

    public readonly struct Char
    {
        public bool Equals(char right, StringComparison comparisonType);
    }
}
```

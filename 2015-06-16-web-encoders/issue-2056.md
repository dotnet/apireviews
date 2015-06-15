```diff
---D:\git\corefx\bin\Windows_NT.AnyCPU.Debug\System.Text.Encodings.Web\System.Text.Encodings.Web.dll
   namespace System.Text.Encodings.Web {
  public sealed class CodePointFilter : ICodePointFilter {
    public CodePointFilter();
    public CodePointFilter(ICodePointFilter other);
    public CodePointFilter(params UnicodeRange[] allowedRanges);
    public CodePointFilter AllowCharacter(char character);
    public CodePointFilter AllowCharacters(params char[] characters);
    public CodePointFilter AllowCharacters(string characters);
    public CodePointFilter AllowFilter(ICodePointFilter filter);
    public CodePointFilter AllowRange(UnicodeRange range);
    public CodePointFilter AllowRanges(params UnicodeRange[] ranges);
    public CodePointFilter Clear();
    public CodePointFilter ForbidCharacter(char character);
    public CodePointFilter ForbidCharacters(params char[] characters);
    public CodePointFilter ForbidCharacters(string characters);
    public CodePointFilter ForbidRange(UnicodeRange range);
    public CodePointFilter ForbidRanges(params UnicodeRange[] ranges);
    public IEnumerable<int> GetAllowedCodePoints();
    public bool IsCharacterAllowed(char character);
  }
  public sealed class DefaultHtmlEncoder : HtmlEncoder {
    public DefaultHtmlEncoder(CodePointFilter filter);
    public DefaultHtmlEncoder(params UnicodeRange[] allowedRanges);
    public override int MaxOutputCharactersPerInputCharacter { get; }
    [MethodImpl(AggressiveInlining)]public override bool Encodes(int unicodeScalar);
    [MethodImpl(AggressiveInlining)]public unsafe override int FindFirstCharacterToEncode(char* text, int textLength);
    public unsafe override bool TryEncodeUnicodeScalar(int unicodeScalar, char* buffer, int bufferLength, out int numberOfCharactersWritten);
  }
  public sealed class DefaultJavaScriptEncoder : JavaScriptEncoder {
    public DefaultJavaScriptEncoder(CodePointFilter filter);
    public DefaultJavaScriptEncoder(params UnicodeRange[] allowedRanges);
    public override int MaxOutputCharactersPerInputCharacter { get; }
    [MethodImpl(AggressiveInlining)]public override bool Encodes(int unicodeScalar);
    [MethodImpl(AggressiveInlining)]public unsafe override int FindFirstCharacterToEncode(char* text, int textLength);
    public unsafe override bool TryEncodeUnicodeScalar(int unicodeScalar, char* buffer, int bufferLength, out int numberOfCharactersWritten);
  }
  public sealed class DefaultUrlEncoder : UrlEncoder {
    public DefaultUrlEncoder(CodePointFilter filter);
    public DefaultUrlEncoder(params UnicodeRange[] allowedRanges);
    public override int MaxOutputCharactersPerInputCharacter { get; }
    [MethodImpl(AggressiveInlining)]public override bool Encodes(int unicodeScalar);
    [MethodImpl(AggressiveInlining)]public unsafe override int FindFirstCharacterToEncode(char* text, int textLength);
    public unsafe override bool TryEncodeUnicodeScalar(int unicodeScalar, char* buffer, int bufferLength, out int numberOfCharactersWritten);
  }
  public abstract class HtmlEncoder : TextEncoder {
    protected HtmlEncoder();
    public static HtmlEncoder Default { get; }
    public static HtmlEncoder Create(CodePointFilter filter);
    public static HtmlEncoder Create(params UnicodeRange[] allowedRanges);
  }
  public interface ICodePointFilter {
    IEnumerable<int> GetAllowedCodePoints();
  }
  public abstract class JavaScriptEncoder : TextEncoder {
    protected JavaScriptEncoder();
    public static JavaScriptEncoder Default { get; }
    public static JavaScriptEncoder Create(CodePointFilter filter);
    public static JavaScriptEncoder Create(params UnicodeRange[] allowedRanges);
  }
  public abstract class TextEncoder {
    protected TextEncoder();
    public abstract int MaxOutputCharactersPerInputCharacter { get; }
    public abstract bool Encodes(int unicodeScalar);
    public unsafe abstract int FindFirstCharacterToEncode(char* text, int textLength);
    public unsafe abstract bool TryEncodeUnicodeScalar(int unicodeScalar, char* buffer, int bufferLength, out int numberOfCharactersWritten);
  }
  public static class TextEncoderExtensions {
    public static void Encode(this TextEncoder encoder, TextWriter output, char[] value, int startIndex, int characterCount);
    public static void Encode(this TextEncoder encoder, TextWriter output, string value);
    public static void Encode(this TextEncoder encoder, TextWriter output, string value, int startIndex, int characterCount);
    public static string Encode(this TextEncoder encoder, string value);
  }
  public abstract class UrlEncoder : TextEncoder {
    protected UrlEncoder();
    public static UrlEncoder Default { get; }
    public static UrlEncoder Create(CodePointFilter filter);
    public static UrlEncoder Create(params UnicodeRange[] allowedRanges);
  }
 }
 namespace System.Text.Unicode {
  public sealed class UnicodeRange {
    public UnicodeRange(int firstCodePoint, int length);
    public int FirstCodePoint { get; }
    public int Length { get; }
    public static UnicodeRange Create(char firstCharacter, char lastCharacter);
  }
  public static class UnicodeRanges {
    public static UnicodeRange All { get; }
    public static UnicodeRange AlphabeticPresentationForms { get; }
    public static UnicodeRange Arabic { get; }
    public static UnicodeRange ArabicExtendedA { get; }
    public static UnicodeRange ArabicPresentationFormsA { get; }
    public static UnicodeRange ArabicPresentationFormsB { get; }
    public static UnicodeRange ArabicSupplement { get; }
    public static UnicodeRange Armenian { get; }
    public static UnicodeRange Arrows { get; }
    public static UnicodeRange Balinese { get; }
    public static UnicodeRange Bamum { get; }
    public static UnicodeRange BasicLatin { get; }
    public static UnicodeRange Batak { get; }
    public static UnicodeRange Bengali { get; }
    public static UnicodeRange BlockElements { get; }
    public static UnicodeRange Bopomofo { get; }
    public static UnicodeRange BopomofoExtended { get; }
    public static UnicodeRange BoxDrawing { get; }
    public static UnicodeRange BraillePatterns { get; }
    public static UnicodeRange Buginese { get; }
    public static UnicodeRange Buhid { get; }
    public static UnicodeRange Cham { get; }
    public static UnicodeRange Cherokee { get; }
    public static UnicodeRange CjkCompatibility { get; }
    public static UnicodeRange CjkCompatibilityForms { get; }
    public static UnicodeRange CjkCompatibilityIdeographs { get; }
    public static UnicodeRange CjkRadicalsSupplement { get; }
    public static UnicodeRange CjkStrokes { get; }
    public static UnicodeRange CjkSymbolsandPunctuation { get; }
    public static UnicodeRange CjkUnifiedIdeographs { get; }
    public static UnicodeRange CjkUnifiedIdeographsExtensionA { get; }
    public static UnicodeRange CombiningDiacriticalMarks { get; }
    public static UnicodeRange CombiningDiacriticalMarksExtended { get; }
    public static UnicodeRange CombiningDiacriticalMarksforSymbols { get; }
    public static UnicodeRange CombiningDiacriticalMarksSupplement { get; }
    public static UnicodeRange CombiningHalfMarks { get; }
    public static UnicodeRange CommonIndicNumberForms { get; }
    public static UnicodeRange ControlPictures { get; }
    public static UnicodeRange Coptic { get; }
    public static UnicodeRange CurrencySymbols { get; }
    public static UnicodeRange Cyrillic { get; }
    public static UnicodeRange CyrillicExtendedA { get; }
    public static UnicodeRange CyrillicExtendedB { get; }
    public static UnicodeRange CyrillicSupplement { get; }
    public static UnicodeRange Devanagari { get; }
    public static UnicodeRange DevanagariExtended { get; }
    public static UnicodeRange Dingbats { get; }
    public static UnicodeRange EnclosedAlphanumerics { get; }
    public static UnicodeRange EnclosedCjkLettersandMonths { get; }
    public static UnicodeRange Ethiopic { get; }
    public static UnicodeRange EthiopicExtended { get; }
    public static UnicodeRange EthiopicExtendedA { get; }
    public static UnicodeRange EthiopicSupplement { get; }
    public static UnicodeRange GeneralPunctuation { get; }
    public static UnicodeRange GeometricShapes { get; }
    public static UnicodeRange Georgian { get; }
    public static UnicodeRange GeorgianSupplement { get; }
    public static UnicodeRange Glagolitic { get; }
    public static UnicodeRange GreekandCoptic { get; }
    public static UnicodeRange GreekExtended { get; }
    public static UnicodeRange Gujarati { get; }
    public static UnicodeRange Gurmukhi { get; }
    public static UnicodeRange HalfwidthandFullwidthForms { get; }
    public static UnicodeRange HangulCompatibilityJamo { get; }
    public static UnicodeRange HangulJamo { get; }
    public static UnicodeRange HangulJamoExtendedA { get; }
    public static UnicodeRange HangulJamoExtendedB { get; }
    public static UnicodeRange HangulSyllables { get; }
    public static UnicodeRange Hanunoo { get; }
    public static UnicodeRange Hebrew { get; }
    public static UnicodeRange Hiragana { get; }
    public static UnicodeRange IdeographicDescriptionCharacters { get; }
    public static UnicodeRange IpaExtensions { get; }
    public static UnicodeRange Javanese { get; }
    public static UnicodeRange Kanbun { get; }
    public static UnicodeRange KangxiRadicals { get; }
    public static UnicodeRange Kannada { get; }
    public static UnicodeRange Katakana { get; }
    public static UnicodeRange KatakanaPhoneticExtensions { get; }
    public static UnicodeRange KayahLi { get; }
    public static UnicodeRange Khmer { get; }
    public static UnicodeRange KhmerSymbols { get; }
    public static UnicodeRange Lao { get; }
    public static UnicodeRange Latin1Supplement { get; }
    public static UnicodeRange LatinExtendedA { get; }
    public static UnicodeRange LatinExtendedAdditional { get; }
    public static UnicodeRange LatinExtendedB { get; }
    public static UnicodeRange LatinExtendedC { get; }
    public static UnicodeRange LatinExtendedD { get; }
    public static UnicodeRange LatinExtendedE { get; }
    public static UnicodeRange Lepcha { get; }
    public static UnicodeRange LetterlikeSymbols { get; }
    public static UnicodeRange Limbu { get; }
    public static UnicodeRange Lisu { get; }
    public static UnicodeRange Malayalam { get; }
    public static UnicodeRange Mandaic { get; }
    public static UnicodeRange MathematicalOperators { get; }
    public static UnicodeRange MeeteiMayek { get; }
    public static UnicodeRange MeeteiMayekExtensions { get; }
    public static UnicodeRange MiscellaneousMathematicalSymbolsA { get; }
    public static UnicodeRange MiscellaneousMathematicalSymbolsB { get; }
    public static UnicodeRange MiscellaneousSymbols { get; }
    public static UnicodeRange MiscellaneousSymbolsandArrows { get; }
    public static UnicodeRange MiscellaneousTechnical { get; }
    public static UnicodeRange ModifierToneLetters { get; }
    public static UnicodeRange Mongolian { get; }
    public static UnicodeRange Myanmar { get; }
    public static UnicodeRange MyanmarExtendedA { get; }
    public static UnicodeRange MyanmarExtendedB { get; }
    public static UnicodeRange NewTaiLue { get; }
    public static UnicodeRange NKo { get; }
    public static UnicodeRange None { get; }
    public static UnicodeRange NumberForms { get; }
    public static UnicodeRange Ogham { get; }
    public static UnicodeRange OlChiki { get; }
    public static UnicodeRange OpticalCharacterRecognition { get; }
    public static UnicodeRange Oriya { get; }
    public static UnicodeRange Phagspa { get; }
    public static UnicodeRange PhoneticExtensions { get; }
    public static UnicodeRange PhoneticExtensionsSupplement { get; }
    public static UnicodeRange Rejang { get; }
    public static UnicodeRange Runic { get; }
    public static UnicodeRange Samaritan { get; }
    public static UnicodeRange Saurashtra { get; }
    public static UnicodeRange Sinhala { get; }
    public static UnicodeRange SmallFormVariants { get; }
    public static UnicodeRange SpacingModifierLetters { get; }
    public static UnicodeRange Specials { get; }
    public static UnicodeRange Sundanese { get; }
    public static UnicodeRange SundaneseSupplement { get; }
    public static UnicodeRange SuperscriptsandSubscripts { get; }
    public static UnicodeRange SupplementalArrowsA { get; }
    public static UnicodeRange SupplementalArrowsB { get; }
    public static UnicodeRange SupplementalMathematicalOperators { get; }
    public static UnicodeRange SupplementalPunctuation { get; }
    public static UnicodeRange SylotiNagri { get; }
    public static UnicodeRange Syriac { get; }
    public static UnicodeRange Tagalog { get; }
    public static UnicodeRange Tagbanwa { get; }
    public static UnicodeRange TaiLe { get; }
    public static UnicodeRange TaiTham { get; }
    public static UnicodeRange TaiViet { get; }
    public static UnicodeRange Tamil { get; }
    public static UnicodeRange Telugu { get; }
    public static UnicodeRange Thaana { get; }
    public static UnicodeRange Thai { get; }
    public static UnicodeRange Tibetan { get; }
    public static UnicodeRange Tifinagh { get; }
    public static UnicodeRange UnifiedCanadianAboriginalSyllabics { get; }
    public static UnicodeRange UnifiedCanadianAboriginalSyllabicsExtended { get; }
    public static UnicodeRange Vai { get; }
    public static UnicodeRange VariationSelectors { get; }
    public static UnicodeRange VedicExtensions { get; }
    public static UnicodeRange VerticalForms { get; }
    public static UnicodeRange YijingHexagramSymbols { get; }
    public static UnicodeRange YiRadicals { get; }
    public static UnicodeRange YiSyllables { get; }
  }
 }
```
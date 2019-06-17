# System.Text.Encoding.Extensions

```diff
---D:\Temp\nullable\before
+++D:\Temp\nullable\after
 assembly System.Text.Encoding.Extensions {
  namespace System.Text {
    public class ASCIIEncoding : Encoding {
      public ASCIIEncoding();
      public override bool IsSingleByte { get; }
      public unsafe override int GetByteCount(char* chars, int count);
      public override int GetByteCount(char[] chars, int index, int count);
+     public override int GetByteCount(ReadOnlySpan<char> chars);
      public override int GetByteCount(string chars);
      public unsafe override int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public override int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
+     public override int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes);
      public override int GetBytes(string chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe override int GetCharCount(byte* bytes, int count);
      public override int GetCharCount(byte[] bytes, int index, int count);
+     public override int GetCharCount(ReadOnlySpan<byte> bytes);
      public unsafe override int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public override int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
+     public override int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars);
      public override Decoder GetDecoder();
      public override Encoder GetEncoder();
      public override int GetMaxByteCount(int charCount);
      public override int GetMaxCharCount(int byteCount);
      public override string GetString(byte[] bytes, int byteIndex, int byteCount);
    }
    public class UnicodeEncoding : Encoding {
      public const int CharSize = 2;
      public UnicodeEncoding();
      public UnicodeEncoding(bool bigEndian, bool byteOrderMark);
      public UnicodeEncoding(bool bigEndian, bool byteOrderMark, bool throwOnInvalidBytes);
+     public override ReadOnlySpan<byte> Preamble { get; }
      public override bool Equals(object value);
      public unsafe override int GetByteCount(char* chars, int count);
      public override int GetByteCount(char[] chars, int index, int count);
      public override int GetByteCount(string s);
      public unsafe override int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public override int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public override int GetBytes(string s, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe override int GetCharCount(byte* bytes, int count);
      public override int GetCharCount(byte[] bytes, int index, int count);
      public unsafe override int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public override int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
      public override Decoder GetDecoder();
      public override Encoder GetEncoder();
      public override int GetHashCode();
      public override int GetMaxByteCount(int charCount);
      public override int GetMaxCharCount(int byteCount);
      public override byte[] GetPreamble();
      public override string GetString(byte[] bytes, int index, int count);
    }
    public sealed class UTF32Encoding : Encoding {
      public UTF32Encoding();
      public UTF32Encoding(bool bigEndian, bool byteOrderMark);
      public UTF32Encoding(bool bigEndian, bool byteOrderMark, bool throwOnInvalidCharacters);
+     public override ReadOnlySpan<byte> Preamble { get; }
      public override bool Equals(object value);
      public unsafe override int GetByteCount(char* chars, int count);
      public override int GetByteCount(char[] chars, int index, int count);
      public override int GetByteCount(string s);
      public unsafe override int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public override int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public override int GetBytes(string s, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe override int GetCharCount(byte* bytes, int count);
      public override int GetCharCount(byte[] bytes, int index, int count);
      public unsafe override int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public override int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
      public override Decoder GetDecoder();
      public override Encoder GetEncoder();
      public override int GetHashCode();
      public override int GetMaxByteCount(int charCount);
      public override int GetMaxCharCount(int byteCount);
      public override byte[] GetPreamble();
      public override string GetString(byte[] bytes, int index, int count);
    }
    public class UTF7Encoding : Encoding {
      public UTF7Encoding();
      public UTF7Encoding(bool allowOptionals);
      public override bool Equals(object value);
      public unsafe override int GetByteCount(char* chars, int count);
      public override int GetByteCount(char[] chars, int index, int count);
      public override int GetByteCount(string s);
      public unsafe override int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public override int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public override int GetBytes(string s, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe override int GetCharCount(byte* bytes, int count);
      public override int GetCharCount(byte[] bytes, int index, int count);
      public unsafe override int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public override int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
      public override Decoder GetDecoder();
      public override Encoder GetEncoder();
      public override int GetHashCode();
      public override int GetMaxByteCount(int charCount);
      public override int GetMaxCharCount(int byteCount);
      public override string GetString(byte[] bytes, int index, int count);
    }
    public class UTF8Encoding : Encoding {
      public UTF8Encoding();
      public UTF8Encoding(bool encoderShouldEmitUTF8Identifier);
      public UTF8Encoding(bool encoderShouldEmitUTF8Identifier, bool throwOnInvalidBytes);
+     public override ReadOnlySpan<byte> Preamble { get; }
      public override bool Equals(object value);
      public unsafe override int GetByteCount(char* chars, int count);
      public override int GetByteCount(char[] chars, int index, int count);
+     public override int GetByteCount(ReadOnlySpan<char> chars);
      public override int GetByteCount(string chars);
      public unsafe override int GetBytes(char* chars, int charCount, byte* bytes, int byteCount);
      public override int GetBytes(char[] chars, int charIndex, int charCount, byte[] bytes, int byteIndex);
+     public override int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes);
      public override int GetBytes(string s, int charIndex, int charCount, byte[] bytes, int byteIndex);
      public unsafe override int GetCharCount(byte* bytes, int count);
      public override int GetCharCount(byte[] bytes, int index, int count);
+     public override int GetCharCount(ReadOnlySpan<byte> bytes);
      public unsafe override int GetChars(byte* bytes, int byteCount, char* chars, int charCount);
      public override int GetChars(byte[] bytes, int byteIndex, int byteCount, char[] chars, int charIndex);
+     public override int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars);
      public override Decoder GetDecoder();
      public override Encoder GetEncoder();
      public override int GetHashCode();
      public override int GetMaxByteCount(int charCount);
      public override int GetMaxCharCount(int byteCount);
      public override byte[] GetPreamble();
      public override string GetString(byte[] bytes, int index, int count);
    }
  }
 }
```

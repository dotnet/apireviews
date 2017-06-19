```C#
namespace System.Buffers.Text {
    public static class Base64 {
        public static int ComputeDecodedLength(ReadOnlySpan<byte> source);
        public static int ComputeEncodedLength(int sourceLength);
        public static TransformationStatus Decode(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public static TransformationStatus DecodeInPlace(Span<byte> buffer, out int bytesConsumed, out int bytesWritten);
        public static TransformationStatus Encode(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        public static TransformationStatus EncodeInPlace(Span<byte> buffer, int sourceLength, out int bytesWritten);
    }
    public static class Encoders {
        public static class Utf16 {
            public static TransformationStatus ComputeEncodedBytesFromUtf32(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ComputeEncodedBytesFromUtf8(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ConvertFromUtf32(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
            public static TransformationStatus ConvertFromUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        }
        public static class Utf32 {
            public static TransformationStatus ComputeEncodedBytesFromUtf16(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ComputeEncodedBytesFromUtf8(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ConvertFromUtf16(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
            public static TransformationStatus ConvertFromUtf8(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        }
        public static class Utf8 {
            public static TransformationStatus ComputeEncodedBytesFromUtf16(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ComputeEncodedBytesFromUtf32(ReadOnlySpan<byte> source, out int bytesNeeded);
            public static TransformationStatus ConvertFromUtf16(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
            public static TransformationStatus ConvertFromUtf32(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
        }
    }
    public interface IBufferFormattable {
        bool TryFormat(Span<byte> buffer, out int written, TextFormat format=default(TextFormat), SymbolTable symbol=null);
    }
    public static class Formatters {
        public static bool TryFormat(this byte value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this bool value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this DateTime value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this DateTimeOffset value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this double value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this decimal value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this Guid value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this short value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this int value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this long value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this sbyte value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this float value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this TimeSpan value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this ushort value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this uint value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryFormat(this ulong value, Span<byte> buffer, out int bytesWritten, TextFormat format=default(TextFormat), SymbolTable symbol=null);
    }
    public static class Parsers {
        public static bool TryParse(ReadOnlySpan<byte> text, out bool value, out int bytesConsumed, SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out byte value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out decimal value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out short value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out int value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out long value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out sbyte value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out ushort value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out uint value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out ulong value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out DateTime value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out DateTimeOffset value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out double value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out Guid value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out TimeSpan value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static bool TryParse(ReadOnlySpan<byte> text, out float value, out int bytesConsumed, TextFormat format=default(TextFormat), SymbolTable symbol=null);
        public static class InvariantUtf16 {
            public static bool TryParse(ReadOnlySpan<byte> text, out bool value, out int bytesConsumed);
            public static bool TryParse(ReadOnlySpan<byte> text, out byte value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out decimal value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out short value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out int value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out long value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out sbyte value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out ushort value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out uint value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out ulong value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out DateTime value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out DateTimeOffset value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out double value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out Guid value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out TimeSpan value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out float value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static class Hex {
                public static bool TryParse(ReadOnlySpan<byte> text, out byte value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out short value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out int value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out long value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out sbyte value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out ushort value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out uint value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out ulong value, out int bytesConsumed);
            }
        }
        public static class InvariantUtf8 {
            public static bool TryParse(ReadOnlySpan<byte> text, out bool value, out int bytesConsumed);
            public static bool TryParse(ReadOnlySpan<byte> text, out byte value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out decimal value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out short value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out int value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out long value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out sbyte value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out ushort value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out uint value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out ulong value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out DateTime value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out DateTimeOffset value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out double value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out Guid value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out TimeSpan value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static bool TryParse(ReadOnlySpan<byte> text, out float value, out int bytesConsumed, TextFormat format=default(TextFormat));
            public static class Hex {
                public static bool TryParse(ReadOnlySpan<byte> text, out byte value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out short value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out int value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out long value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out sbyte value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out ushort value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out uint value, out int bytesConsumed);
                public static bool TryParse(ReadOnlySpan<byte> text, out ulong value, out int bytesConsumed);
            }
        }
    }
    public abstract class SymbolTable
    {
        public static SymbolTable InvariantUtf8;
        public static SymbolTable InvariantUtf16;
        public abstract bool TryEncode(Symbol symbol, Span<byte> destination, out int bytesWritten);
        public abstract bool TryEncode(byte utf8, Span<byte> destination, out int bytesWritten);
        public abstract bool TryEncode(ReadOnlySpan<byte> utf8, Span<byte> destination, out int bytesWritten);
        public abstract bool TryParse(ReadOnlySpan<byte> source, out Symbol symbol, out int bytesConsumed);
        public abstract bool TryParse(ReadOnlySpan<byte> source, out byte utf8, out int bytesConsumed);
        public abstract bool TryParse(ReadOnlySpan<byte> source, Span<byte> utf8, out int bytesConsumed);
        public enum Symbol : ushort {
            D0 = (ushort)0,
            D1 = (ushort)1,
            D2 = (ushort)2,
            D3 = (ushort)3,
            D4 = (ushort)4,
            D5 = (ushort)5,
            D6 = (ushort)6,
            D7 = (ushort)7,
            D8 = (ushort)8,
            D9 = (ushort)9,
            DecimalSeparator = (ushort)10,
            Exponent = (ushort)16,
            GroupSeparator = (ushort)11,
            InfinitySign = (ushort)12,
            MinusSign = (ushort)13,
            NaN = (ushort)15,
            PlusSign = (ushort)14,
        }
    }
    public struct TextFormat : IEquatable<TextFormat> {
        public const byte NoPrecision = (byte)255;
        public static TextFormat HexLowercase;
        public static TextFormat HexUppercase;
        public TextFormat(char symbol, byte precision=(byte)255);
        public bool HasPrecision { get; }
        public bool IsDefault { get; }
        public bool IsHexadecimal { get; }
        public byte Precision { get; }
        public char Symbol { get; set; }
        public override bool Equals(object obj);
        public bool Equals(TextFormat other);
        public override int GetHashCode();
        public static bool operator ==(TextFormat left, TextFormat right);
        public static implicit operator TextFormat (char symbol);
        public static bool operator !=(TextFormat left, TextFormat right);
        public static TextFormat Parse(char format);
        public static TextFormat Parse(ReadOnlySpan<char> format);
        public static TextFormat Parse(string format);
        public override string ToString();
    }
}
```

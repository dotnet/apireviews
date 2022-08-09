# API Review 08/09/2022

## Add cultureName constructors to GeneratedRegex

**Approved** | [#runtime/59492](https://github.com/dotnet/runtime/issues/59492#issuecomment-1209657256) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=0h0m0s)

* Looks good as proposed

```diff
namespace System.Text.RegularExpressions;

[AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = false)]
public sealed class GeneratedRegexAttribute : System.Attribute
{
    public GeneratedRegexAttribute([StringSyntax(StringSyntaxAttribute.Regex)] string pattern) { }
    public GeneratedRegexAttribute([StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options) { }
+   public GeneratedRegexAttribute([StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options, string cultureName) { }
    public GeneratedRegexAttribute([StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options, int matchTimeoutMilliseconds) { }
+   public GeneratedRegexAttribute([StringSyntax(StringSyntaxAttribute.Regex, "options")] string pattern, RegexOptions options, int matchTimeoutMilliseconds, string cultureName) { }

+   public string CultureName { get; }
}
```
## Trim friendly Font ToLogFont/FromLogFont

**Approved** | [#runtime/70358](https://github.com/dotnet/runtime/issues/70358#issuecomment-1209689638) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=0h15m42s)

* We made Interop.LOGFONT public and removed the genericness of the functions

```C#
namespace System.Drawing
{
    public sealed partial class Font
    {
        public void ToLogFont(out Interop.LOGFONT logFont, Graphics graphics)  { throw null; }
        public static Font FromLogFont(in Interop.LOGFONT logFont) { throw null; }
        public static Font FromLogFont(in Interop.LOGFONT logFont, IntPtr hdc) { throw null; }
        public void ToLogFont(out Interop.LOGFONT logFont) { throw null; }
    }
}

namespace System.Drawing.Interop
{
    public unsafe struct LOGFONT
    {
        public int lfHeight;
        public int lfWidth;
        public int lfEscapement;
        public int lfOrientation;
        public int lfWeight;
        public byte lfItalic;
        public byte lfUnderline;
        public byte lfStrikeOut;
        public byte lfCharSet;
        public byte lfOutPrecision;
        public byte lfClipPrecision;
        public byte lfQuality;
        public byte lfPitchAndFamily;
        private fixed char _lfFaceName[LF_FACESIZE];

        [UnscopedRef]
        public Span<char> lfFaceName => MemoryMarshal.CreateSpan(ref _lfFaceName[0], LF_FACESIZE);
    }
}
```
## Obsolete XmlSecureResolver

**Approved** | [#runtime/73636](https://github.com/dotnet/runtime/issues/73636#issuecomment-1209699938) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=0h42m23s)

* Looks good as proposed

```C#
// System.Xml.ReaderWriter.dll
namespace System.Xml
{
    [Obsolete(/* ... SYSLIB0047 ... */)]
    public class XmlSecureResolver : XmlResolver
    {
        public XmlSecureResolver(XmlResolver resolver, string? securityUrl); // existing ctor, no change

        public override object? GetEntity(Uri absoluteUri, string? role, Type? ofObjectToReturn)
        {
            throw new XmlException(/* ... */);
        }
    }
}
```
## XmlResolver.NullResolver

**Approved** | [#runtime/73637](https://github.com/dotnet/runtime/issues/73637#issuecomment-1209706181) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=0h53m4s)

* We changed "Null" to "Throwing" to better indicate the behavior and intent.

```C#
// System.Xml.ReaderWriter.dll
namespace System.Xml
{
    public partial class XmlResolver
    {
        public static System.Xml.XmlResolver ThrowingResolver { get; }
    }
}
```
## Add InstructionEncoder.Switch to enable emitting switch instructions with LabelHandles

**Approved** | [#runtime/40546](https://github.com/dotnet/runtime/issues/40546#issuecomment-1209732905) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=0h59m34s)

* We went with the alternative proposal with `SwitchInstructionEncoder` instead of the params array, since it felt more consistent with existing paradigms.

```C#
namespace System.Reflection.Metadata.Ecma335
{
    public readonly struct SwitchInstructionEncoder
    {
        public void Branch(LabelHandle label);
    }

    public partial struct InstructionEncoder
    {
        public SwitchInstructionEncoder Switch(int branchCount);
    }
}
```
## Add support for the WASM SIMD APIs

**Approved** | [#runtime/53730](https://github.com/dotnet/runtime/issues/53730#issuecomment-1209770214) | [Video](https://www.youtube.com/watch?v=_lbBfpxE5JI&t=1h21m43s)

* We went through all the parameter names in the meeting and made them as consistent with other ISAs as possible
* We changed WasmBase to PackedSimd because it's not the "base" ISA for WASM, but the first extension to it ("WebAssembly 128-bit Packed SIMD Extension")

```C#
namespace System.Runtime.Intrinsics.Wasm
{
    public abstract class PackedSimd
    {
        public static bool IsSupported { get; }

        // Constructing SIMD Values

        // cut (lives somewhere else)
        //public static Vector128<T> Constant(ImmByte[16] imm);

        public static Vector128<byte>   Splat(uint   value);
        public static Vector128<sbyte>  Splat(int    value);
        public static Vector128<ushort> Splat(uint   value);
        public static Vector128<int>    Splat(int    value);
        public static Vector128<uint>   Splat(uint   value);
        public static Vector128<long>   Splat(long   value);
        public static Vector128<ulong>  Splat(ulong  value);
        public static Vector128<float>  Splat(float  value);
        public static Vector128<double> Splat(double value);
        public static Vector128<nint>   Splat(nint   value);
        public static Vector128<nuint>  Splat(nuint  value);

        // Accessing lanes

        public static int    ExtractLane(Vector128<sbyte>  value, byte index);    // takes ImmLaneIdx16
        public static uint   ExtractLane(Vector128<byte>   value, byte index);    // takes ImmLaneIdx16
        public static int    ExtractLane(Vector128<short>  value, byte index);    // takes ImmLaneIdx8
        public static uint   ExtractLane(Vector128<ushort> value, byte index);    // takes ImmLaneIdx8
        public static int    ExtractLane(Vector128<int>    value, byte index);    // takes ImmLaneIdx4
        public static uint   ExtractLane(Vector128<uint>   value, byte index);    // takes ImmLaneIdx4
        public static long   ExtractLane(Vector128<long>   value, byte index);    // takes ImmLaneIdx2
        public static ulong  ExtractLane(Vector128<ulong>  value, byte index);    // takes ImmLaneIdx2
        public static float  ExtractLane(Vector128<float>  value, byte index);    // takes ImmLaneIdx4
        public static double ExtractLane(Vector128<double> value, byte index);    // takes ImmLaneIdx2
        public static nint   ExtractLane(Vector128<nint>   value, byte index);
        public static nuint  ExtractLane(Vector128<nuint>  value, byte index);

        public static Vector128<sbyte>  ReplaceLane(Vector128<sbyte>  vector, byte imm, int    value);   // takes ImmLaneIdx16
        public static Vector128<byte>   ReplaceLane(Vector128<byte>   vector, byte imm, uint   value);   // takes ImmLaneIdx16
        public static Vector128<short>  ReplaceLane(Vector128<short>  vector, byte imm, int    value);   // takes ImmLaneIdx8
        public static Vector128<ushort> ReplaceLane(Vector128<ushort> vector, byte imm, uint   value);   // takes ImmLaneIdx8
        public static Vector128<int>    ReplaceLane(Vector128<int>    vector, byte imm, int    value);   // takes ImmLaneIdx4
        public static Vector128<int>    ReplaceLane(Vector128<uint>   vector, byte imm, uint   value);   // takes ImmLaneIdx4
        public static Vector128<long>   ReplaceLane(Vector128<long>   vector, byte imm, long   value);   // takes ImmLaneIdx2
        public static Vector128<ulong>  ReplaceLane(Vector128<ulong>  vector, byte imm, ulong  value);   // takes ImmLaneIdx2
        public static Vector128<float>  ReplaceLane(Vector128<float>  vector, byte imm, float  value);   // takes ImmLaneIdx4
        public static Vector128<double> ReplaceLane(Vector128<double> vector, byte imm, double value);   // takes ImmLaneIdx2
        public static Vector128<nint>   ReplaceLane(Vector128<nint>   vector, byte imm, nint   value);
        public static Vector128<nuint>  ReplaceLane(Vector128<nuint>  vector, byte imm, nuint  value);

        public static Vector128<sbyte> Shuffle(Vector128<sbyte> lower, Vector128<sbyte> upper, Vector128<sbyte> indices);
        public static Vector128<byte>  Shuffle(Vector128<byte>  lower, Vector128<byte>  upper, Vector128<byte> indices);

        public static Vector128<sbyte> Swizzle(Vector128<sbyte> vector, Vector128<sbyte> indices);
        public static Vector128<byte>  Swizzle(Vector128<byte>  vector, Vector128<byte>  indices);

        // Integer arithmetic

        public static Vector128<sbyte>  Add(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Add(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Add(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Add(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Add(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Add(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   Add(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  Add(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<nint>   Add(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  Add(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  Subtract(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Subtract(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Subtract(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Subtract(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Subtract(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Subtract(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   Subtract(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  Subtract(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<nint>   Subtract(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  Subtract(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<short>  Multiply(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Multiply(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Multiply(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Multiply(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   Multiply(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  Multiply(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<nint>   Multiply(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  Multiply(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<int> Dot(Vector128<short> left, Vector128<short> right);

        public static Vector128<sbyte>  Negate(Vector128<sbyte>  value);
        public static Vector128<byte>   Negate(Vector128<byte>   value);
        public static Vector128<short>  Negate(Vector128<short>  value);
        public static Vector128<ushort> Negate(Vector128<ushort> value);
        public static Vector128<int>    Negate(Vector128<int>    value);
        public static Vector128<uint>   Negate(Vector128<uint>   value);
        public static Vector128<long>   Negate(Vector128<long>   value);
        public static Vector128<ulong>  Negate(Vector128<ulong>  value);
        public static Vector128<nint>   Negate(Vector128<nint>   value);
        public static Vector128<nuint>  Negate(Vector128<nuint>  value);

        // Extended integer arithmetic

        public static Vector128<short>  MultiplyWideningLower(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> MultiplyWideningLower(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<int>    MultiplyWideningLower(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   MultiplyWideningLower(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<long>   MultiplyWideningLower(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<ulong>  MultiplyWideningLower(Vector128<uint>   left, Vector128<uint>   right);

        public static Vector128<short>  MultiplyWideningUpper(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<ushort> MultiplyWideningUpper(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<int>    MultiplyWideningUpper(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<uint>   MultiplyWideningUpper(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<long>   MultiplyWideningUpper(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<ulong>  MultiplyWideningUpper(Vector128<uint>   left, Vector128<uint>   right);

        public static Vector128<short>  AddPairwiseWidening(Vector128<sbyte>  value);
        public static Vector128<ushort> AddPairwiseWidening(Vector128<byte>   value);
        public static Vector128<int>    AddPairwiseWidening(Vector128<short>  value);
        public static Vector128<uint>   AddPairwiseWidening(Vector128<ushort> value);

        // Saturating integer arithmetic

        public static Vector128<sbyte>  AddSaturate(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   AddSaturate(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  AddSaturate(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> AddSaturate(Vector128<ushort> left, Vector128<ushort> right);

        public static Vector128<sbyte>  SubtractSaturate(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   SubtractSaturate(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  SubtractSaturate(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> SubtractSaturate(Vector128<ushort> left, Vector128<ushort> right);

        public static Vector128<short>  MultiplyRoundedSaturateQ15(Vector128<short> left, Vector128<short> right);

        public static Vector128<sbyte>  Min(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Min(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Min(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Min(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Min(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Min(Vector128<uint>   left, Vector128<uint>   right);

        public static Vector128<sbyte>  Max(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Max(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Max(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Max(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Max(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Max(Vector128<uint>   left, Vector128<uint>   right);

        public static Vector128<byte>   AverageRounded(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<ushort> AverageRounded(Vector128<ushort> left, Vector128<ushort> right);

        public static Vector128<sbyte>  Abs(Vector128<sbyte> value);
        public static Vector128<short>  Abs(Vector128<short> value);
        public static Vector128<int>    Abs(Vector128<int>   value);
        public static Vector128<long>   Abs(Vector128<long>  value);
        public static Vector128<nint>   Abs(Vector128<nint>  value);

        // Bit shifts

        public static Vector128<sbyte>  ShiftLeft(Vector128<sbyte>  value, int count);
        public static Vector128<byte>   ShiftLeft(Vector128<byte>   value, int count);
        public static Vector128<short>  ShiftLeft(Vector128<short>  value, int count);
        public static Vector128<ushort> ShiftLeft(Vector128<ushort> value, int count);
        public static Vector128<int>    ShiftLeft(Vector128<int>    value, int count);
        public static Vector128<uint>   ShiftLeft(Vector128<uint>   value, int count);
        public static Vector128<long>   ShiftLeft(Vector128<long>   value, int count);
        public static Vector128<ulong>  ShiftLeft(Vector128<ulong>  value, int count);
        public static Vector128<nint>   ShiftLeft(Vector128<nint>   value, int count);
        public static Vector128<nuint>  ShiftLeft(Vector128<nuint>  value, int count);

        public static Vector128<sbyte>  ShiftRightArithmetic(Vector128<sbyte>  value, int count);
        public static Vector128<byte>   ShiftRightArithmetic(Vector128<byte>   value, int count);
        public static Vector128<short>  ShiftRightArithmetic(Vector128<short>  value, int count);
        public static Vector128<ushort> ShiftRightArithmetic(Vector128<ushort> value, int count);
        public static Vector128<int>    ShiftRightArithmetic(Vector128<int>    value, int count);
        public static Vector128<uint>   ShiftRightArithmetic(Vector128<uint>   value, int count);
        public static Vector128<long>   ShiftRightArithmetic(Vector128<long>   value, int count);
        public static Vector128<ulong>  ShiftRightArithmetic(Vector128<ulong>  value, int count);
        public static Vector128<nint>   ShiftRightArithmetic(Vector128<nint>   value, int count);
        public static Vector128<nuint>  ShiftRightArithmetic(Vector128<nuint>  value, int count);

        public static Vector128<sbyte>  ShiftRightLogical(Vector128<sbyte>  value, int count);
        public static Vector128<byte>   ShiftRightLogical(Vector128<byte>   value, int count);
        public static Vector128<short>  ShiftRightLogical(Vector128<short>  value, int count);
        public static Vector128<ushort> ShiftRightLogical(Vector128<ushort> value, int count);
        public static Vector128<int>    ShiftRightLogical(Vector128<int>    value, int count);
        public static Vector128<uint>   ShiftRightLogical(Vector128<uint>   value, int count);
        public static Vector128<long>   ShiftRightLogical(Vector128<long>   value, int count);
        public static Vector128<ulong>  ShiftRightLogical(Vector128<ulong>  value, int count);
        public static Vector128<nint>   ShiftRightLogical(Vector128<nint>   value, int count);
        public static Vector128<nuint>  ShiftRightLogical(Vector128<nuint>  value, int count);

        // Bitwise operations

        public static Vector128<sbyte>  And(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   And(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  And(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> And(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    And(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   And(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   And(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  And(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  And(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> And(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   And(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  And(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  Or(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Or(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Or(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Or(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Or(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Or(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   Or(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  Or(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  Or(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Or(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   Or(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  Or(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  Xor(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   Xor(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  Xor(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> Xor(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    Xor(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   Xor(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   Xor(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  Xor(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  Xor(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Xor(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   Xor(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  Xor(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  Not(Vector128<sbyte>  value);
        public static Vector128<byte>   Not(Vector128<byte>   value);
        public static Vector128<short>  Not(Vector128<short>  value);
        public static Vector128<ushort> Not(Vector128<ushort> value);
        public static Vector128<int>    Not(Vector128<int>    value);
        public static Vector128<uint>   Not(Vector128<uint>   value);
        public static Vector128<long>   Not(Vector128<long>   value);
        public static Vector128<ulong>  Not(Vector128<ulong>  value);
        public static Vector128<float>  Not(Vector128<float>  value);
        public static Vector128<double> Not(Vector128<double> value);
        public static Vector128<nint>   Not(Vector128<nint>   value);
        public static Vector128<nuint>  Not(Vector128<nuint>  value);

        public static Vector128<sbyte>  AndNot(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   AndNot(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  AndNot(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> AndNot(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    AndNot(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   AndNot(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   AndNot(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  AndNot(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  AndNot(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> AndNot(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   AndNot(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  AndNot(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  BitwiseSelect(Vector128<sbyte>  left, Vector128<sbyte>  right, Vector128<sbyte>  select);
        public static Vector128<byte>   BitwiseSelect(Vector128<byte>   left, Vector128<byte>   right, Vector128<byte>   select);
        public static Vector128<short>  BitwiseSelect(Vector128<short>  left, Vector128<short>  right, Vector128<short>  select);
        public static Vector128<ushort> BitwiseSelect(Vector128<ushort> left, Vector128<ushort> right, Vector128<ushort> select);
        public static Vector128<int>    BitwiseSelect(Vector128<int>    left, Vector128<int>    right, Vector128<int>    select);
        public static Vector128<uint>   BitwiseSelect(Vector128<uint>   left, Vector128<uint>   right, Vector128<uint>   select);
        public static Vector128<long>   BitwiseSelect(Vector128<long>   left, Vector128<long>   right, Vector128<long>   select);
        public static Vector128<ulong>  BitwiseSelect(Vector128<ulong>  left, Vector128<ulong>  right, Vector128<ulong>  select);
        public static Vector128<float>  BitwiseSelect(Vector128<float>  left, Vector128<float>  right, Vector128<float>  select);
        public static Vector128<double> BitwiseSelect(Vector128<double> left, Vector128<double> right, Vector128<double> select);
        public static Vector128<nint>   BitwiseSelect(Vector128<nint>   left, Vector128<nint>   right, Vector128<nint>   select);
        public static Vector128<nuint>  BitwiseSelect(Vector128<nuint>  left, Vector128<nuint>  right, Vector128<nuint>  select);

        public static Vector128<byte> PopCount(Vector128<byte> value);

        // Boolean horizontal reductions

        public static bool AnyTrue(Vector128<sbyte>  value); // returns i32, AnyBitSet?
        public static bool AnyTrue(Vector128<byte>   value); // returns i32
        public static bool AnyTrue(Vector128<short>  value); // returns i32
        public static bool AnyTrue(Vector128<ushort> value); // returns i32
        public static bool AnyTrue(Vector128<int>    value); // returns i32
        public static bool AnyTrue(Vector128<uint>   value); // returns i32
        public static bool AnyTrue(Vector128<long>   value); // returns i32
        public static bool AnyTrue(Vector128<ulong>  value); // returns i32
        public static bool AnyTrue(Vector128<float>  value); // returns i32
        public static bool AnyTrue(Vector128<double> value); // returns i32
        public static bool AnyTrue(Vector128<nint>   value); // returns i32
        public static bool AnyTrue(Vector128<nuint>  value); // returns i32

        public static bool AllTrue(Vector128<sbyte>  value); // returns i32, AreAllNonZero?
        public static bool AllTrue(Vector128<byte>   value); // returns i32
        public static bool AllTrue(Vector128<short>  value); // returns i32
        public static bool AllTrue(Vector128<ushort> value); // returns i32
        public static bool AllTrue(Vector128<int>    value); // returns i32
        public static bool AllTrue(Vector128<uint>   value); // returns i32
        public static bool AllTrue(Vector128<long>   value); // returns i32
        public static bool AllTrue(Vector128<ulong>  value); // returns i32
        public static bool AllTrue(Vector128<nint>   value); // returns i32
        public static bool AllTrue(Vector128<nuint>  value); // returns i32

        // Bitmask extraction

        public static int Bitmask(Vector128<sbyte>  value);
        public static int Bitmask(Vector128<byte>   value);
        public static int Bitmask(Vector128<short>  value);
        public static int Bitmask(Vector128<ushort> value);
        public static int Bitmask(Vector128<int>    value);
        public static int Bitmask(Vector128<uint>   value);
        public static int Bitmask(Vector128<long>   value);
        public static int Bitmask(Vector128<ulong>  value);
        public static int Bitmask(Vector128<nint>   value);
        public static int Bitmask(Vector128<nuint>  value);

        // Comparisons

        public static Vector128<sbyte>  CompareEqual(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareEqual(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareEqual(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareEqual(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareEqual(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareEqual(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareEqual(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareEqual(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareEqual(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareEqual(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareEqual(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareEqual(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  CompareNotEqual(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareNotEqual(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareNotEqual(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareNotEqual(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareNotEqual(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareNotEqual(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareNotEqual(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareNotEqual(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareNotEqual(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareNotEqual(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareNotEqual(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareNotEqual(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  CompareLessThan(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareLessThan(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareLessThan(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareLessThan(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareLessThan(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareLessThan(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareLessThan(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareLessThan(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareLessThan(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareLessThan(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareLessThan(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareLessThan(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  CompareLessThanOrEqual(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareLessThanOrEqual(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareLessThanOrEqual(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareLessThanOrEqual(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareLessThanOrEqual(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareLessThanOrEqual(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareLessThanOrEqual(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareLessThanOrEqual(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareLessThanOrEqual(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareLessThanOrEqual(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareLessThanOrEqual(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareLessThanOrEqual(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  CompareGreaterThan(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareGreaterThan(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareGreaterThan(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareGreaterThan(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareGreaterThan(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareGreaterThan(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareGreaterThan(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareGreaterThan(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareGreaterThan(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareGreaterThan(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareGreaterThan(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareGreaterThan(Vector128<nuint>  left, Vector128<nuint>  right);

        public static Vector128<sbyte>  CompareGreaterThanOrEqual(Vector128<sbyte>  left, Vector128<sbyte>  right);
        public static Vector128<byte>   CompareGreaterThanOrEqual(Vector128<byte>   left, Vector128<byte>   right);
        public static Vector128<short>  CompareGreaterThanOrEqual(Vector128<short>  left, Vector128<short>  right);
        public static Vector128<ushort> CompareGreaterThanOrEqual(Vector128<ushort> left, Vector128<ushort> right);
        public static Vector128<int>    CompareGreaterThanOrEqual(Vector128<int>    left, Vector128<int>    right);
        public static Vector128<uint>   CompareGreaterThanOrEqual(Vector128<uint>   left, Vector128<uint>   right);
        public static Vector128<long>   CompareGreaterThanOrEqual(Vector128<long>   left, Vector128<long>   right);
        public static Vector128<ulong>  CompareGreaterThanOrEqual(Vector128<ulong>  left, Vector128<ulong>  right);
        public static Vector128<float>  CompareGreaterThanOrEqual(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> CompareGreaterThanOrEqual(Vector128<double> left, Vector128<double> right);
        public static Vector128<nint>   CompareGreaterThanOrEqual(Vector128<nint>   left, Vector128<nint>   right);
        public static Vector128<nuint>  CompareGreaterThanOrEqual(Vector128<nuint>  left, Vector128<nuint>  right);

        // Load and store

        public static Vector128<sbyte>  LoadVector128(sbyte*  address);
        public static Vector128<byte>   LoadVector128(byte*   address);
        public static Vector128<short>  LoadVector128(short*  address);
        public static Vector128<ushort> LoadVector128(ushort* address);
        public static Vector128<int>    LoadVector128(int*    address);
        public static Vector128<uint>   LoadVector128(uint*   address);
        public static Vector128<long>   LoadVector128(long*   address);
        public static Vector128<ulong>  LoadVector128(ulong*  address);
        public static Vector128<float>  LoadVector128(float*  address);
        public static Vector128<double> LoadVector128(double* address);
        public static Vector128<nint>   LoadVector128(nint*   address);
        public static Vector128<nuint>  LoadVector128(nuint*  address);

        public static Vector128<int>    LoadScalarVector128(int*    address);
        public static Vector128<uint>   LoadScalarVector128(uint*   address);
        public static Vector128<long>   LoadScalarVector128(long*   address);
        public static Vector128<ulong>  LoadScalarVector128(ulong*  address);
        public static Vector128<float>  LoadScalarVector128(float*  address);
        public static Vector128<double> LoadScalarVector128(double* address);
        public static Vector128<nint>   LoadScalarVector128(nint*   address);
        public static Vector128<nuint>  LoadScalarVector128(nuint*  address);

        public static Vector128<sbyte>  LoadScalarAndSplatVector128(sbyte*  address);
        public static Vector128<byte>   LoadScalarAndSplatVector128(byte*   address);
        public static Vector128<short>  LoadScalarAndSplatVector128(short*  address);
        public static Vector128<ushort> LoadScalarAndSplatVector128(ushort* address);
        public static Vector128<int>    LoadScalarAndSplatVector128(int*    address);
        public static Vector128<uint>   LoadScalarAndSplatVector128(uint*   address);
        public static Vector128<long>   LoadScalarAndSplatVector128(long*   address);
        public static Vector128<ulong>  LoadScalarAndSplatVector128(ulong*  address);
        public static Vector128<float>  LoadScalarAndSplatVector128(float*  address);
        public static Vector128<double> LoadScalarAndSplatVector128(double* address);
        public static Vector128<nint>   LoadScalarAndSplatVector128(nint*   address);
        public static Vector128<nuint>  LoadScalarAndSplatVector128(nuint*  address);

        public static Vector128<sbyte>  LoadScalarAndInsert(sbyte*  address, Vector128<sbyte>  vector, byte index); // takes ImmLaneIdx16
        public static Vector128<byte>   LoadScalarAndInsert(byte*   address, Vector128<byte>   vector, byte index); // takes ImmLaneIdx16
        public static Vector128<short>  LoadScalarAndInsert(short*  address, Vector128<short>  vector, byte index); // takes ImmLaneIdx8
        public static Vector128<ushort> LoadScalarAndInsert(ushort* address, Vector128<ushort> vector, byte index); // takes ImmLaneIdx8
        public static Vector128<int>    LoadScalarAndInsert(int*    address, Vector128<int>    vector, byte index); // takes ImmLaneIdx4
        public static Vector128<uint>   LoadScalarAndInsert(uint*   address, Vector128<uint>   vector, byte index); // takes ImmLaneIdx4
        public static Vector128<long>   LoadScalarAndInsert(long*   address, Vector128<long>   vector, byte index); // takes ImmLaneIdx2
        public static Vector128<ulong>  LoadScalarAndInsert(ulong*  address, Vector128<ulong>  vector, byte index); // takes ImmLaneIdx2
        public static Vector128<float>  LoadScalarAndInsert(float*  address, Vector128<float>  vector, byte index); // takes ImmLaneIdx4
        public static Vector128<double> LoadScalarAndInsert(double* address, Vector128<double> vector, byte index); // takes ImmLaneIdx2
        public static Vector128<nint>   LoadScalarAndInsert(nint*   address, Vector128<nint>   vector, byte index);
        public static Vector128<nuint>  LoadScalarAndInsert(nuint*  address, Vector128<nuint>  vector, byte index);

        public static Vector128<short>  LoadWideningVector128(sbyte*  address);  // takes ImmLaneIdx16
        public static Vector128<ushort> LoadWideningVector128(byte*   address);  // takes ImmLaneIdx16
        public static Vector128<int>    LoadWideningVector128(short*  address);  // takes ImmLaneIdx8
        public static Vector128<uint>   LoadWideningVector128(ushort* address);  // takes ImmLaneIdx8
        public static Vector128<long>   LoadWideningVector128(int*    address);  // takes ImmLaneIdx4
        public static Vector128<ulong>  LoadWideningVector128(uint*   address);  // takes ImmLaneIdx4

        public static void Store(sbyte*  address, Vector128<sbyte>  source);
        public static void Store(byte*   address, Vector128<byte>   source);
        public static void Store(short*  address, Vector128<short>  source);
        public static void Store(ushort* address, Vector128<ushort> source);
        public static void Store(int*    address, Vector128<int>    source);
        public static void Store(uint*   address, Vector128<uint>   source);
        public static void Store(long*   address, Vector128<long>   source);
        public static void Store(ulong*  address, Vector128<ulong>  source);
        public static void Store(float*  address, Vector128<float>  source);
        public static void Store(double* address, Vector128<double> source);
        public static void Store(nint*   address, Vector128<nint>   source);
        public static void Store(nuint*  address, Vector128<nuint>  source);

        public static void StoreSelectedScalar(sbyte*  address, Vector128<sbyte>  source, byte index); // takes ImmLaneIdx16
        public static void StoreSelectedScalar(byte*   address, Vector128<byte>   source, byte index); // takes ImmLaneIdx16
        public static void StoreSelectedScalar(short*  address, Vector128<short>  source, byte index); // takes ImmLaneIdx8
        public static void StoreSelectedScalar(ushort* address, Vector128<ushort> source, byte index); // takes ImmLaneIdx8
        public static void StoreSelectedScalar(int*    address, Vector128<int>    source, byte index); // takes ImmLaneIdx4
        public static void StoreSelectedScalar(uint*   address, Vector128<uint>   source, byte index); // takes ImmLaneIdx4
        public static void StoreSelectedScalar(long*   address, Vector128<long>   source, byte index); // takes ImmLaneIdx2
        public static void StoreSelectedScalar(ulong*  address, Vector128<ulong>  source, byte index); // takes ImmLaneIdx2
        public static void StoreSelectedScalar(float*  address, Vector128<float>  source, byte index); // takes ImmLaneIdx4
        public static void StoreSelectedScalar(double* address, Vector128<double> source, byte index); // takes ImmLaneIdx2
        public static void StoreSelectedScalar(nint*   address, Vector128<nint>   source, byte index);
        public static void StoreSelectedScalar(nuint*  address, Vector128<nuint>  source, byte index);

        // Floating-point sign bit operations

        public static Vector128<float>  Negate(Vector128<float>  value);
        public static Vector128<double> Negate(Vector128<double> value);

        public static Vector128<float>  Abs(Vector128<float>  value);
        public static Vector128<double> Abs(Vector128<double> value);

        // Floating-point min and max

        public static Vector128<float>  Min(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Min(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  Max(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Max(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  PseudoMin(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> PseudoMin(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  PseudoMax(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> PseudoMax(Vector128<double> left, Vector128<double> right);

        // Floating-point arithmetic

        public static Vector128<float>  Add(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Add(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  Subtract(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Subtract(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  Divide(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Divide(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  Multiply(Vector128<float>  left, Vector128<float>  right);
        public static Vector128<double> Multiply(Vector128<double> left, Vector128<double> right);

        public static Vector128<float>  Sqrt(Vector128<float>  value);
        public static Vector128<double> Sqrt(Vector128<double> value);

        public static Vector128<float>  Ceiling(Vector128<float>  value);
        public static Vector128<double> Ceiling(Vector128<double> value);

        public static Vector128<float>  Floor(Vector128<float>  value);
        public static Vector128<double> Floor(Vector128<double> value);

        public static Vector128<float>  Truncate(Vector128<float>  value);
        public static Vector128<double> Truncate(Vector128<double> value);

        public static Vector128<float>  RoundToNearest(Vector128<float>  value);
        public static Vector128<double> RoundToNearest(Vector128<double> value);

        // Conversions

        public static Vector128<float> ConvertToSingle(Vector128<int> value);
        public static Vector128<float> ConvertToSingle(Vector128<uint> value);
        public static Vector128<float> ConvertToSingle(Vector128<double> value);

        public static Vector128<double> ConvertToDoubleLower(Vector128<int> value);
        public static Vector128<double> ConvertToDoubleLower(Vector128<uint> value);
        public static Vector128<double> ConvertToDoubleLower(Vector128<float> value);

        public static Vector128<int>  ConvertToInt32Saturate(Vector128<float> value);
        public static Vector128<uint> ConvertToInt32Saturate(Vector128<float> value);

        public static Vector128<int>  ConvertToInt32Saturate(Vector128<double> value);
        public static Vector128<uint> ConvertToInt32Saturate(Vector128<double> value);

        public static Vector128<sbyte>  ConvertNarrowingSaturate(Vector128<short>  lower, Vector128<short>  upper);
        public static Vector128<byte>   ConvertNarrowingSaturate(Vector128<ushort> lower, Vector128<ushort> upper);
        public static Vector128<short>  ConvertNarrowingSaturate(Vector128<int>    lower, Vector128<int>    upper);
        public static Vector128<ushort> ConvertNarrowingSaturate(Vector128<uint>   lower, Vector128<uint>   upper);
        public static Vector128<int>    ConvertNarrowingSaturate(Vector128<long>   lower, Vector128<long>   upper);
        public static Vector128<uint>   ConvertNarrowingSaturate(Vector128<ulong>  lower, Vector128<ulong>  upper);

        public static Vector128<short>  SignExtendWideningLower(Vector128<sbyte>  value);
        public static Vector128<ushort> SignExtendWideningLower(Vector128<byte>   value);
        public static Vector128<int>    SignExtendWideningLower(Vector128<short>  value);
        public static Vector128<uint>   SignExtendWideningLower(Vector128<ushort> value);
        public static Vector128<long>   SignExtendWideningLower(Vector128<int>    value);
        public static Vector128<ulong>  SignExtendWideningLower(Vector128<uint>   value);

        public static Vector128<short>  SignExtendWideningUpper(Vector128<sbyte>  value);
        public static Vector128<ushort> SignExtendWideningUpper(Vector128<byte>   value);
        public static Vector128<int>    SignExtendWideningUpper(Vector128<short>  value);
        public static Vector128<uint>   SignExtendWideningUpper(Vector128<ushort> value);
        public static Vector128<long>   SignExtendWideningUpper(Vector128<int>    value);
        public static Vector128<ulong>  SignExtendWideningUpper(Vector128<uint>   value);

        public static Vector128<short>  ZeroExtendWideningLower(Vector128<sbyte>  value);
        public static Vector128<ushort> ZeroExtendWideningLower(Vector128<byte>   value);
        public static Vector128<int>    ZeroExtendWideningLower(Vector128<short>  value);
        public static Vector128<uint>   ZeroExtendWideningLower(Vector128<ushort> value);
        public static Vector128<long>   ZeroExtendWideningLower(Vector128<int>    value);
        public static Vector128<ulong>  ZeroExtendWideningLower(Vector128<uint>   value);

        public static Vector128<short>  ZeroExtendWideningUpper(Vector128<sbyte>  value);
        public static Vector128<ushort> ZeroExtendWideningUpper(Vector128<byte>   value);
        public static Vector128<int>    ZeroExtendWideningUpper(Vector128<short>  value);
        public static Vector128<uint>   ZeroExtendWideningUpper(Vector128<ushort> value);
        public static Vector128<long>   ZeroExtendWideningUpper(Vector128<int>    value);
        public static Vector128<ulong>  ZeroExtendWideningUpper(Vector128<uint>   value);
    }
}
```

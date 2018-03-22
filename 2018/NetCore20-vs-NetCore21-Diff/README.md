# API diff between .NET Core 2.0 and .NET Core 2.1

To the best of my knowledge this is an accureate diff between .NET Core 2.0 and
.NET Core 2.1, excluding ASP.NET Core as well as the Windows Compaptibility
pack.

Sources:

* **.NET Core 2.0**. `\\fxcore\platforms\ApiCatalog\.NET Core 2.0`. This folder
  was created based on the `/reference` arguments passed to `csc.exe` when
  building a .NET Core 2.0 console app.
* **.NET Core 2.1**. `\\cpvsbuild\drops\DotNetCore\DotNet-CoreFx-APICatalog-LayoutDrop\20180322-01\ApiCatalog\ref\microsoft.netcore.app`
  This is our live build drop for .NET Core.

## Action Items

```diff
 namespace System {
-    public class ArgumentException : SystemException, ISerializable {
+    public class ArgumentException : SystemException {
     }
-    public class ArgumentOutOfRangeException : ArgumentException, ISerializable {
+    public class ArgumentOutOfRangeException : ArgumentException {
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct ArraySegment<T> : ICollection<T>, IEnumerable, IEnumerable<T>, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
-        void System.Collections.Generic.ICollection<T>.CopyTo(T[] array, int arrayIndex);
     }
     public static class BitConverter {
+        public static bool ToBoolean(ReadOnlySpan<byte> value);
+        public static char ToChar(ReadOnlySpan<byte> value);
+        public static double ToDouble(ReadOnlySpan<byte> value);
+        public static short ToInt16(ReadOnlySpan<byte> value);
+        public static int ToInt32(ReadOnlySpan<byte> value);
+        public static long ToInt64(ReadOnlySpan<byte> value);
+        public static float ToSingle(ReadOnlySpan<byte> value);
+        public static ushort ToUInt16(ReadOnlySpan<byte> value);
+        public static uint ToUInt32(ReadOnlySpan<byte> value);
+        public static ulong ToUInt64(ReadOnlySpan<byte> value);
+        public static bool TryWriteBytes(Span<byte> destination, char value);
+        public static bool TryWriteBytes(Span<byte> destination, short value);
+        public static bool TryWriteBytes(Span<byte> destination, int value);
+        public static bool TryWriteBytes(Span<byte> destination, long value);
+        public static bool TryWriteBytes(Span<byte> destination, ushort value);
+        public static bool TryWriteBytes(Span<byte> destination, uint value);
+        public static bool TryWriteBytes(Span<byte> destination, ulong value);
+        public static bool TryWriteBytes(Span<byte> destination, float value);
+        public static bool TryWriteBytes(Span<byte> destination, double value);
+        public static bool TryWriteBytes(Span<byte> destination, bool value);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Boolean : IComparable, IComparable<bool>, IConvertible, IEquatable<bool> {
+        public static Boolean Parse(ReadOnlySpan<char> value);
+        public Boolean TryFormat(Span<char> destination, out int charsWritten);
+        public static Boolean TryParse(ReadOnlySpan<char> value, out Boolean result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Byte : IComparable, IComparable<byte>, IConvertible, IEquatable<byte>, IFormattable {
+        public static Byte Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Byte result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Byte result);
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct ConsoleKeyInfo {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ConsoleKeyInfo {
     }
     public static class Convert {
+        public static string ToBase64String(ReadOnlySpan<byte> bytes, Base64FormattingOptions options=(Base64FormattingOptions)(0));
+        public static bool TryFromBase64Chars(ReadOnlySpan<char> chars, Span<byte> bytes, out int bytesWritten);
+        public static bool TryFromBase64String(string s, Span<byte> bytes, out int bytesWritten);
+        public static bool TryToBase64Chars(ReadOnlySpan<byte> bytes, Span<char> chars, out int charsWritten, Base64FormattingOptions options=(Base64FormattingOptions)(0));
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct DateTime : IComparable, IComparable<DateTime>, IConvertible, IEquatable<DateTime>, IFormattable, ISerializable {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct DateTime : IComparable, IComparable<DateTime>, IConvertible, IEquatable<DateTime>, IFormattable, ISerializable {
+        public static readonly DateTime UnixEpoch;
+        public static DateTime Parse(ReadOnlySpan<char> s, IFormatProvider provider=null, DateTimeStyles styles=(DateTimeStyles)(0));
+        public static DateTime ParseExact(ReadOnlySpan<char> s, string[] formats, IFormatProvider provider, DateTimeStyles style=(DateTimeStyles)(0));
+        public static DateTime ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider provider, DateTimeStyles style=(DateTimeStyles)(0));
-        int System.IComparable.CompareTo(object value);
-        TypeCode System.IConvertible.GetTypeCode();
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out DateTime result);
+        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider provider, DateTimeStyles styles, out DateTime result);
+        public static bool TryParseExact(ReadOnlySpan<char> s, string[] formats, IFormatProvider provider, DateTimeStyles style, out DateTime result);
+        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider provider, DateTimeStyles style, out DateTime result);
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct DateTimeOffset : IComparable, IComparable<DateTimeOffset>, IDeserializationCallback, IEquatable<DateTimeOffset>, IFormattable, ISerializable {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct DateTimeOffset : IComparable, IComparable<DateTimeOffset>, IDeserializationCallback, IEquatable<DateTimeOffset>, IFormattable, ISerializable {
+        public static readonly DateTimeOffset UnixEpoch;
+        public static DateTimeOffset Parse(ReadOnlySpan<char> input, IFormatProvider formatProvider=null, DateTimeStyles styles=(DateTimeStyles)(0));
+        public static DateTimeOffset ParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider formatProvider, DateTimeStyles styles=(DateTimeStyles)(0));
+        public static DateTimeOffset ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider formatProvider, DateTimeStyles styles=(DateTimeStyles)(0));
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider formatProvider=null);
+        public static bool TryParse(ReadOnlySpan<char> input, out DateTimeOffset result);
+        public static bool TryParse(ReadOnlySpan<char> input, IFormatProvider formatProvider, DateTimeStyles styles, out DateTimeOffset result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider formatProvider, DateTimeStyles styles, out DateTimeOffset result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider formatProvider, DateTimeStyles styles, out DateTimeOffset result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Decimal : IComparable, IComparable<decimal>, IConvertible, IDeserializationCallback, IEquatable<decimal>, IFormattable {
+        public static Decimal Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Decimal result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Decimal result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Double : IComparable, IComparable<double>, IConvertible, IEquatable<double>, IFormattable {
+        public static bool IsFinite(Double d);
+        public static bool IsNegative(Double d);
+        public static bool IsNormal(Double d);
+        public static bool IsSubnormal(Double d);
+        public static Double Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Double result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Double result);
     }
-    public class DuplicateWaitObjectException : ArgumentException, ISerializable {
+    public class DuplicateWaitObjectException : ArgumentException {
     }
     public abstract class Enum : ValueType, IComparable, IConvertible, IFormattable {
-        TypeCode System.IConvertible.GetTypeCode();
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Guid : IComparable, IComparable<Guid>, IEquatable<Guid>, IFormattable {
+        public Guid(ReadOnlySpan<byte> b);
+        public static Guid Parse(ReadOnlySpan<char> input);
+        public static Guid ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>));
+        public static bool TryParse(ReadOnlySpan<char> input, out Guid result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, out Guid result);
+        public bool TryWriteBytes(Span<byte> destination);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct HashCode {
+        public void Add<T>(T value);
+        public void Add<T>(T value, IEqualityComparer<T> comparer);
+        public static int Combine<T1>(T1 value1);
+        public static int Combine<T1, T2>(T1 value1, T2 value2);
+        public static int Combine<T1, T2, T3>(T1 value1, T2 value2, T3 value3);
+        public static int Combine<T1, T2, T3, T4>(T1 value1, T2 value2, T3 value3, T4 value4);
+        public static int Combine<T1, T2, T3, T4, T5>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5);
+        public static int Combine<T1, T2, T3, T4, T5, T6>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6);
+        public static int Combine<T1, T2, T3, T4, T5, T6, T7>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6, T7 value7);
+        public static int Combine<T1, T2, T3, T4, T5, T6, T7, T8>(T1 value1, T2 value2, T3 value3, T4 value4, T5 value5, T6 value6, T7 value7, T8 value8);
+        public override bool Equals(object obj);
+        public override int GetHashCode();
+        public int ToHashCode();
+    }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Int16 : IComparable, IComparable<short>, IConvertible, IEquatable<short>, IFormattable {
+        public static Int16 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Int16 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Int16 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Int32 : IComparable, IComparable<int>, IConvertible, IEquatable<int>, IFormattable {
+        public static Int32 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out Int32 charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Int32 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Int32 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Int64 : IComparable, IComparable<long>, IConvertible, IEquatable<long>, IFormattable {
+        public static Int64 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Int64 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Int64 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct IntPtr : IEquatable<IntPtr>, ISerializable {
-        bool System.IEquatable<System.IntPtr>.Equals(IntPtr other);
+        bool System.IEquatable<System.IntPtr>.Equals(IntPtr value);
     }
     public static class Math {
+        public static double Acosh(double d);
+        public static double Asinh(double d);
+        public static double Atanh(double d);
+        public static double Cbrt(double d);
     }
     public static class MathF {
+        public static float Acosh(float x);
+        public static float Asinh(float x);
+        public static float Atanh(float x);
+        public static float Cbrt(float x);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Memory<T> {
+        public Memory(T[] array);
+        public Memory(T[] array, int start, int length);
+        public static Memory<T> Empty { get; }
+        public bool IsEmpty { get; }
+        public int Length { get; }
+        public Span<T> Span { get; }
+        public void CopyTo(Memory<T> destination);
+        public bool Equals(Memory<T> other);
+        public override bool Equals(object obj);
+        public override int GetHashCode();
+        public static implicit operator Memory<T> (ArraySegment<T> arraySegment);
+        public static implicit operator ReadOnlyMemory<T> (Memory<T> memory);
+        public static implicit operator Memory<T> (T[] array);
+        public MemoryHandle Retain(bool pin=false);
+        public Memory<T> Slice(int start);
+        public Memory<T> Slice(int start, int length);
+        public T[] ToArray();
+        public bool TryCopyTo(Memory<T> destination);
+    }
+    public static class MemoryExtensions {
+        public static Span<byte> AsBytes<T>(this Span<T> span) where T : struct;
+        public static ReadOnlySpan<byte> AsBytes<T>(this ReadOnlySpan<T> span) where T : struct;
+        public static Memory<T> AsMemory<T>(this T[] array);
+        public static Memory<T> AsMemory<T>(this T[] array, int start);
+        public static Memory<T> AsMemory<T>(this ArraySegment<T> segment);
+        public static Memory<T> AsMemory<T>(this T[] array, int start, int length);
+        public static Memory<T> AsMemory<T>(this ArraySegment<T> segment, int start);
+        public static Memory<T> AsMemory<T>(this ArraySegment<T> segment, int start, int length);
+        public static ReadOnlyMemory<char> AsMemory(this string text);
+        public static ReadOnlyMemory<char> AsMemory(this string text, int start);
+        public static ReadOnlyMemory<char> AsMemory(this string text, int start, int length);
+        public static Span<T> AsSpan<T>(this T[] array);
+        public static Span<T> AsSpan<T>(this T[] array, int start);
+        public static Span<T> AsSpan<T>(this ArraySegment<T> segment);
+        public static Span<T> AsSpan<T>(this T[] array, int start, int length);
+        public static Span<T> AsSpan<T>(this ArraySegment<T> segment, int start);
+        public static Span<T> AsSpan<T>(this ArraySegment<T> segment, int start, int length);
+        public static ReadOnlySpan<char> AsSpan(this string text);
+        public static ReadOnlySpan<char> AsSpan(this string text, int start);
+        public static ReadOnlySpan<char> AsSpan(this string text, int start, int length);
+        public static int BinarySearch<T>(this Span<T> span, IComparable<T> comparable);
+        public static int BinarySearch<T>(this ReadOnlySpan<T> span, IComparable<T> comparable);
+        public static int BinarySearch<T, TComparer>(this Span<T> span, T value, TComparer comparer) where TComparer : IComparer<T>;
+        public static int BinarySearch<T, TComparable>(this Span<T> span, TComparable comparable) where TComparable : IComparable<T>;
+        public static int BinarySearch<T, TComparer>(this ReadOnlySpan<T> span, T value, TComparer comparer) where TComparer : IComparer<T>;
+        public static int BinarySearch<T, TComparable>(this ReadOnlySpan<T> span, TComparable comparable) where TComparable : IComparable<T>;
+        public static int CompareTo(this ReadOnlySpan<char> span, ReadOnlySpan<char> other, StringComparison comparisonType);
+        public static bool Contains(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
+        public static void CopyTo<T>(this T[] source, Span<T> destination);
+        public static void CopyTo<T>(this T[] source, Memory<T> destination);
+        public static bool EndsWith<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static bool EndsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static bool EndsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
+        public static bool Equals(this ReadOnlySpan<char> span, ReadOnlySpan<char> other, StringComparison comparisonType);
+        public static int IndexOf<T>(this Span<T> span, T value) where T : IEquatable<T>;
+        public static int IndexOf<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
+        public static int IndexOf<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static int IndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static int IndexOf(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
+        public static int IndexOfAny<T>(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
+        public static int IndexOfAny<T>(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
+        public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
+        public static int IndexOfAny<T>(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
+        public static int IndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
+        public static int IndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
+        public static bool IsWhiteSpace(this ReadOnlySpan<char> span);
+        public static int LastIndexOf<T>(this Span<T> span, T value) where T : IEquatable<T>;
+        public static int LastIndexOf<T>(this ReadOnlySpan<T> span, T value) where T : IEquatable<T>;
+        public static int LastIndexOf<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static int LastIndexOf<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this Span<T> span, T value0, T value1) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this Span<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this Span<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, T value0, T value1, T value2) where T : IEquatable<T>;
+        public static int LastIndexOfAny<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> values) where T : IEquatable<T>;
+        public static bool Overlaps<T>(this Span<T> span, ReadOnlySpan<T> other);
+        public static bool Overlaps<T>(this Span<T> span, ReadOnlySpan<T> other, out int elementOffset);
+        public static bool Overlaps<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other);
+        public static bool Overlaps<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other, out int elementOffset);
+        public static void Reverse<T>(this Span<T> span);
+        public static int SequenceCompareTo<T>(this Span<T> span, ReadOnlySpan<T> other) where T : IComparable<T>;
+        public static int SequenceCompareTo<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T : IComparable<T>;
+        public static bool SequenceEqual<T>(this Span<T> span, ReadOnlySpan<T> other) where T : IEquatable<T>;
+        public static bool SequenceEqual<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> other) where T : IEquatable<T>;
+        public static bool StartsWith<T>(this Span<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static bool StartsWith<T>(this ReadOnlySpan<T> span, ReadOnlySpan<T> value) where T : IEquatable<T>;
+        public static bool StartsWith(this ReadOnlySpan<char> span, ReadOnlySpan<char> value, StringComparison comparisonType);
+        public static int ToLower(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
+        public static int ToLowerInvariant(this ReadOnlySpan<char> source, Span<char> destination);
+        public static int ToUpper(this ReadOnlySpan<char> source, Span<char> destination, CultureInfo culture);
+        public static int ToUpperInvariant(this ReadOnlySpan<char> source, Span<char> destination);
+        public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span);
+        public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, char trimChar);
+        public static ReadOnlySpan<char> Trim(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
+        public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span);
+        public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, char trimChar);
+        public static ReadOnlySpan<char> TrimEnd(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
+        public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span);
+        public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, char trimChar);
+        public static ReadOnlySpan<char> TrimStart(this ReadOnlySpan<char> span, ReadOnlySpan<char> trimChars);
+    }
-    public class MissingMethodException : MissingMemberException, ISerializable {
+    public class MissingMethodException : MissingMemberException {
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct ModuleHandle {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ModuleHandle {
     }
     public class Random {
+        public virtual void NextBytes(Span<byte> buffer);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ReadOnlyMemory<T> {
+        public ReadOnlyMemory(T[] array);
+        public ReadOnlyMemory(T[] array, int start, int length);
+        public static ReadOnlyMemory<T> Empty { get; }
+        public bool IsEmpty { get; }
+        public int Length { get; }
+        public ReadOnlySpan<T> Span { get; }
+        public void CopyTo(Memory<T> destination);
+        public override bool Equals(object obj);
+        public bool Equals(ReadOnlyMemory<T> other);
+        public override int GetHashCode();
+        public static implicit operator ReadOnlyMemory<T> (ArraySegment<T> arraySegment);
+        public static implicit operator ReadOnlyMemory<T> (T[] array);
+        public MemoryHandle Retain(bool pin=false);
+        public ReadOnlyMemory<T> Slice(int start);
+        public ReadOnlyMemory<T> Slice(int start, int length);
+        public T[] ToArray();
+        public override string ToString();
+        public bool TryCopyTo(Memory<T> destination);
+    }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ReadOnlySpan<T> {
+        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+        public struct Enumerator {
+            public ref T Current { get; }
+            public bool MoveNext();
+        }
+        public ReadOnlySpan(T[] array);
+        public ReadOnlySpan(T[] array, int start, int length);
+        public unsafe ReadOnlySpan(void* pointer, int length);
+        public static ReadOnlySpan<T> Empty { get; }
+        public bool IsEmpty { get; }
+        public ref T this[int index] { get; }
+        public int Length { get; }
+        public void CopyTo(Span<T> destination);
+        public override bool Equals(object obj);
+        public ReadOnlySpan<T>.Enumerator GetEnumerator();
+        public override int GetHashCode();
+        public static bool operator ==(ReadOnlySpan<T> left, ReadOnlySpan<T> right);
+        public static implicit operator ReadOnlySpan<T> (ArraySegment<T> arraySegment);
+        public static implicit operator ReadOnlySpan<T> (T[] array);
+        public static bool operator !=(ReadOnlySpan<T> left, ReadOnlySpan<T> right);
+        public ReadOnlySpan<T> Slice(int start);
+        public ReadOnlySpan<T> Slice(int start, int length);
+        public T[] ToArray();
+        public override string ToString();
+        public bool TryCopyTo(Span<T> destination);
+    }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct SByte : IComparable, IComparable<sbyte>, IConvertible, IEquatable<sbyte>, IFormattable {
+        public static SByte Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out SByte result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out SByte result);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct SequencePosition : IEquatable<SequencePosition> {
+        public SequencePosition(object segment, int index);
+        public override bool Equals(object obj);
+        public bool Equals(SequencePosition other);
+        public override int GetHashCode();
+        public int GetInteger();
+        public object GetObject();
+        public static bool operator ==(SequencePosition left, SequencePosition right);
+        public static bool operator !=(SequencePosition left, SequencePosition right);
+        public override string ToString();
+    }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Single : IComparable, IComparable<float>, IConvertible, IEquatable<float>, IFormattable {
+        public static bool IsFinite(Single f);
+        public static bool IsNegative(Single f);
+        public static bool IsNormal(Single f);
+        public static bool IsSubnormal(Single f);
+        public static Single Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out Single result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out Single result);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Span<T> {
+        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+        public struct Enumerator {
+            public ref T Current { get; }
+            public bool MoveNext();
+        }
+        public Span(T[] array);
+        public Span(T[] array, int start, int length);
+        public unsafe Span(void* pointer, int length);
+        public static Span<T> Empty { get; }
+        public bool IsEmpty { get; }
+        public ref T this[int index] { get; }
+        public int Length { get; }
+        public void Clear();
+        public void CopyTo(Span<T> destination);
+        public override bool Equals(object obj);
+        public void Fill(T value);
+        public Span<T>.Enumerator GetEnumerator();
+        public override int GetHashCode();
+        public static bool operator ==(Span<T> left, Span<T> right);
+        public static implicit operator Span<T> (ArraySegment<T> arraySegment);
+        public static implicit operator ReadOnlySpan<T> (Span<T> span);
+        public static implicit operator Span<T> (T[] array);
+        public static bool operator !=(Span<T> left, Span<T> right);
+        public Span<T> Slice(int start);
+        public Span<T> Slice(int start, int length);
+        public T[] ToArray();
+        public override string ToString();
+        public bool TryCopyTo(Span<T> destination);
+    }
     public sealed class String : ICloneable, IComparable, IComparable<string>, IConvertible, IEnumerable, IEnumerable<char>, IEquatable<string> {
+        public String(ReadOnlySpan<char> value);
+        public bool Contains(char value);
+        public bool Contains(char value, StringComparison comparisonType);
+        public bool Contains(String value, StringComparison comparisonType);
+        public static String Create<TState>(int length, TState state, SpanAction<char, TState> action);
+        public int IndexOf(char value, StringComparison comparisonType);
+        public static implicit operator ReadOnlySpan<char> (String value);
     }
     public abstract class StringComparer : IComparer, IComparer<string>, IEqualityComparer, IEqualityComparer<string> {
+        public static StringComparer Create(CultureInfo culture, CompareOptions options);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct TimeSpan : IComparable, IComparable<TimeSpan>, IEquatable<TimeSpan>, IFormattable {
+        public static TimeSpan Parse(ReadOnlySpan<char> input, IFormatProvider formatProvider=null);
+        public static TimeSpan ParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider formatProvider, TimeSpanStyles styles=(TimeSpanStyles)(0));
+        public static TimeSpan ParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider formatProvider, TimeSpanStyles styles=(TimeSpanStyles)(0));
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider formatProvider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out TimeSpan result);
+        public static bool TryParse(ReadOnlySpan<char> input, IFormatProvider formatProvider, out TimeSpan result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider formatProvider, out TimeSpan result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider formatProvider, out TimeSpan result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, string[] formats, IFormatProvider formatProvider, TimeSpanStyles styles, out TimeSpan result);
+        public static bool TryParseExact(ReadOnlySpan<char> input, ReadOnlySpan<char> format, IFormatProvider formatProvider, TimeSpanStyles styles, out TimeSpan result);
     }
     public abstract class Type : MemberInfo, IReflect {
+        public virtual bool IsByRefLike { get; }
+        public virtual bool IsGenericMethodParameter { get; }
+        public virtual bool IsGenericTypeParameter { get; }
+        public virtual bool IsSignatureType { get; }
+        public MethodInfo GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
+        public MethodInfo GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
+        public MethodInfo GetMethod(string name, int genericParameterCount, Type[] types);
+        public MethodInfo GetMethod(string name, int genericParameterCount, Type[] types, ParameterModifier[] modifiers);
+        protected virtual MethodInfo GetMethodImpl(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
+        public static Type MakeGenericMethodParameter(int position);
     }
-    public class TypeUnloadedException : SystemException, ISerializable {
+    public class TypeUnloadedException : SystemException {
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct UInt16 : IComparable, IComparable<ushort>, IConvertible, IEquatable<ushort>, IFormattable {
+        public static UInt16 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out UInt16 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out UInt16 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct UInt32 : IComparable, IComparable<uint>, IConvertible, IEquatable<uint>, IFormattable {
+        public static UInt32 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out UInt32 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out UInt32 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct UInt64 : IComparable, IComparable<ulong>, IConvertible, IEquatable<ulong>, IFormattable {
+        public static UInt64 Parse(ReadOnlySpan<char> s, NumberStyles style=(NumberStyles)(7), IFormatProvider provider=null);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> s, out UInt64 result);
+        public static bool TryParse(ReadOnlySpan<char> s, NumberStyles style, IFormatProvider provider, out UInt64 result);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct UIntPtr : IEquatable<UIntPtr>, ISerializable {
-        bool System.IEquatable<System.UIntPtr>.Equals(UIntPtr other);
+        bool System.IEquatable<System.UIntPtr>.Equals(UIntPtr value);
     }
-    public struct ValueTuple<T1, T2> : IComparable, IComparable<ValueTuple<T1, T2>>, IEquatable<ValueTuple<T1, T2>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2> : IComparable, IComparable<ValueTuple<T1, T2>>, IEquatable<ValueTuple<T1, T2>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3> : IComparable, IComparable<ValueTuple<T1, T2, T3>>, IEquatable<ValueTuple<T1, T2, T3>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3> : IComparable, IComparable<ValueTuple<T1, T2, T3>>, IEquatable<ValueTuple<T1, T2, T3>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3, T4> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4>>, IEquatable<ValueTuple<T1, T2, T3, T4>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3, T4> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4>>, IEquatable<ValueTuple<T1, T2, T3, T4>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3, T4, T5> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3, T4, T5> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3, T4, T5, T6> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3, T4, T5, T6> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6, T7>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6, T7>>, IStructuralComparable, IStructuralEquatable, ITuple {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6, T7>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6, T7>>, IStructuralComparable, IStructuralEquatable, ITuple {
     }
-    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest>>, IStructuralComparable, IStructuralEquatable, ITuple where TRest : struct {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest> : IComparable, IComparable<ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest>>, IEquatable<ValueTuple<T1, T2, T3, T4, T5, T6, T7, TRest>>, IStructuralComparable, IStructuralEquatable, ITuple where TRest : struct {
     }
     public sealed class Version : ICloneable, IComparable, IComparable<Version>, IEquatable<Version> {
+        public static Version Parse(ReadOnlySpan<char> input);
+        public bool TryFormat(Span<char> destination, out int charsWritten);
+        public bool TryFormat(Span<char> destination, int fieldCount, out int charsWritten);
+        public static bool TryParse(ReadOnlySpan<char> input, out Version result);
     }
 }
 namespace System.Buffers {
+    public static class BuffersExtensions {
+        public static void CopyTo<T>(this ref ReadOnlySequence<T> source, Span<T> destination);
+        public static Nullable<SequencePosition> PositionOf<T>(this ref ReadOnlySequence<T> source, T value) where T : IEquatable<T>;
+        public static T[] ToArray<T>(this ref ReadOnlySequence<T> sequence);
+        public static void Write<T>(this IBufferWriter<T> writer, ReadOnlySpan<T> value);
+    }
+    public interface IBufferWriter<T> {
+        void Advance(int count);
+        Memory<T> GetMemory(int sizeHint=0);
+        Span<T> GetSpan(int sizeHint=0);
+    }
+    public interface IRetainable {
+        bool Release();
+        void Retain();
+    }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct MemoryHandle : IDisposable {
+        public unsafe MemoryHandle(IRetainable owner, void* pointer=null, GCHandle handle=default(GCHandle));
+        public bool HasPointer { get; }
+        public unsafe void* Pointer { get; }
+        public void Dispose();
+    }
+    public abstract class MemoryPool<T> : IDisposable {
+        protected MemoryPool();
+        public abstract int MaxBufferSize { get; }
+        public static MemoryPool<T> Shared { get; }
+        public void Dispose();
+        protected abstract void Dispose(bool disposing);
+        public abstract OwnedMemory<T> Rent(int minBufferSize=-1);
+    }
+    public enum OperationStatus {
+        DestinationTooSmall = 1,
+        Done = 0,
+        InvalidData = 3,
+        NeedMoreData = 2,
+    }
+    public abstract class OwnedMemory<T> : IDisposable, IRetainable {
+        protected OwnedMemory();
+        public abstract bool IsDisposed { get; }
+        protected abstract bool IsRetained { get; }
+        public abstract int Length { get; }
+        public Memory<T> Memory { get; }
+        public abstract Span<T> Span { get; }
+        public void Dispose();
+        protected abstract void Dispose(bool disposing);
+        public abstract MemoryHandle Pin(int byteOffset=0);
+        public abstract bool Release();
+        public abstract void Retain();
+        protected internal abstract bool TryGetArray(out ArraySegment<T> arraySegment);
+    }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ReadOnlySequence<T> {
+        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+        public struct Enumerator {
+            public Enumerator(ref ReadOnlySequence<T> sequence);
+            public ReadOnlyMemory<T> Current { get; }
+            public bool MoveNext();
+        }
+        public static readonly ReadOnlySequence<T> Empty;
+        public ReadOnlySequence(OwnedMemory<T> memory);
+        public ReadOnlySequence(OwnedMemory<T> memory, int start, int length);
+        public ReadOnlySequence(ReadOnlyMemory<T> memory);
+        public ReadOnlySequence(ReadOnlySequenceSegment<T> startSegment, int startIndex, ReadOnlySequenceSegment<T> endSegment, int endIndex);
+        public ReadOnlySequence(T[] array);
+        public ReadOnlySequence(T[] array, int start, int length);
+        public SequencePosition End { get; }
+        public ReadOnlyMemory<T> First { get; }
+        public bool IsEmpty { get; }
+        public bool IsSingleSegment { get; }
+        public long Length { get; }
+        public SequencePosition Start { get; }
+        public ReadOnlySequence<T>.Enumerator GetEnumerator();
+        public SequencePosition GetPosition(long offset);
+        public SequencePosition GetPosition(long offset, SequencePosition origin);
+        public ReadOnlySequence<T> Slice(int start, int length);
+        public ReadOnlySequence<T> Slice(int start, SequencePosition end);
+        public ReadOnlySequence<T> Slice(long start);
+        public ReadOnlySequence<T> Slice(long start, long length);
+        public ReadOnlySequence<T> Slice(long start, SequencePosition end);
+        public ReadOnlySequence<T> Slice(SequencePosition start);
+        public ReadOnlySequence<T> Slice(SequencePosition start, int length);
+        public ReadOnlySequence<T> Slice(SequencePosition start, long length);
+        public ReadOnlySequence<T> Slice(SequencePosition start, SequencePosition end);
+        public override string ToString();
+        public bool TryGet(ref SequencePosition position, out ReadOnlyMemory<T> memory, bool advance=true);
+    }
+    public abstract class ReadOnlySequenceSegment<T> {
+        protected ReadOnlySequenceSegment();
+        public ReadOnlyMemory<T> Memory { get; protected set; }
+        public ReadOnlySequenceSegment<T> Next { get; protected set; }
+        public long RunningIndex { get; protected set; }
+    }
+    public delegate void ReadOnlySpanAction<T, in TArg>(ReadOnlySpan<T> span, TArg arg); {
+        public ReadOnlySpanAction(object @object, IntPtr method);
+        public virtual IAsyncResult BeginInvoke(ReadOnlySpan<T> span, TArg arg, AsyncCallback callback, object @object);
+        public virtual void EndInvoke(IAsyncResult result);
+        public virtual void Invoke(ReadOnlySpan<T> span, TArg arg);
+    }
+    public delegate void SpanAction<T, in TArg>(Span<T> span, TArg arg); {
+        public SpanAction(object @object, IntPtr method);
+        public virtual IAsyncResult BeginInvoke(Span<T> span, TArg arg, AsyncCallback callback, object @object);
+        public virtual void EndInvoke(IAsyncResult result);
+        public virtual void Invoke(Span<T> span, TArg arg);
+    }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct StandardFormat : IEquatable<StandardFormat> {
+        public const byte MaxPrecision = (byte)99;
+        public const byte NoPrecision = (byte)255;
+        public StandardFormat(char symbol, byte precision=(byte)255);
+        public bool HasPrecision { get; }
+        public bool IsDefault { get; }
+        public byte Precision { get; }
+        public char Symbol { get; }
+        public override bool Equals(object obj);
+        public bool Equals(StandardFormat other);
+        public override int GetHashCode();
+        public static bool operator ==(StandardFormat left, StandardFormat right);
+        public static implicit operator StandardFormat (char symbol);
+        public static bool operator !=(StandardFormat left, StandardFormat right);
+        public static StandardFormat Parse(ReadOnlySpan<char> format);
+        public static StandardFormat Parse(string format);
+        public override string ToString();
+    }
 }
+namespace System.Buffers.Binary {
+    public static class BinaryPrimitives {
+        public static short ReadInt16BigEndian(ReadOnlySpan<byte> source);
+        public static short ReadInt16LittleEndian(ReadOnlySpan<byte> source);
+        public static int ReadInt32BigEndian(ReadOnlySpan<byte> source);
+        public static int ReadInt32LittleEndian(ReadOnlySpan<byte> source);
+        public static long ReadInt64BigEndian(ReadOnlySpan<byte> source);
+        public static long ReadInt64LittleEndian(ReadOnlySpan<byte> source);
+        public static T ReadMachineEndian<T>(ReadOnlySpan<byte> source) where T : struct;
+        public static ushort ReadUInt16BigEndian(ReadOnlySpan<byte> source);
+        public static ushort ReadUInt16LittleEndian(ReadOnlySpan<byte> source);
+        public static uint ReadUInt32BigEndian(ReadOnlySpan<byte> source);
+        public static uint ReadUInt32LittleEndian(ReadOnlySpan<byte> source);
+        public static ulong ReadUInt64BigEndian(ReadOnlySpan<byte> source);
+        public static ulong ReadUInt64LittleEndian(ReadOnlySpan<byte> source);
+        public static byte ReverseEndianness(byte value);
+        public static short ReverseEndianness(short value);
+        public static int ReverseEndianness(int value);
+        public static long ReverseEndianness(long value);
+        public static sbyte ReverseEndianness(sbyte value);
+        public static ushort ReverseEndianness(ushort value);
+        public static uint ReverseEndianness(uint value);
+        public static ulong ReverseEndianness(ulong value);
+        public static bool TryReadInt16BigEndian(ReadOnlySpan<byte> source, out short value);
+        public static bool TryReadInt16LittleEndian(ReadOnlySpan<byte> source, out short value);
+        public static bool TryReadInt32BigEndian(ReadOnlySpan<byte> source, out int value);
+        public static bool TryReadInt32LittleEndian(ReadOnlySpan<byte> source, out int value);
+        public static bool TryReadInt64BigEndian(ReadOnlySpan<byte> source, out long value);
+        public static bool TryReadInt64LittleEndian(ReadOnlySpan<byte> source, out long value);
+        public static bool TryReadMachineEndian<T>(ReadOnlySpan<byte> source, out T value) where T : struct;
+        public static bool TryReadUInt16BigEndian(ReadOnlySpan<byte> source, out ushort value);
+        public static bool TryReadUInt16LittleEndian(ReadOnlySpan<byte> source, out ushort value);
+        public static bool TryReadUInt32BigEndian(ReadOnlySpan<byte> source, out uint value);
+        public static bool TryReadUInt32LittleEndian(ReadOnlySpan<byte> source, out uint value);
+        public static bool TryReadUInt64BigEndian(ReadOnlySpan<byte> source, out ulong value);
+        public static bool TryReadUInt64LittleEndian(ReadOnlySpan<byte> source, out ulong value);
+        public static bool TryWriteInt16BigEndian(Span<byte> destination, short value);
+        public static bool TryWriteInt16LittleEndian(Span<byte> destination, short value);
+        public static bool TryWriteInt32BigEndian(Span<byte> destination, int value);
+        public static bool TryWriteInt32LittleEndian(Span<byte> destination, int value);
+        public static bool TryWriteInt64BigEndian(Span<byte> destination, long value);
+        public static bool TryWriteInt64LittleEndian(Span<byte> destination, long value);
+        public static bool TryWriteMachineEndian<T>(Span<byte> destination, ref T value) where T : struct;
+        public static bool TryWriteUInt16BigEndian(Span<byte> destination, ushort value);
+        public static bool TryWriteUInt16LittleEndian(Span<byte> destination, ushort value);
+        public static bool TryWriteUInt32BigEndian(Span<byte> destination, uint value);
+        public static bool TryWriteUInt32LittleEndian(Span<byte> destination, uint value);
+        public static bool TryWriteUInt64BigEndian(Span<byte> destination, ulong value);
+        public static bool TryWriteUInt64LittleEndian(Span<byte> destination, ulong value);
+        public static void WriteInt16BigEndian(Span<byte> destination, short value);
+        public static void WriteInt16LittleEndian(Span<byte> destination, short value);
+        public static void WriteInt32BigEndian(Span<byte> destination, int value);
+        public static void WriteInt32LittleEndian(Span<byte> destination, int value);
+        public static void WriteInt64BigEndian(Span<byte> destination, long value);
+        public static void WriteInt64LittleEndian(Span<byte> destination, long value);
+        public static void WriteMachineEndian<T>(Span<byte> destination, ref T value) where T : struct;
+        public static void WriteUInt16BigEndian(Span<byte> destination, ushort value);
+        public static void WriteUInt16LittleEndian(Span<byte> destination, ushort value);
+        public static void WriteUInt32BigEndian(Span<byte> destination, uint value);
+        public static void WriteUInt32LittleEndian(Span<byte> destination, uint value);
+        public static void WriteUInt64BigEndian(Span<byte> destination, ulong value);
+        public static void WriteUInt64LittleEndian(Span<byte> destination, ulong value);
+    }
+}
+namespace System.Buffers.Text {
+    public static class Base64 {
+        public static OperationStatus DecodeFromUtf8(ReadOnlySpan<byte> utf8, Span<byte> bytes, out int bytesConsumed, out int bytesWritten, bool isFinalBlock=true);
+        public static OperationStatus DecodeFromUtf8InPlace(Span<byte> buffer, out int bytesWritten);
+        public static OperationStatus EncodeToUtf8(ReadOnlySpan<byte> bytes, Span<byte> utf8, out int bytesConsumed, out int bytesWritten, bool isFinalBlock=true);
+        public static OperationStatus EncodeToUtf8InPlace(Span<byte> buffer, int dataLength, out int bytesWritten);
+        public static int GetMaxDecodedFromUtf8Length(int length);
+        public static int GetMaxEncodedToUtf8Length(int length);
+    }
+    public static class Utf8Formatter {
+        public static bool TryFormat(bool value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(byte value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(DateTime value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(DateTimeOffset value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(decimal value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(double value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(Guid value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(short value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(int value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(long value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(sbyte value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(float value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(TimeSpan value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(ushort value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(uint value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+        public static bool TryFormat(ulong value, Span<byte> destination, out int bytesWritten, StandardFormat format=default(StandardFormat));
+    }
+    public static class Utf8Parser {
+        public static bool TryParse(ReadOnlySpan<byte> source, out Guid value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out byte value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out sbyte value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out short value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out int value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out long value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out float value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out double value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out ushort value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out uint value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out ulong value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out bool value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out decimal value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out DateTime value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out TimeSpan value, out int bytesConsumed, char standardFormat='\0');
+        public static bool TryParse(ReadOnlySpan<byte> source, out DateTimeOffset value, out int bytesConsumed, char standardFormat='\0');
+    }
+}
 namespace System.Collections.Generic {
     public class Dictionary<TKey, TValue> : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDeserializationCallback, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue>, ISerializable {
+        public int EnsureCapacity(int capacity);
+        public void TrimExcess();
+        public void TrimExcess(int capacity);
     }
     public class HashSet<T> : ICollection<T>, IDeserializationCallback, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, ISerializable, ISet<T> {
+        public int EnsureCapacity(int capacity);
     }
-    public class KeyNotFoundException : SystemException, ISerializable {
+    public class KeyNotFoundException : SystemException {
     }
 }
 namespace System.Collections.Immutable {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct ImmutableArray<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IEquatable<ImmutableArray<T>>, IImmutableList<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T>, IStructuralComparable, IStructuralEquatable {
         public sealed class Builder : ICollection<T>, IEnumerable, IEnumerable<T>, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
+            public ref T ItemRef(int index);
         }
+        public ref T ItemRef(int index);
     }
     public sealed class ImmutableList<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IImmutableList<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
         public sealed class Builder : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T> {
+            public ref T ItemRef(int index);
         }
+        public ref T ItemRef(int index);
     }
     public sealed class ImmutableQueue<T> : IEnumerable, IEnumerable<T>, IImmutableQueue<T> {
+        public ref T PeekRef();
     }
     public sealed class ImmutableSortedDictionary<TKey, TValue> : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IImmutableDictionary<TKey, TValue>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue> {
         public sealed class Builder : ICollection, ICollection<KeyValuePair<TKey, TValue>>, IDictionary, IDictionary<TKey, TValue>, IEnumerable, IEnumerable<KeyValuePair<TKey, TValue>>, IReadOnlyCollection<KeyValuePair<TKey, TValue>>, IReadOnlyDictionary<TKey, TValue> {
+            public ref TValue ValueRef(TKey key);
         }
+        public ref TValue ValueRef(TKey key);
     }
     public sealed class ImmutableSortedSet<T> : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IImmutableSet<T>, IList, IList<T>, IReadOnlyCollection<T>, IReadOnlyList<T>, ISet<T> {
         public sealed class Builder : ICollection, ICollection<T>, IEnumerable, IEnumerable<T>, IReadOnlyCollection<T>, ISet<T> {
+            public ref T ItemRef(int index);
         }
+        public ref T ItemRef(int index);
     }
     public sealed class ImmutableStack<T> : IEnumerable, IEnumerable<T>, IImmutableStack<T> {
+        public ref T PeekRef();
     }
 }
 namespace System.ComponentModel.DataAnnotations {
     public class DisplayFormatAttribute : Attribute {
+        public Type NullDisplayTextResourceType { get; set; }
+        public string GetNullDisplayText();
     }
     public class RangeAttribute : ValidationAttribute {
+        public bool ConvertValueInInvariantCulture { get; set; }
+        public bool ParseLimitsInInvariantCulture { get; set; }
     }
 }
 namespace System.Data {
     public sealed class DBConcurrencyException : SystemException {
-        public override void GetObjectData(SerializationInfo si, StreamingContext context);
+        public override void GetObjectData(SerializationInfo info, StreamingContext context);
     }
-    public class PropertyCollection : Hashtable {
+    public class PropertyCollection : Hashtable, ICloneable {
     }
 }
 namespace System.Data.Common {
+    public static class DbProviderFactories {
+        public static DbProviderFactory GetFactory(DataRow providerRow);
+        public static DbProviderFactory GetFactory(DbConnection connection);
+        public static DbProviderFactory GetFactory(string providerInvariantName);
+        public static DataTable GetFactoryClasses();
+        public static IEnumerable<string> GetProviderInvariantNames();
+        public static void RegisterFactory(string providerInvariantName, DbProviderFactory factory);
+        public static void RegisterFactory(string providerInvariantName, string factoryTypeAssemblyQualifiedName);
+        public static void RegisterFactory(string providerInvariantName, Type providerFactoryClass);
+        public static bool TryGetFactory(string providerInvariantName, out DbProviderFactory factory);
+        public static bool UnregisterFactory(string providerInvariantName);
+    }
 }
 namespace System.Diagnostics {
     public sealed class ProcessStartInfo {
+        public Collection<string> ArgumentList { get; }
+        public Encoding StandardInputEncoding { get; set; }
     }
 }
 namespace System.Diagnostics.Tracing {
-    public class EventCounter {
+    public class EventCounter : IDisposable {
+        public void Dispose();
     }
 }
 namespace System.Drawing {
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct Color : IEquatable<Color> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Color : IEquatable<Color> {
+        public bool IsKnownColor { get; }
+        public bool IsSystemColor { get; }
+        public static Color FromKnownColor(KnownColor color);
+        public KnownColor ToKnownColor();
     }
+    public enum KnownColor {
+        ActiveBorder = 1,
+        ActiveCaption = 2,
+        ActiveCaptionText = 3,
+        AliceBlue = 28,
+        AntiqueWhite = 29,
+        AppWorkspace = 4,
+        Aqua = 30,
+        Aquamarine = 31,
+        Azure = 32,
+        Beige = 33,
+        Bisque = 34,
+        Black = 35,
+        BlanchedAlmond = 36,
+        Blue = 37,
+        BlueViolet = 38,
+        Brown = 39,
+        BurlyWood = 40,
+        ButtonFace = 168,
+        ButtonHighlight = 169,
+        ButtonShadow = 170,
+        CadetBlue = 41,
+        Chartreuse = 42,
+        Chocolate = 43,
+        Control = 5,
+        ControlDark = 6,
+        ControlDarkDark = 7,
+        ControlLight = 8,
+        ControlLightLight = 9,
+        ControlText = 10,
+        Coral = 44,
+        CornflowerBlue = 45,
+        Cornsilk = 46,
+        Crimson = 47,
+        Cyan = 48,
+        DarkBlue = 49,
+        DarkCyan = 50,
+        DarkGoldenrod = 51,
+        DarkGray = 52,
+        DarkGreen = 53,
+        DarkKhaki = 54,
+        DarkMagenta = 55,
+        DarkOliveGreen = 56,
+        DarkOrange = 57,
+        DarkOrchid = 58,
+        DarkRed = 59,
+        DarkSalmon = 60,
+        DarkSeaGreen = 61,
+        DarkSlateBlue = 62,
+        DarkSlateGray = 63,
+        DarkTurquoise = 64,
+        DarkViolet = 65,
+        DeepPink = 66,
+        DeepSkyBlue = 67,
+        Desktop = 11,
+        DimGray = 68,
+        DodgerBlue = 69,
+        Firebrick = 70,
+        FloralWhite = 71,
+        ForestGreen = 72,
+        Fuchsia = 73,
+        Gainsboro = 74,
+        GhostWhite = 75,
+        Gold = 76,
+        Goldenrod = 77,
+        GradientActiveCaption = 171,
+        GradientInactiveCaption = 172,
+        Gray = 78,
+        GrayText = 12,
+        Green = 79,
+        GreenYellow = 80,
+        Highlight = 13,
+        HighlightText = 14,
+        Honeydew = 81,
+        HotPink = 82,
+        HotTrack = 15,
+        InactiveBorder = 16,
+        InactiveCaption = 17,
+        InactiveCaptionText = 18,
+        IndianRed = 83,
+        Indigo = 84,
+        Info = 19,
+        InfoText = 20,
+        Ivory = 85,
+        Khaki = 86,
+        Lavender = 87,
+        LavenderBlush = 88,
+        LawnGreen = 89,
+        LemonChiffon = 90,
+        LightBlue = 91,
+        LightCoral = 92,
+        LightCyan = 93,
+        LightGoldenrodYellow = 94,
+        LightGray = 95,
+        LightGreen = 96,
+        LightPink = 97,
+        LightSalmon = 98,
+        LightSeaGreen = 99,
+        LightSkyBlue = 100,
+        LightSlateGray = 101,
+        LightSteelBlue = 102,
+        LightYellow = 103,
+        Lime = 104,
+        LimeGreen = 105,
+        Linen = 106,
+        Magenta = 107,
+        Maroon = 108,
+        MediumAquamarine = 109,
+        MediumBlue = 110,
+        MediumOrchid = 111,
+        MediumPurple = 112,
+        MediumSeaGreen = 113,
+        MediumSlateBlue = 114,
+        MediumSpringGreen = 115,
+        MediumTurquoise = 116,
+        MediumVioletRed = 117,
+        Menu = 21,
+        MenuBar = 173,
+        MenuHighlight = 174,
+        MenuText = 22,
+        MidnightBlue = 118,
+        MintCream = 119,
+        MistyRose = 120,
+        Moccasin = 121,
+        NavajoWhite = 122,
+        Navy = 123,
+        OldLace = 124,
+        Olive = 125,
+        OliveDrab = 126,
+        Orange = 127,
+        OrangeRed = 128,
+        Orchid = 129,
+        PaleGoldenrod = 130,
+        PaleGreen = 131,
+        PaleTurquoise = 132,
+        PaleVioletRed = 133,
+        PapayaWhip = 134,
+        PeachPuff = 135,
+        Peru = 136,
+        Pink = 137,
+        Plum = 138,
+        PowderBlue = 139,
+        Purple = 140,
+        Red = 141,
+        RosyBrown = 142,
+        RoyalBlue = 143,
+        SaddleBrown = 144,
+        Salmon = 145,
+        SandyBrown = 146,
+        ScrollBar = 23,
+        SeaGreen = 147,
+        SeaShell = 148,
+        Sienna = 149,
+        Silver = 150,
+        SkyBlue = 151,
+        SlateBlue = 152,
+        SlateGray = 153,
+        Snow = 154,
+        SpringGreen = 155,
+        SteelBlue = 156,
+        Tan = 157,
+        Teal = 158,
+        Thistle = 159,
+        Tomato = 160,
+        Transparent = 27,
+        Turquoise = 161,
+        Violet = 162,
+        Wheat = 163,
+        White = 164,
+        WhiteSmoke = 165,
+        Window = 24,
+        WindowFrame = 25,
+        WindowText = 26,
+        Yellow = 166,
+        YellowGreen = 167,
+    }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct Point : IEquatable<Point> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Point : IEquatable<Point> {
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct PointF : IEquatable<PointF> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct PointF : IEquatable<PointF> {
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct Rectangle : IEquatable<Rectangle> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Rectangle : IEquatable<Rectangle> {
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct RectangleF : IEquatable<RectangleF> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct RectangleF : IEquatable<RectangleF> {
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct Size : IEquatable<Size> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct Size : IEquatable<Size> {
+        public static Size operator /(Size left, int right);
+        public static SizeF operator /(Size left, float right);
+        public static Size operator *(int left, Size right);
+        public static SizeF operator *(float left, Size right);
+        public static Size operator *(Size left, int right);
+        public static SizeF operator *(Size left, float right);
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct SizeF : IEquatable<SizeF> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct SizeF : IEquatable<SizeF> {
+        public static SizeF operator /(SizeF left, float right);
+        public static SizeF operator *(float left, SizeF right);
+        public static SizeF operator *(SizeF left, float right);
     }
 }
 namespace System.Globalization {
     public static class CharUnicodeInfo {
+        public static UnicodeCategory GetUnicodeCategory(int codePoint);
     }
-    public class CultureNotFoundException : ArgumentException, ISerializable {
+    public class CultureNotFoundException : ArgumentException {
     }
 }
 namespace System.IO {
     public class BinaryReader : IDisposable {
+        public virtual int Read(Span<char> buffer);
+        public virtual int Read(Span<byte> buffer);
     }
     public class BinaryWriter : IDisposable {
+        public virtual void Write(ReadOnlySpan<byte> buffer);
+        public virtual void Write(ReadOnlySpan<char> chars);
     }
     public static class Directory {
+        public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern, EnumerationOptions enumerationOptions);
+        public static IEnumerable<string> EnumerateFiles(string path, string searchPattern, EnumerationOptions enumerationOptions);
+        public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern, EnumerationOptions enumerationOptions);
+        public static string[] GetDirectories(string path, string searchPattern, EnumerationOptions enumerationOptions);
+        public static string[] GetFiles(string path, string searchPattern, EnumerationOptions enumerationOptions);
+        public static string[] GetFileSystemEntries(string path, string searchPattern, EnumerationOptions enumerationOptions);
     }
     public sealed class DirectoryInfo : FileSystemInfo {
+        public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern, EnumerationOptions enumerationOptions);
+        public IEnumerable<FileInfo> EnumerateFiles(string searchPattern, EnumerationOptions enumerationOptions);
+        public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern, EnumerationOptions enumerationOptions);
+        public DirectoryInfo[] GetDirectories(string searchPattern, EnumerationOptions enumerationOptions);
+        public FileInfo[] GetFiles(string searchPattern, EnumerationOptions enumerationOptions);
+        public FileSystemInfo[] GetFileSystemInfos(string searchPattern, EnumerationOptions enumerationOptions);
     }
+    public class EnumerationOptions {
+        public EnumerationOptions();
+        public FileAttributes AttributesToSkip { get; set; }
+        public int BufferSize { get; set; }
+        public bool IgnoreInaccessible { get; set; }
+        public MatchCasing MatchCasing { get; set; }
+        public MatchType MatchType { get; set; }
+        public bool RecurseSubdirectories { get; set; }
+        public bool ReturnSpecialDirectories { get; set; }
+    }
     public class FileStream : Stream {
-        public string Name { get; }
+        public virtual string Name { get; }
-        public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
+        public override IAsyncResult BeginRead(byte[] array, int offset, int numBytes, AsyncCallback callback, object state);
-        public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
+        public override IAsyncResult BeginWrite(byte[] array, int offset, int numBytes, AsyncCallback callback, object state);
     }
+    public enum MatchCasing {
+        CaseInsensitive = 2,
+        CaseSensitive = 1,
+        PlatformDefault = 0,
+    }
+    public enum MatchType {
+        Simple = 0,
+        Win32 = 1,
+    }
     public class MemoryStream : Stream {
+        public override int Read(Span<byte> destination);
+        public override ValueTask<int> ReadAsync(Memory<byte> destination, CancellationToken cancellationToken=default(CancellationToken));
+        public override void Write(ReadOnlySpan<byte> source);
+        public override ValueTask WriteAsync(ReadOnlyMemory<byte> source, CancellationToken cancellationToken=default(CancellationToken));
     }
     public static class Path {
+        public static ReadOnlySpan<char> GetDirectoryName(ReadOnlySpan<char> path);
+        public static ReadOnlySpan<char> GetExtension(ReadOnlySpan<char> path);
+        public static ReadOnlySpan<char> GetFileName(ReadOnlySpan<char> path);
+        public static ReadOnlySpan<char> GetFileNameWithoutExtension(ReadOnlySpan<char> path);
+        public static string GetFullPath(string path, string basePath);
+        public static ReadOnlySpan<char> GetPathRoot(ReadOnlySpan<char> path);
+        public static bool HasExtension(ReadOnlySpan<char> path);
+        public static bool IsPathFullyQualified(ReadOnlySpan<char> path);
+        public static bool IsPathFullyQualified(string path);
+        public static bool IsPathRooted(ReadOnlySpan<char> path);
+        public static string Join(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2);
+        public static string Join(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, ReadOnlySpan<char> path3);
+        public static bool TryJoin(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, Span<char> destination, out int charsWritten);
+        public static bool TryJoin(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, ReadOnlySpan<char> path3, Span<char> destination, out int charsWritten);
     }
     public abstract class Stream : MarshalByRefObject, IDisposable {
+        public Task CopyToAsync(Stream destination, CancellationToken cancellationToken);
+        public virtual int Read(Span<byte> destination);
+        public virtual ValueTask<int> ReadAsync(Memory<byte> destination, CancellationToken cancellationToken=default(CancellationToken));
+        public virtual void Write(ReadOnlySpan<byte> source);
+        public virtual ValueTask WriteAsync(ReadOnlyMemory<byte> source, CancellationToken cancellationToken=default(CancellationToken));
     }
     public class StreamReader : TextReader {
+        public override int Read(Span<char> buffer);
+        public override ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public override int ReadBlock(Span<char> buffer);
+        public override ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public class StreamWriter : TextWriter {
+        public override void Write(ReadOnlySpan<char> buffer);
+        public override Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public override void WriteLine(ReadOnlySpan<char> buffer);
+        public override void WriteLine(string value);
+        public override Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public class StringReader : TextReader {
+        public override int Read(Span<char> buffer);
+        public override ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public override int ReadBlock(Span<char> buffer);
+        public override ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public class StringWriter : TextWriter {
+        public override void Write(ReadOnlySpan<char> buffer);
+        public override Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public override void WriteLine(ReadOnlySpan<char> buffer);
+        public override Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public abstract class TextReader : MarshalByRefObject, IDisposable {
+        public virtual int Read(Span<char> buffer);
+        public virtual ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public virtual int ReadBlock(Span<char> buffer);
+        public virtual ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public abstract class TextWriter : MarshalByRefObject, IDisposable {
+        public virtual void Write(ReadOnlySpan<char> buffer);
+        public virtual Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
+        public virtual void WriteLine(ReadOnlySpan<char> buffer);
+        public virtual Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken=default(CancellationToken));
     }
     public class UnmanagedMemoryStream : Stream {
+        public override int Read(Span<byte> destination);
+        public override void Write(ReadOnlySpan<byte> source);
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct WaitForChangedResult {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct WaitForChangedResult {
     }
 }
 namespace System.IO.Compression {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
+    public struct BrotliDecoder : IDisposable {
+        public OperationStatus Decompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten);
+        public void Dispose();
+        public static bool TryDecompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
+    }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
+    public struct BrotliEncoder : IDisposable {
+        public BrotliEncoder(int quality, int window);
+        public OperationStatus Compress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesConsumed, out int bytesWritten, bool isFinalBlock);
+        public void Dispose();
+        public OperationStatus Flush(Span<byte> destination, out int bytesWritten);
+        public static int GetMaxCompressedLength(int inputSize);
+        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
+        public static bool TryCompress(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten, int quality, int window);
+    }
+    public sealed class BrotliStream : Stream {
+        public BrotliStream(Stream stream, CompressionLevel compressionLevel);
+        public BrotliStream(Stream stream, CompressionLevel compressionLevel, bool leaveOpen);
+        public BrotliStream(Stream stream, CompressionMode mode);
+        public BrotliStream(Stream stream, CompressionMode mode, bool leaveOpen);
+        public Stream BaseStream { get; }
+        public override bool CanRead { get; }
+        public override bool CanSeek { get; }
+        public override bool CanWrite { get; }
+        public override long Length { get; }
+        public override long Position { get; set; }
+        public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback asyncCallback, object asyncState);
+        public override IAsyncResult BeginWrite(byte[] array, int offset, int count, AsyncCallback asyncCallback, object asyncState);
+        public override int EndRead(IAsyncResult asyncResult);
+        public override void EndWrite(IAsyncResult asyncResult);
+        public override void Flush();
+        public override int Read(byte[] array, int offset, int count);
+        public override Task<int> ReadAsync(byte[] array, int offset, int count, CancellationToken cancellationToken);
+        public override long Seek(long offset, SeekOrigin origin);
+        public override void SetLength(long value);
+        public override void Write(byte[] array, int offset, int count);
+        public override Task WriteAsync(byte[] array, int offset, int count, CancellationToken cancellationToken);
+    }
     public class ZipArchiveEntry {
+        public uint Crc32 { get; }
     }
 }
+namespace System.IO.Enumeration {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
+    public struct FileSystemEntry {
+        public FileAttributes Attributes { get; }
+        public DateTimeOffset CreationTimeUtc { get; }
+        public ReadOnlySpan<char> Directory { get; }
+        public ReadOnlySpan<char> FileName { get; }
+        public bool IsDirectory { get; }
+        public bool IsHidden { get; }
+        public DateTimeOffset LastAccessTimeUtc { get; }
+        public DateTimeOffset LastWriteTimeUtc { get; }
+        public long Length { get; }
+        public ReadOnlySpan<char> OriginalRootDirectory { get; }
+        public ReadOnlySpan<char> RootDirectory { get; }
+        public FileSystemInfo ToFileSystemInfo();
+        public string ToFullPath();
+        public string ToSpecifiedFullPath();
+    }
+    public class FileSystemEnumerable<TResult> : IEnumerable, IEnumerable<TResult> {
+        public delegate bool FindPredicate(ref FileSystemEntry entry); {
+            public FindPredicate(object @object, IntPtr method);
+            public virtual IAsyncResult BeginInvoke(ref FileSystemEntry entry, AsyncCallback callback, object @object);
+            public virtual bool EndInvoke(ref FileSystemEntry entry, IAsyncResult result);
+            public virtual bool Invoke(ref FileSystemEntry entry);
+        }
+        public delegate TResult FindTransform(ref FileSystemEntry entry); {
+            public FindTransform(object @object, IntPtr method);
+            public virtual IAsyncResult BeginInvoke(ref FileSystemEntry entry, AsyncCallback callback, object @object);
+            public virtual TResult EndInvoke(ref FileSystemEntry entry, IAsyncResult result);
+            public virtual TResult Invoke(ref FileSystemEntry entry);
+        }
+        public FileSystemEnumerable(string directory, FileSystemEnumerable<TResult>.FindTransform transform, EnumerationOptions options=null);
+        public FileSystemEnumerable<TResult>.FindPredicate ShouldIncludePredicate { get; set; }
+        public FileSystemEnumerable<TResult>.FindPredicate ShouldRecursePredicate { get; set; }
+        public IEnumerator<TResult> GetEnumerator();
+        IEnumerator System.Collections.IEnumerable.GetEnumerator();
+    }
+    public abstract class FileSystemEnumerator<TResult> : CriticalFinalizerObject, IDisposable, IEnumerator, IEnumerator<TResult> {
+        public FileSystemEnumerator(string directory, EnumerationOptions options=null);
+        public TResult Current { get; }
+        object System.Collections.IEnumerator.Current { get; }
+        protected virtual bool ContinueOnError(int error);
+        public void Dispose();
+        protected virtual void Dispose(bool disposing);
+        public bool MoveNext();
+        protected virtual void OnDirectoryFinished(ReadOnlySpan<char> directory);
+        public void Reset();
+        protected virtual bool ShouldIncludeEntry(ref FileSystemEntry entry);
+        protected virtual bool ShouldRecurseIntoEntry(ref FileSystemEntry entry);
+        protected abstract TResult TransformEntry(ref FileSystemEntry entry);
+    }
+    public static class FileSystemName {
+        public static bool MatchesSimpleExpression(ReadOnlySpan<char> expression, ReadOnlySpan<char> name, bool ignoreCase=true);
+        public static bool MatchesWin32Expression(ReadOnlySpan<char> expression, ReadOnlySpan<char> name, bool ignoreCase=true);
+        public static string TranslateWin32Expression(string expression);
+    }
+}
 namespace System.IO.Pipes {
     public enum PipeOptions {
+        CurrentUserOnly = 536870912,
     }
 }
 namespace System.Net {
     public enum HttpStatusCode {
+        AlreadyReported = 208,
+        EarlyHints = 103,
+        FailedDependency = 424,
+        IMUsed = 226,
+        InsufficientStorage = 507,
+        Locked = 423,
+        LoopDetected = 508,
+        MisdirectedRequest = 421,
+        MultiStatus = 207,
+        NetworkAuthenticationRequired = 511,
+        NotExtended = 510,
+        PermanentRedirect = 308,
+        PreconditionRequired = 428,
+        Processing = 102,
+        RequestHeaderFieldsTooLarge = 431,
+        TooManyRequests = 429,
+        UnavailableForLegalReasons = 451,
+        UnprocessableEntity = 422,
+        VariantAlsoNegotiates = 506,
     }
     public class IPAddress {
+        public IPAddress(ReadOnlySpan<byte> address);
+        public IPAddress(ReadOnlySpan<byte> address, long scopeid);
+        public static IPAddress Parse(ReadOnlySpan<char> ipString);
+        public bool TryFormat(Span<char> destination, out int charsWritten);
+        public static bool TryParse(ReadOnlySpan<char> ipString, out IPAddress address);
+        public bool TryWriteBytes(Span<byte> destination, out int bytesWritten);
     }
 }
 namespace System.Net.Http {
     public class HttpClient : HttpMessageInvoker {
+        public Task<HttpResponseMessage> PatchAsync(string requestUri, HttpContent content);
+        public Task<HttpResponseMessage> PatchAsync(string requestUri, HttpContent content, CancellationToken cancellationToken);
+        public Task<HttpResponseMessage> PatchAsync(Uri requestUri, HttpContent content);
+        public Task<HttpResponseMessage> PatchAsync(Uri requestUri, HttpContent content, CancellationToken cancellationToken);
     }
     public class HttpMethod : IEquatable<HttpMethod> {
+        public static HttpMethod Patch { get; }
     }
+    public sealed class ReadOnlyMemoryContent : HttpContent {
+        public ReadOnlyMemoryContent(ReadOnlyMemory<byte> content);
+    }
+    public sealed class SocketsHttpHandler : HttpMessageHandler {
+        public SocketsHttpHandler();
+        public bool AllowAutoRedirect { get; set; }
+        public DecompressionMethods AutomaticDecompression { get; set; }
+        public TimeSpan ConnectTimeout { get; set; }
+        public CookieContainer CookieContainer { get; set; }
+        public ICredentials Credentials { get; set; }
+        public ICredentials DefaultProxyCredentials { get; set; }
+        public TimeSpan Expect100ContinueTimeout { get; set; }
+        public int MaxAutomaticRedirections { get; set; }
+        public int MaxConnectionsPerServer { get; set; }
+        public int MaxResponseDrainSize { get; set; }
+        public int MaxResponseHeadersLength { get; set; }
+        public TimeSpan PooledConnectionIdleTimeout { get; set; }
+        public TimeSpan PooledConnectionLifetime { get; set; }
+        public bool PreAuthenticate { get; set; }
+        public IDictionary<string, object> Properties { get; }
+        public IWebProxy Proxy { get; set; }
+        public TimeSpan ResponseDrainTimeout { get; set; }
+        public SslClientAuthenticationOptions SslOptions { get; set; }
+        public bool UseCookies { get; set; }
+        public bool UseProxy { get; set; }
+    }
 }
 namespace System.Net.Mime {
     public static class MediaTypeNames {
         public static class Application {
+            public const string Json = "application/json";
+            public const string Xml = "application/xml";
         }
     }
 }
 namespace System.Net.Security {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct SslApplicationProtocol : IEquatable<SslApplicationProtocol> {
+        public static readonly SslApplicationProtocol Http11;
+        public static readonly SslApplicationProtocol Http2;
+        public SslApplicationProtocol(byte[] protocol);
+        public SslApplicationProtocol(string protocol);
+        public ReadOnlyMemory<byte> Protocol { get; }
+        public override bool Equals(object obj);
+        public bool Equals(SslApplicationProtocol other);
+        public override int GetHashCode();
+        public static bool operator ==(SslApplicationProtocol left, SslApplicationProtocol right);
+        public static bool operator !=(SslApplicationProtocol left, SslApplicationProtocol right);
+        public override string ToString();
+    }
+    public class SslClientAuthenticationOptions {
+        public SslClientAuthenticationOptions();
+        public bool AllowRenegotiation { get; set; }
+        public List<SslApplicationProtocol> ApplicationProtocols { get; set; }
+        public X509RevocationMode CertificateRevocationCheckMode { get; set; }
+        public X509CertificateCollection ClientCertificates { get; set; }
+        public SslProtocols EnabledSslProtocols { get; set; }
+        public EncryptionPolicy EncryptionPolicy { get; set; }
+        public LocalCertificateSelectionCallback LocalCertificateSelectionCallback { get; set; }
+        public RemoteCertificateValidationCallback RemoteCertificateValidationCallback { get; set; }
+        public string TargetHost { get; set; }
+    }
+    public class SslServerAuthenticationOptions {
+        public SslServerAuthenticationOptions();
+        public bool AllowRenegotiation { get; set; }
+        public List<SslApplicationProtocol> ApplicationProtocols { get; set; }
+        public X509RevocationMode CertificateRevocationCheckMode { get; set; }
+        public bool ClientCertificateRequired { get; set; }
+        public SslProtocols EnabledSslProtocols { get; set; }
+        public EncryptionPolicy EncryptionPolicy { get; set; }
+        public RemoteCertificateValidationCallback RemoteCertificateValidationCallback { get; set; }
+        public X509Certificate ServerCertificate { get; set; }
+    }
     public class SslStream : AuthenticatedStream {
+        public SslApplicationProtocol NegotiatedApplicationProtocol { get; }
+        public Task AuthenticateAsClientAsync(SslClientAuthenticationOptions sslClientAuthenticationOptions, CancellationToken cancellationToken);
+        public Task AuthenticateAsServerAsync(SslServerAuthenticationOptions sslServerAuthenticationOptions, CancellationToken cancellationToken);
     }
 }
 namespace System.Net.Sockets {
     public class Socket : IDisposable {
+        public int Receive(Span<byte> buffer);
+        public int Receive(Span<byte> buffer, SocketFlags socketFlags);
+        public int Receive(Span<byte> buffer, SocketFlags socketFlags, out SocketError errorCode);
+        public int Send(ReadOnlySpan<byte> buffer);
+        public int Send(ReadOnlySpan<byte> buffer, SocketFlags socketFlags);
+        public int Send(ReadOnlySpan<byte> buffer, SocketFlags socketFlags, out SocketError errorCode);
     }
     public class SocketAsyncEventArgs : EventArgs, IDisposable {
+        public Memory<byte> MemoryBuffer { get; }
+        public void SetBuffer(Memory<byte> buffer);
     }
     public static class SocketTaskExtensions {
+        public static ValueTask<int> ReceiveAsync(this Socket socket, Memory<byte> buffer, SocketFlags socketFlags, CancellationToken cancellationToken=default(CancellationToken));
+        public static ValueTask<int> SendAsync(this Socket socket, ReadOnlyMemory<byte> buffer, SocketFlags socketFlags, CancellationToken cancellationToken=default(CancellationToken));
     }
+    public sealed class UnixDomainSocketEndPoint : EndPoint {
+        public UnixDomainSocketEndPoint(string path);
+    }
 }
 namespace System.Net.WebSockets {
     public sealed class ClientWebSocketOptions {
+        public RemoteCertificateValidationCallback RemoteCertificateValidationCallback { get; set; }
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueWebSocketReceiveResult {
+        public ValueWebSocketReceiveResult(int count, WebSocketMessageType messageType, bool endOfMessage);
+        public int Count { get; }
+        public bool EndOfMessage { get; }
+        public WebSocketMessageType MessageType { get; }
+    }
     public abstract class WebSocket : IDisposable {
+        public static WebSocket CreateFromStream(Stream stream, bool isServer, string subProtocol, TimeSpan keepAliveInterval, Memory<byte> buffer=default(Memory<byte>));
+        public virtual ValueTask<ValueWebSocketReceiveResult> ReceiveAsync(Memory<byte> buffer, CancellationToken cancellationToken);
+        public virtual ValueTask SendAsync(ReadOnlyMemory<byte> buffer, WebSocketMessageType messageType, bool endOfMessage, CancellationToken cancellationToken);
     }
 }
 namespace System.Numerics {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct BigInteger : IComparable, IComparable<BigInteger>, IEquatable<BigInteger>, IFormattable {
+        public BigInteger(ReadOnlySpan<byte> value, bool isUnsigned=false, bool isBigEndian=false);
+        public int GetByteCount(bool isUnsigned=false);
+        public static BigInteger Parse(ReadOnlySpan<char> value, NumberStyles style=7, IFormatProvider provider=null);
+        public byte[] ToByteArray(bool isUnsigned=false, bool isBigEndian=false);
+        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format=default(ReadOnlySpan<char>), IFormatProvider provider=null);
+        public static bool TryParse(ReadOnlySpan<char> value, out BigInteger result);
+        public static bool TryParse(ReadOnlySpan<char> value, NumberStyles style, IFormatProvider provider, out BigInteger result);
+        public bool TryWriteBytes(Span<byte> destination, out int bytesWritten, bool isUnsigned=false, bool isBigEndian=false);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct Vector<T> : IEquatable<Vector<T>>, IFormattable where T : struct {
+        public Vector(Span<T> values);
     }
 }
 namespace System.Reflection {
     public abstract class Assembly : ICustomAttributeProvider, ISerializable {
+        public virtual Type[] GetForwardedTypes();
     }
     public enum BindingFlags {
+        DoNotWrapExceptions = 33554432,
     }
     public abstract class MemberInfo : ICustomAttributeProvider {
+        public virtual bool HasSameMetadataDefinitionAs(MemberInfo other);
     }
     public abstract class MethodBase : MemberInfo {
+        public virtual bool IsConstructedGenericMethod { get; }
     }
     public class TypeDelegator : TypeInfo {
+        public override bool IsByRefLike { get; }
+        public override bool IsGenericMethodParameter { get; }
+        public override bool IsGenericTypeParameter { get; }
     }
 }
 namespace System.Reflection.Emit {
     public sealed class EnumBuilder : Type {
+        public override bool IsByRefLike { get; }
     }
     public sealed class GenericTypeParameterBuilder : Type {
+        public override bool IsByRefLike { get; }
     }
     public class ILGenerator {
+        public virtual void EmitCalli(OpCode opcode, CallingConvention unmanagedCallConv, Type returnType, Type[] parameterTypes);
     }
     public sealed class MethodBuilder : MethodInfo {
+        public override bool IsConstructedGenericMethod { get; }
     }
     public sealed class TypeBuilder : Type {
+        public override bool IsByRefLike { get; }
     }
 }
 namespace System.Reflection.Metadata {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct AssemblyDefinition {
+        public AssemblyName GetAssemblyName();
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct AssemblyReference {
+        public AssemblyName GetAssemblyName();
     }
     public sealed class DebugMetadataHeader {
+        public int IdStartOffset { get; }
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct EventAccessors {
+        public ImmutableArray<MethodDefinitionHandle> Others { get; }
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct ModuleDefinition {
+        public CustomAttributeHandleCollection GetCustomAttributes();
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct PropertyAccessors {
+        public ImmutableArray<MethodDefinitionHandle> Others { get; }
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct TypeDefinition {
+        public bool IsNested { get; }
     }
 }
 namespace System.Reflection.Metadata.Ecma335 {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct MethodBodyStreamEncoder {
-        public int AddMethodBody(InstructionEncoder instructionEncoder, int maxStack=8, StandaloneSignatureHandle localVariablesSignature=default(StandaloneSignatureHandle), MethodBodyAttributes attributes=(MethodBodyAttributes)(1));
+        public int AddMethodBody(InstructionEncoder instructionEncoder, int maxStack, StandaloneSignatureHandle localVariablesSignature, MethodBodyAttributes attributes);
+        public int AddMethodBody(InstructionEncoder instructionEncoder, int maxStack=8, StandaloneSignatureHandle localVariablesSignature=default(StandaloneSignatureHandle), MethodBodyAttributes attributes=(MethodBodyAttributes)(1), bool hasDynamicStackAllocation=false);
-        public MethodBodyStreamEncoder.MethodBody AddMethodBody(int codeSize, int maxStack=8, int exceptionRegionCount=0, bool hasSmallExceptionRegions=true, StandaloneSignatureHandle localVariablesSignature=default(StandaloneSignatureHandle), MethodBodyAttributes attributes=(MethodBodyAttributes)(1));
+        public MethodBodyStreamEncoder.MethodBody AddMethodBody(int codeSize, int maxStack, int exceptionRegionCount, bool hasSmallExceptionRegions, StandaloneSignatureHandle localVariablesSignature, MethodBodyAttributes attributes);
+        public MethodBodyStreamEncoder.MethodBody AddMethodBody(int codeSize, int maxStack=8, int exceptionRegionCount=0, bool hasSmallExceptionRegions=true, StandaloneSignatureHandle localVariablesSignature=default(StandaloneSignatureHandle), MethodBodyAttributes attributes=(MethodBodyAttributes)(1), bool hasDynamicStackAllocation=false);
     }
 }
 namespace System.Reflection.PortableExecutable {
     public sealed class DebugDirectoryBuilder {
+        public void AddEntry<TData>(DebugDirectoryEntryType type, uint version, uint stamp, TData data, Action<BlobBuilder, TData> dataSerializer);
+        public void AddEntry(DebugDirectoryEntryType type, uint version, uint stamp);
+        public void AddPdbChecksumEntry(string algorithmName, ImmutableArray<byte> checksum);
     }
     public enum DebugDirectoryEntryType {
+        PdbChecksum = 19,
     }
     public enum Machine : ushort {
+        Arm64 = (ushort)43620,
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct PdbChecksumDebugDirectoryData {
+        public string AlgorithmName { get; }
+        public ImmutableArray<byte> Checksum { get; }
+    }
     public sealed class PEReader : IDisposable {
+        public PdbChecksumDebugDirectoryData ReadPdbChecksumDebugDirectoryData(DebugDirectoryEntry entry);
     }
 }
 namespace System.Runtime.CompilerServices {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct AsyncValueTaskMethodBuilder {
+        public ValueTask Task { get; }
+        public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
+        public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
+        public static AsyncValueTaskMethodBuilder Create();
+        public void SetException(Exception exception);
+        public void SetResult();
+        public void SetStateMachine(IAsyncStateMachine stateMachine);
+        public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
+    }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct AsyncValueTaskMethodBuilder<TResult> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct AsyncValueTaskMethodBuilder<TResult> {
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ConfiguredValueTaskAwaitable {
+        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+        public struct ConfiguredValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
+            public bool IsCompleted { get; }
+            public void GetResult();
+            public void OnCompleted(Action continuation);
+            public void UnsafeOnCompleted(Action continuation);
+        }
+        public ConfiguredValueTaskAwaitable.ConfiguredValueTaskAwaiter GetAwaiter();
+    }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct ConfiguredValueTaskAwaitable<TResult> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ConfiguredValueTaskAwaitable<TResult> {
-        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-        public struct ConfiguredValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
+        [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+        public struct ConfiguredValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
         }
     }
     public static class RuntimeFeature {
+        public const string DefaultImplementationsOfInterfaces = "DefaultImplementationsOfInterfaces";
+        public const string PortablePdb = "PortablePdb";
     }
     public sealed class RuntimeWrappedException : Exception {
+        public RuntimeWrappedException(object thrownObject);
     }
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTaskAwaiter : ICriticalNotifyCompletion, INotifyCompletion {
+        public bool IsCompleted { get; }
+        public void GetResult();
+        public void OnCompleted(Action continuation);
+        public void UnsafeOnCompleted(Action continuation);
+    }
 }
 namespace System.Runtime.InteropServices {
+    public static class MemoryMarshal {
+        public static Memory<T> AsMemory<T>(ReadOnlyMemory<T> memory);
+        public static Span<TTo> Cast<TFrom, TTo>(Span<TFrom> span) where TFrom : struct where TTo : struct;
+        public static ReadOnlySpan<TTo> Cast<TFrom, TTo>(ReadOnlySpan<TFrom> span) where TFrom : struct where TTo : struct;
+        public static ReadOnlySpan<T> CreateReadOnlySpan<T>(ref T reference, int length);
+        public static Span<T> CreateSpan<T>(ref T reference, int length);
+        public static ref T GetReference<T>(Span<T> span);
+        public static ref T GetReference<T>(ReadOnlySpan<T> span);
+        public static IEnumerable<T> ToEnumerable<T>(ReadOnlyMemory<T> memory);
+        public static bool TryGetArray<T>(ReadOnlyMemory<T> memory, out ArraySegment<T> segment);
+        public static bool TryGetOwnedMemory<T, TOwner>(ReadOnlyMemory<T> memory, out TOwner owner) where TOwner : OwnedMemory<T>;
+        public static bool TryGetOwnedMemory<T, TOwner>(ReadOnlyMemory<T> memory, out TOwner owner, out int start, out int length) where TOwner : OwnedMemory<T>;
+        public static bool TryGetString(ReadOnlyMemory<char> memory, out string text, out int start, out int length);
+    }
+    public static class SequenceMarshal {
+        public static bool TryGetArray<T>(ReadOnlySequence<T> sequence, out ArraySegment<T> segment);
+        public static bool TryGetOwnedMemory<T>(ReadOnlySequence<T> sequence, out OwnedMemory<T> ownedMemory, out int start, out int length);
+        public static bool TryGetReadOnlyMemory<T>(ReadOnlySequence<T> sequence, out ReadOnlyMemory<T> memory);
+        public static bool TryGetReadOnlySequenceSegment<T>(ReadOnlySequence<T> sequence, out ReadOnlySequenceSegment<T> startSegment, out int startIndex, out ReadOnlySequenceSegment<T> endSegment, out int endIndex);
+    }
 }
 namespace System.Security.Cryptography {
+    public static class CryptographicOperations {
+        public static bool FixedTimeEquals(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
+        public static void ZeroMemory(Span<byte> buffer);
+    }
     public class CryptoStream : Stream, IDisposable {
+        public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
+        public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
+        public override int EndRead(IAsyncResult asyncResult);
+        public override void EndWrite(IAsyncResult asyncResult);
     }
     public abstract class DSA : AsymmetricAlgorithm {
+        public virtual bool TryCreateSignature(ReadOnlySpan<byte> hash, Span<byte> destination, out int bytesWritten);
+        protected virtual bool TryHashData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, out int bytesWritten);
+        public virtual bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, out int bytesWritten);
+        public virtual bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm);
+        public virtual bool VerifySignature(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature);
     }
+    public abstract class ECDiffieHellman : AsymmetricAlgorithm {
+        protected ECDiffieHellman();
+        public override string KeyExchangeAlgorithm { get; }
+        public abstract ECDiffieHellmanPublicKey PublicKey { get; }
+        public override string SignatureAlgorithm { get; }
+        public static ECDiffieHellman Create();
+        public static ECDiffieHellman Create(ECCurve curve);
+        public static ECDiffieHellman Create(ECParameters parameters);
+        public static ECDiffieHellman Create(string algorithm);
+        public byte[] DeriveKeyFromHash(ECDiffieHellmanPublicKey otherPartyPublicKey, HashAlgorithmName hashAlgorithm);
+        public virtual byte[] DeriveKeyFromHash(ECDiffieHellmanPublicKey otherPartyPublicKey, HashAlgorithmName hashAlgorithm, byte[] secretPrepend, byte[] secretAppend);
+        public byte[] DeriveKeyFromHmac(ECDiffieHellmanPublicKey otherPartyPublicKey, HashAlgorithmName hashAlgorithm, byte[] hmacKey);
+        public virtual byte[] DeriveKeyFromHmac(ECDiffieHellmanPublicKey otherPartyPublicKey, HashAlgorithmName hashAlgorithm, byte[] hmacKey, byte[] secretPrepend, byte[] secretAppend);
+        public virtual byte[] DeriveKeyMaterial(ECDiffieHellmanPublicKey otherPartyPublicKey);
+        public virtual byte[] DeriveKeyTls(ECDiffieHellmanPublicKey otherPartyPublicKey, byte[] prfLabel, byte[] prfSeed);
+        public virtual ECParameters ExportExplicitParameters(bool includePrivateParameters);
+        public virtual ECParameters ExportParameters(bool includePrivateParameters);
+        public override void FromXmlString(string xmlString);
+        public virtual void GenerateKey(ECCurve curve);
+        public virtual void ImportParameters(ECParameters parameters);
+        public override string ToXmlString(bool includePrivateParameters);
+    }
     public abstract class ECDiffieHellmanPublicKey : IDisposable {
+        protected ECDiffieHellmanPublicKey();
+        public virtual ECParameters ExportExplicitParameters();
+        public virtual ECParameters ExportParameters();
     }
     public abstract class ECDsa : AsymmetricAlgorithm {
+        protected virtual bool TryHashData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, out int bytesWritten);
+        public virtual bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, out int bytesWritten);
+        public virtual bool TrySignHash(ReadOnlySpan<byte> hash, Span<byte> destination, out int bytesWritten);
+        public virtual bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm);
+        public virtual bool VerifyHash(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature);
     }
     public abstract class HashAlgorithm : ICryptoTransform, IDisposable {
+        protected virtual void HashCore(ReadOnlySpan<byte> source);
+        public bool TryComputeHash(ReadOnlySpan<byte> source, Span<byte> destination, out int bytesWritten);
+        protected virtual bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public abstract class HMAC : KeyedHashAlgorithm {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public class HMACMD5 : HMAC {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public class HMACSHA1 : HMAC {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public class HMACSHA256 : HMAC {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public class HMACSHA384 : HMAC {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public class HMACSHA512 : HMAC {
+        protected override void HashCore(ReadOnlySpan<byte> source);
+        protected override bool TryHashFinal(Span<byte> destination, out int bytesWritten);
     }
     public sealed class IncrementalHash : IDisposable {
+        public void AppendData(ReadOnlySpan<byte> data);
+        public bool TryGetHashAndReset(Span<byte> destination, out int bytesWritten);
     }
     public abstract class RandomNumberGenerator : IDisposable {
+        public static void Fill(Span<byte> data);
+        public virtual void GetBytes(Span<byte> data);
+        public virtual void GetNonZeroBytes(Span<byte> data);
     }
     public abstract class RSA : AsymmetricAlgorithm {
+        public virtual bool TryDecrypt(ReadOnlySpan<byte> data, Span<byte> destination, RSAEncryptionPadding padding, out int bytesWritten);
+        public virtual bool TryEncrypt(ReadOnlySpan<byte> data, Span<byte> destination, RSAEncryptionPadding padding, out int bytesWritten);
+        protected virtual bool TryHashData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, out int bytesWritten);
+        public virtual bool TrySignData(ReadOnlySpan<byte> data, Span<byte> destination, HashAlgorithmName hashAlgorithm, RSASignaturePadding padding, out int bytesWritten);
+        public virtual bool TrySignHash(ReadOnlySpan<byte> hash, Span<byte> destination, HashAlgorithmName hashAlgorithm, RSASignaturePadding padding, out int bytesWritten);
+        public virtual bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, RSASignaturePadding padding);
+        public virtual bool VerifyHash(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, HashAlgorithmName hashAlgorithm, RSASignaturePadding padding);
     }
 }
 namespace System.Security.Cryptography.X509Certificates {
     public class X509Certificate : IDeserializationCallback, IDisposable, ISerializable {
+        public virtual byte[] GetCertHash(HashAlgorithmName hashAlgorithm);
+        public virtual string GetCertHashString(HashAlgorithmName hashAlgorithm);
+        public virtual bool TryGetCertHash(HashAlgorithmName hashAlgorithm, Span<byte> destination, out int bytesWritten);
     }
     public class X509CertificateCollection : CollectionBase {
+        protected override void OnValidate(object value);
     }
 }
 namespace System.Text {
     public abstract class Decoder {
+        public virtual void Convert(ReadOnlySpan<byte> bytes, Span<char> chars, bool flush, out int bytesUsed, out int charsUsed, out bool completed);
+        public virtual int GetCharCount(ReadOnlySpan<byte> bytes, bool flush);
+        public virtual int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars, bool flush);
     }
     public abstract class Encoder {
+        public virtual void Convert(ReadOnlySpan<char> chars, Span<byte> bytes, bool flush, out int charsUsed, out int bytesUsed, out bool completed);
+        public virtual int GetByteCount(ReadOnlySpan<char> chars, bool flush);
+        public virtual int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes, bool flush);
     }
     public abstract class Encoding : ICloneable {
+        public virtual ReadOnlySpan<byte> Preamble { get; }
+        public virtual int GetByteCount(ReadOnlySpan<char> chars);
+        public virtual int GetBytes(ReadOnlySpan<char> chars, Span<byte> bytes);
+        public virtual int GetCharCount(ReadOnlySpan<byte> bytes);
+        public virtual int GetChars(ReadOnlySpan<byte> bytes, Span<char> chars);
+        public string GetString(ReadOnlySpan<byte> bytes);
     }
     public sealed class StringBuilder : ISerializable {
+        public StringBuilder Append(ReadOnlySpan<char> value);
+        public StringBuilder Append(StringBuilder value);
+        public StringBuilder Append(StringBuilder value, int startIndex, int count);
+        public void CopyTo(int sourceIndex, Span<char> destination, int count);
+        public bool Equals(ReadOnlySpan<char> value);
+        public StringBuilder Insert(int index, ReadOnlySpan<char> value);
     }
 }
 namespace System.Text.RegularExpressions {
     public class Regex : ISerializable {
+        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname);
+        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname, CustomAttributeBuilder[] attributes);
+        public static void CompileToAssembly(RegexCompilationInfo[] regexinfos, AssemblyName assemblyname, CustomAttributeBuilder[] attributes, string resourceFile);
     }
+    public class RegexCompilationInfo {
+        public RegexCompilationInfo(string pattern, RegexOptions options, string name, string fullnamespace, bool ispublic);
+        public RegexCompilationInfo(string pattern, RegexOptions options, string name, string fullnamespace, bool ispublic, TimeSpan matchTimeout);
+        public bool IsPublic { get; set; }
+        public TimeSpan MatchTimeout { get; set; }
+        public string Name { get; set; }
+        public string Namespace { get; set; }
+        public RegexOptions Options { get; set; }
+        public string Pattern { get; set; }
+    }
 }
 namespace System.Threading {
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct AsyncFlowControl : IDisposable {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct AsyncFlowControl : IDisposable {
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct CancellationTokenRegistration : IDisposable, IEquatable<CancellationTokenRegistration> {
+        public CancellationToken Token { get; }
     }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct LockCookie {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct LockCookie {
     }
     public sealed class Thread : CriticalFinalizerObject {
+        public static int GetCurrentProcessorId();
     }
     public static class ThreadPool {
+        public static bool QueueUserWorkItem<TState>(Action<TState> callBack, TState state, bool preferLocal);
     }
 }
 namespace System.Threading.Tasks {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTask : IEquatable<ValueTask> {
+        public ValueTask(IValueTaskSource source, short token);
+        public ValueTask(Task task);
+        public bool IsCanceled { get; }
+        public bool IsCompleted { get; }
+        public bool IsCompletedSuccessfully { get; }
+        public bool IsFaulted { get; }
+        public Task AsTask();
+        public ConfiguredValueTaskAwaitable ConfigureAwait(bool continueOnCapturedContext);
+        public override bool Equals(object obj);
+        public bool Equals(ValueTask other);
+        public ValueTaskAwaiter GetAwaiter();
+        public override int GetHashCode();
+        public static bool operator ==(ValueTask left, ValueTask right);
+        public static bool operator !=(ValueTask left, ValueTask right);
+        public ValueTask Preserve();
+    }
-    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
-    public struct ValueTask<TResult> : IEquatable<ValueTask<TResult>> {
+    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
+    public struct ValueTask<TResult> : IEquatable<ValueTask<TResult>> {
+        public ValueTask(IValueTaskSource<TResult> source, short token);
-        public static AsyncValueTaskMethodBuilder<TResult> CreateAsyncMethodBuilder();
+        public ValueTask<TResult> Preserve();
     }
 }
+namespace System.Threading.Tasks.Sources {
+    public interface IValueTaskSource {
+        void GetResult(short token);
+        ValueTaskSourceStatus GetStatus(short token);
+        void OnCompleted(Action<object> continuation, object state, short token, ValueTaskSourceOnCompletedFlags flags);
+    }
+    public interface IValueTaskSource<out TResult> {
+        TResult GetResult(short token);
+        ValueTaskSourceStatus GetStatus(short token);
+        void OnCompleted(Action<object> continuation, object state, short token, ValueTaskSourceOnCompletedFlags flags);
+    }
+    public enum ValueTaskSourceOnCompletedFlags {
+        FlowExecutionContext = 2,
+        None = 0,
+        UseSchedulingContext = 1,
+    }
+    public enum ValueTaskSourceStatus {
+        Canceled = 3,
+        Faulted = 2,
+        Pending = 0,
+        Succeeded = 1,
+    }
+}
 namespace Microsoft.Win32.SafeHandles {
     public sealed class SafeX509ChainHandle : SafeHandleZeroOrMinusOneIsInvalid {
-        public override bool IsInvalid { get; }
     }
 }
```

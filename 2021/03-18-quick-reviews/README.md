# Quick Reviews 03/18/2021

## Introduce Date and Time only structs

**NeedsWork** | [#runtime/49036](https://github.com/dotnet/runtime/issues/49036#issuecomment-802237375)

* `DateOnly` vs `Date`
    - VB has a keyword `Date` that binds to `System.DateTime`. If we called the type `Date` VB users would have to use a namespace qualified name or use the escaping mechanism `[Date]`. If we expect this type to be popular among VB users that would be bad. If we don't believe it is, `Date` might be acceptable to.
    - We already have many properties in the BCL called `Date` that return a `DateTime`. If we wanted to add properties that return the new type, we'd have to come up with a new name. In that sense, it might be better if were to choose a different name and use that for both the type name and the property name, such as the proposed `DateOnly`.
* `DateOnly`
    - The constructor taking a single int is a bit weird. Should this be a factory instead?
    - Is `DaysInMonth()`, `IsLeapYear()` required?
    - We can't have an implicit conversion from `DateTime` to `Date` because it results in data loss. Also, if we want to have an implicit conversion the other way, we shouldn't have the other way because it can result in ambiguities. Do we need conversions at all or should we have named methods or properties?
    - The plus operator is unfortunate because time zone is unspecified. An instance method like `ToDateTime(TimeOfDay, ...)` can have arguments. If we want to support an implicit zero time, we probably want different methods.
    - Let's remove the static `Compare()` and `Equals` methods?
    - The parse methods should have string overloads
* `TimeOfDay`
    - Can we get away with removing the `Meridiem` type and associated constructors and properties and only expose this concept in parsing and formatting?
    - Circular math without throwing makes sense, but it might surprise people. We should add an overload that indicates by how many days the clock wrapped forward or backwards. To make the API simpler, let's remove the other `AddXxx()` methods.
    - `IsBetween()` the parameters should be named `start` and `end`
    - Let's remove the binary `-` and `+` that take TimeSpan in favor of the `Add` method
    - Let's remove the conversion operators in favor of constructors
    - Let's remove the static `Compare()` and `Equals` methods?
    - The parse methods should have string overloads

```C#
namespace System
{
    // The name should be just Date but we couldn't use this name as VB is using the name type.
    // Using DateOnly name but we may accept better names if there is any.
    public readonly struct DateOnly : IComparable, IComparable<DateOnly>, IEquatable<DateOnly>, IFormattable
    {
        public static DateOnly MaxValue { get; }
        public static DateOnly MinValue { get; }

        public DateOnly(int year, int month, int day);
        public DateOnly(int year, int month, int day, Calendar calendar);
        public DateOnly(int dayNumber); // Needed only for serialization as we did in DateTime.Ticks

        public int Year { get; }
        public int Month { get; }
        public int Day { get; }
        public DayOfWeek DayOfWeek { get; }
        public int DayOfYear { get; }
        public int DayNumber { get; } // can be used to calculate the number of days between 2 dates
        public static int DaysInMonth(int year, int month);

        public DateOnly AddDays(int days);
        public DateOnly AddMonths(int months);
        public DateOnly AddYears(int years);
        public static bool IsLeapYear(int year);
        public static bool operator ==(DateOnly left, DateOnly right);
        public static bool operator >(DateOnly left, DateOnly right);
        public static bool operator >=(DateOnly left, DateOnly right);
        public static bool operator !=(DateOnly left, DateOnly right);
        public static bool operator <(DateOnly left, DateOnly right);
        public static bool operator <=(DateOnly left, DateOnly right);

        public int CompareTo(DateOnly value);
        public int CompareTo(object? value);

        public bool Equals(DateOnly value);
        public override bool Equals(object? value);
        public override int GetHashCode(); // day number

        // Only Allowed DateTimeStyles: AllowWhiteSpaces, AllowTrailingWhite, AllowLeadingWhite, and AllowInnerWhite
        public static DateOnly Parse(string s);
        public static DateOnly Parse(string s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static DateOnly ParseExact(string s, string format);
        public static DateOnly ParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static DateOnly ParseExact(string s, string [] formats);
        public static DateOnly ParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(string s, out DateOnly date);
        public static bool TryParse(string s, IFormatProvider? provider, DateTimeStyles styles, out DateOnly date);
        public static bool TryParseExact(string s, string format, out DateOnly date);
        public static bool TryParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);
        public static bool TryParseExact(string s, string [] formats, out DateOnly date);
        public static bool TryParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);

        public static DateOnly Parse(ReadOnlySpan<char> s);
        public static DateOnly Parse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, string [] formats);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(ReadOnlySpan<char> s, out DateOnly date);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);

        // Acceptable formats:
        //      Year month pattern              y/Y
        //      Sortable date/time pattern      s   date part only // like O one
        //      RFC1123 pattern                 r/R date part only // include the day of week. based on RFC1123/rfc5322 "ddd, dd MMM yyyy"
        //      round-trip date/time pattern    o/O date part only
        //      Month/day pattern               m/M
        //      Short date pattern              d
        //      Long date pattern               D

        public string ToLongDateString(); // use "D"     should we call it ToLongString
        public string ToShortDateString(); // us "d"     should we call it ToShortString

        public override string ToString();
        public string ToString(string? format);
        public string ToString(IFormatProvider? provider);
        public string ToString(string? format, IFormatProvider? provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default, IFormatProvider? provider = null);
    }

    public readonly struct TimeOfDay : IComparable, IComparable<TimeOfDay>, IEquatable<TimeOfDay>, IFormattable // TimeOfDay
    {
        public static TimeOfDay MaxValue { get; }
        public static TimeOfDay MinValue { get; }

        public TimeOfDay(int hour, int minutes);
        public TimeOfDay(int hour, int minutes, int seconds);
        public TimeOfDay(int hour, int minutes, int seconds, int milliseconds);
        public TimeOfDay(long ticks);
        public TimeOfDay(TimeSpan timeSpan);
        public TimeOfDay(DateTime dateTime);

        public int Hour { get; }
        public int Minute { get; }
        public int Second { get; }
        public int Millisecond { get; }
        public long Ticks { get; }

        public TimeOfDay Add(TimeSpan timeSpan); // circular
        public TimeOfDay Add(TimeSpan timeSpan, out int daysWrappedAround);
        public bool IsBetween(TimeOfDay start, TimeOfDay end);

        public static bool operator ==(TimeOfDay left, TimeOfDay right);
        public static bool operator >(TimeOfDay left, TimeOfDay right);
        public static bool operator >=(TimeOfDay left, TimeOfDay right);
        public static bool operator !=(TimeOfDay left, TimeOfDay right);
        public static bool operator <(TimeOfDay left, TimeOfDay right);
        public static bool operator <=(TimeOfDay left, TimeOfDay right);

        public static TimeSpan operator -(TimeOfDay t1, TimeOfDay t2); // Always positive time span

        public int CompareTo(TimeOfDay value);
        public int CompareTo(object? value);

        public bool Equals(TimeOfDay value);
        public override bool Equals(object? value);
        public override int GetHashCode();

        // Only Allowed DateTimeStyles: AllowWhiteSpaces, AllowTrailingWhite, AllowLeadingWhite, and AllowInnerWhite
        public static TimeOfDay Parse(string s);
        public static TimeOfDay Parse(string s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static TimeOfDay ParseExact(string s, string format);
        public static TimeOfDay ParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static TimeOfDay ParseExact(string s, string [] formats);
        public static TimeOfDay ParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(string s, out TimeOfDay time);
        public static bool TryParse(string s, IFormatProvider? provider, DateTimeStyles styles, out TimeOfDay time);
        public static bool TryParseExact(string s, string format, out TimeOfDay time);
        public static bool TryParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);
        public static bool TryParseExact(string s, string [] formats, out TimeOfDay time);
        public static bool TryParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);

        public static TimeOfDay Parse(ReadOnlySpan<char> s);
        public static TimeOfDay Parse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, string [] formats);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(ReadOnlySpan<char> s, out TimeOfDay time);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);

        // Acceptable formats:
        //      Sortable date/time pattern      s   Time part only
        //      RFC1123 pattern                 r/R Time part only like o
        //      round-trip date/time pattern    o/O Time part only
        //      Short time pattern              t
        //      Long time pattern               T

        public string ToLongString(); // use "T"     ToLongTimeString
        public string ToShortString(); // us "t"     ToShortTimeString

        public override string ToString();
        public string ToString(string? format);
        public string ToString(IFormatProvider? provider);
        public string ToString(string? format, IFormatProvider? provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
    }
}
```


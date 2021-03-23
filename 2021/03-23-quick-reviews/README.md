# Quick Reviews 03/23/2021

## Introduce Date and Time only structs

**Approved** | [#runtime/49036](https://github.com/dotnet/runtime/issues/49036#issuecomment-805091554) | [Video](https://www.youtube.com/watch?v=UF3eu1WBRrc&t=0h0m0s)

* `DateOnly`
    - Remove `AddSeconds()`
    - Add `ToTimeSpan()`
    - The `AddXxx()` methods should take `double`, because that's what `DateTime` does
* `TimeOfDay`
    - Looks good as proposed

```C#
namespace System
{
    public readonly struct DateOnly : IComparable, IComparable<DateOnly>, IEquatable<DateOnly>, IFormattable
    {
        public static DateOnly MaxValue { get; }
        public static DateOnly MinValue { get; }

        public DateOnly(int year, int month, int day);
        public DateOnly(int year, int month, int day, Calendar calendar);

        public static DateOnly FromDayNumber(int dayNumber);

        public int Year { get; }
        public int Month { get; }
        public int Day { get; }
        public DayOfWeek DayOfWeek { get; }
        public int DayOfYear { get; }
        public int DayNumber { get; } // can be used to calculate the number of days between 2 dates

        public DateOnly AddDays(double days);
        public DateOnly AddMonths(double months);
        public DateOnly AddYears(double years);

        public static bool operator ==(DateOnly left, DateOnly right);
        public static bool operator >(DateOnly left, DateOnly right);
        public static bool operator >=(DateOnly left, DateOnly right);
        public static bool operator !=(DateOnly left, DateOnly right);
        public static bool operator <(DateOnly left, DateOnly right);
        public static bool operator <=(DateOnly left, DateOnly right);

        public DateTime ToDateTime(TimeOfDay time);
        public DateTime ToDateTime(TimeOfDay time, DateTimeKind kind);
        public static DateOnly FromDateTime(DateTime dateTime);

        public int CompareTo(DateOnly value);
        public int CompareTo(object? value);

        public bool Equals(DateOnly value);

        public override bool Equals(object? value);
        public override int GetHashCode(); // day number

        // Only Allowed DateTimeStyles: AllowWhiteSpaces, AllowTrailingWhite, AllowLeadingWhite, and AllowInnerWhite
        public static DateOnly Parse(ReadOnlySpan<char> s);
        public static DateOnly Parse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, string [] formats);
        public static DateOnly ParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static DateOnly Parse(string s);
        public static DateOnly Parse(string s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static DateOnly ParseExact(string s, string format);
        public static DateOnly ParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static DateOnly ParseExact(string s, string [] formats);
        public static DateOnly ParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(ReadOnlySpan<char> s, out DateOnly date);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, out DateOnly date);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);

        public static bool TryParse(string s, out DateOnly date);
        public static bool TryParse(string s, IFormatProvider? provider, DateTimeStyles styles, out DateOnly date);
        public static bool TryParseExact(string s, string format, out DateOnly date);
        public static bool TryParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);
        public static bool TryParseExact(string s, string [] formats, out DateOnly date);
        public static bool TryParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out DateOnly date);

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
        public string ToString(System.IFormatProvider? provider);
        public string ToString(string? format, System.IFormatProvider? provider);
        public bool TryFormat(Span<char> destination, out int charsWritten, ReadOnlySpan<char> format = default(ReadOnlySpan<char>), IFormatProvider? provider = null);
    }

    public readonly struct TimeOfDay : IComparable, IComparable<TimeOfDay>, IEquatable<TimeOfDay>, IFormattable // TimeOfDay
    {
        public static TimeOfDay MaxValue { get; }
        public static TimeOfDay MinValue { get; }

        public TimeOfDay(int hour, int minutes);
        public TimeOfDay(int hour, int minutes, int seconds);
        public TimeOfDay(int hour, int minutes, int seconds, int milliseconds);

        public TimeOfDay(long ticks);

        public int Hour { get; }
        public int Minute { get; }
        public int Second { get; }
        public int Millisecond { get; }
        public long Ticks { get; }

        public TimeOfDay Add(TimeSpan timeSpan); // circular
        public TimeOfDay Add(TimeSpan timeSpan, out int wrappedDays); // circular

        public TimeOfDay AddHours(double hours);
        public TimeOfDay AddHours(double hours, out int wrappedDays);

        public TimeOfDay AddMinutes(double minutes);
        public TimeOfDay AddMinutes(double minutes, out int wrappedDays);
       
        public bool IsBetween(TimeOfDay start, TimeOfDay end);

        public static bool operator ==(TimeOfDay left, TimeOfDay right);
        public static bool operator >(TimeOfDay left, TimeOfDay right);
        public static bool operator >=(TimeOfDay left, TimeOfDay right);
        public static bool operator !=(TimeOfDay left, TimeOfDay right);
        public static bool operator <(TimeOfDay left, TimeOfDay right);
        public static bool operator <=(TimeOfDay left, TimeOfDay right);

        public static TimeSpan operator -(TimeOfDay t1, TimeOfDay t2); // Always positive time span
        public static TimeOfDay FromTimeSpan(TimeSpan timeSpan);
        public static TimeOfDay FromDateTime(DateTime dateTime); 
        public int CompareTo(TimeOfDay value);
        public int CompareTo(object? value);

        public bool Equals(TimeOfDay value);
        public override bool Equals(object? value);
        public override int GetHashCode();

        // Only Allowed DateTimeStyles: AllowWhiteSpaces, AllowTrailingWhite, AllowLeadingWhite, and AllowInnerWhite
        public static TimeOfDay Parse(ReadOnlySpan<char> s);
        public static TimeOfDay Parse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, string [] formats);
        public static TimeOfDay ParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static TimeOfDay Parse(string s);
        public static TimeOfDay Parse(string s, IFormatProvider? provider, DateTimeStyles styles = DateTimeStyles.None);
        public static TimeOfDay ParseExact(string s, string format);
        public static TimeOfDay ParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);
        public static TimeOfDay ParseExact(string s, string [] formats);
        public static TimeOfDay ParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style = DateTimeStyles.None);

        public static bool TryParse(ReadOnlySpan<char> s, out TimeOfDay time);
        public static bool TryParse(ReadOnlySpan<char> s, IFormatProvider? provider, DateTimeStyles styles, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, ReadOnlySpan<char> format, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, out TimeOfDay time);
        public static bool TryParseExact(ReadOnlySpan<char> s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);

        public static bool TryParse(string s, out TimeOfDay time);
        public static bool TryParse(string s, IFormatProvider? provider, DateTimeStyles styles, out TimeOfDay time);
        public static bool TryParseExact(string s, string format, out TimeOfDay time);
        public static bool TryParseExact(string s, string format, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);
        public static bool TryParseExact(string s, string [] formats, out TimeOfDay time);
        public static bool TryParseExact(string s, string [] formats, IFormatProvider? provider, DateTimeStyles style, out TimeOfDay time);

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

## Enhance user-facing API for strongly-typed ILogger messages

**NeedsWork** | [#runtime/36022](https://github.com/dotnet/runtime/issues/36022#issuecomment-805134925) | [Video](https://www.youtube.com/watch?v=UF3eu1WBRrc&t=0h26m0s)

* We should  make sure this design is aligned with C# 10's changes to interpolated string builders
* The generator currently finds the `ILogger` instance by name, it should do it by type and fail if more than one field of `ILogger` is in scope. We could introduce an attribute to let people choose their name in those cases. Project file configuration would be unfortunate because it's not within the code.
* Event IDs needs to be unique for a given category. It would be nice to have simple rules that allows the generator to validate uniqueness for most cases. We could accept false positives and rely on the normal suppression mechanism for diagnostics.
* Alternatively, the original proposal used extension methods which people didn't like because they don't want people to use the basic `Log` methods. Maybe the generator could warn on usages when extension methods exists?
    - This would mean that we don't have to identity the logger field
    - This would mean that event IDs uniqueness scope would be indicated by the type of the first argument.
* We shouldn't introduce an `Internal` namespace

```C#
namespace Microsoft.Extensions.Logging
{
    [AttributeUsage(AttributeTargets.Method)]
    public sealed partial class LoggerMessageAttribute : Attribute
    {
        public LoggerMessageAttribute(int eventId, LogLevel level, string? message = null);
        public LoggerMessageAttribute(int eventId, string? message = null);
        public int EventId { get; }
        public string? EventName { get; set; }
        public LogLevel? Level { get; }
        public string? Message { get; }
    }
}
```

## AsyncMethodBuilderOverrideAttribute and PoolingAsyncValueTaskMethodBuilders

**Approved** | [#runtime/49903](https://github.com/dotnet/runtime/issues/49903#issuecomment-805149431) | [Video](https://www.youtube.com/watch?v=UF3eu1WBRrc&t=1h27m38s)

* Looks good as proposed

```C#
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeTargets.Class |
                    AttributeTargets.Struct |
                    AttributeTargets.Interface |
                    AttributeTargets.Method |
                    AttributeTargets.Constructor |
                    AttributeTargets.Event |
                    AttributeTargets.Property |
                    AttributeTargets.Module, Inherited = false, AllowMultiple = true)]
    public sealed partial class AsyncMethodBuilderOverrideAttribute : System.Attribute
    {
        public AsyncMethodBuilderOverrideAttribute(Type builderType);
        public Type BuilderType { get; }
    }
    public struct PoolingAsyncValueTaskMethodBuilder
    {
        public static PoolingAsyncValueTaskMethodBuilder Create();
        public ValueTask Task { get; }
        public void SetResult();
        public void SetException(Exception exception);
        public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
        public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
        public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
        public void SetStateMachine(IAsyncStateMachine stateMachine);
    }
    public struct PoolingAsyncValueTaskMethodBuilder<TResult>
    {
        public static PoolingAsyncValueTaskMethodBuilder<TResult> Create();
        public ValueTask<TResult> Task { get; }
        public void SetResult(TResult result);
        public void SetException(Exception exception);
        public void AwaitOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : INotifyCompletion where TStateMachine : IAsyncStateMachine;
        public void AwaitUnsafeOnCompleted<TAwaiter, TStateMachine>(ref TAwaiter awaiter, ref TStateMachine stateMachine) where TAwaiter : System.Runtime.CompilerServices.ICriticalNotifyCompletion where TStateMachine : IAsyncStateMachine;
        public void SetStateMachine(IAsyncStateMachine stateMachine);
        public void Start<TStateMachine>(ref TStateMachine stateMachine) where TStateMachine : IAsyncStateMachine;
    }
}
```


## ExceptionDispatchInfo.TrySetStackTrace

**Approved** | [#runtime/50044](https://github.com/dotnet/runtime/issues/50044#issuecomment-805161306) | [Video](https://www.youtube.com/watch?v=UF3eu1WBRrc&t=1h47m18s)

* Let's make this a non-try method (and let it fail if it can't work)
* We should make sure that `new StackTrace(exception)` doesn't blow up when someone catches the exception and tries to parse the stack trace

```C#
namespace System.Runtime.ExceptionServices
{
    public sealed class ExceptionDispatchInfo
    {
        public static bool SetRemoteStackTrace(Exception source, string stackTrace);

        // existing API for reference
        // public static Exception SetCurrentStackTrace(Exception source);
    }
}
```


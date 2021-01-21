# Quick Reviews 01/21/2021

## Add Activity.GetTagItem

**Approved** | [#runtime/39866](https://github.com/dotnet/runtime/issues/39866#issuecomment-764841563) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=0h0m0s)

Looks good as proposed.

```C#
namespace System.Diagnostics
{
    public class Activity
    {
        public object? GetTagItem(string key);
    }
}
```
## Generic Host is not stopped when BackgroundService crashes

**Approved** | [#runtime/43637](https://github.com/dotnet/runtime/issues/43637#issuecomment-764874136) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=0h10m9s)

* It seems the new behavior is more desirable than the current behavior but it's hard to know how many apps would be broken by the new behavior.
* The API isn't very discoverable, so if it's purely opt-in, then most customers aren't likely to benefit from the new behavior.
* A compromise would be:
    - Change the templates for .NET 6 to set this property to `BackgroundServiceExceptionBehavior.StopHost`
    - Based on customer feedback, we can change the default for .NET 7 and simplify the templates
* On the other hand, this is early for .NET 6
    - Let's ship the API
    - Let's change the default now, not update the templates, see what feedback we get for the previews, and revert back to opt-in if it breaks many people.

```C#
namespace Microsoft.Extensions.Hosting
{
    public enum BackgroundServiceExceptionBehavior
    {
        Ignore,
        StopHost
    }
    public class HostOptions
    {
        public BackgroundServiceExceptionBehavior BackgroundServiceExceptionBehavior { get; set; }
    }
}
```

## Add missing math functions defined by IEEE 754:2019

**Approved** | [#runtime/20137](https://github.com/dotnet/runtime/issues/20137#issuecomment-764879294) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=1h8m15s)

* Looks good as proposed, but let's also add the `MaxNumber` variants that we previously rejected so that we get entire spec implemented

```C#
namespace System
{
    public static partial class Math
    {    
        public static double AcosPi(double x);
        public static double AsinPi(double x);
        public static double Atan2Pi(double y, double x);
        public static double AtanPi(double x);
        public static double Compound(double x, double n);
        public static double CosPi(double x);
        public static double Exp10(double x);
        public static double Exp10M1(double x);
        public static double Exp2(double x);
        public static double Exp2M1(double x);
        public static double ExpM1(double x);
        public static double Hypot(double x, double y);
        public static double Log10P1(double x);
        public static double Log2P1(double x);
        public static double LogP1(double x);
        public static double MaxMagnitudeNumber(double x, double y);
        public static double MaxNumber(double x, double y);
        public static double MinMagnitudeNumber(double x, double y);
        public static double MinNumber(double x, double y);
        public static double Root(double x, double n);
        public static double SinPi(double x);
        public static double TanPi(double x);
    }
    public static partial class MathF
    {
        public static float AcosPi(float x);
        public static float AsinPi(float x);
        public static float Atan2Pi(float y, float x);
        public static float AtanPi(float x);
        public static float Compound(float x, float n);
        public static float CosPi(float x);
        public static float Exp10(float x);
        public static float Exp10M1(float x);
        public static float Exp2(float x);
        public static float Exp2M1(float x);
        public static float ExpM1(float x);
        public static float Hypot(float x, float y);
        public static float Log10P1(float x);
        public static float Log2P1(float x);
        public static float LogP1(float x);
        public static float MaxMagnitudeNumber(float x, float y);
        public static float MaxNumber(float x, float y);
        public static float MinMagnitudeNumber(float x, float y);
        public static float MinNumber(float x, float y);
        public static float Root(float x, float n);
        public static float SinPi(float x);
        public static float TanPi(float x);
    }
}
```

## Enable AsVector<T> in Vector<T>

**Approved** | [#runtime/508](https://github.com/dotnet/runtime/issues/508#issuecomment-764884320) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=1h17m25s)

* Looks good as proposed

```C#
public partial class Vector
{
    public static Vector<TTo> As<TFrom, TTo>(this Vector<TFrom> vector)
        where TFrom : struct
        where TTo : struct;
}
```

## Add BinderOption to allow callers to specify that configuration binding should throw an exception upon any failure.

**Approved** | [#runtime/36015](https://github.com/dotnet/runtime/issues/36015#issuecomment-764897808) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=1h25m23s)

* `ErrorOnPropertyMissing` makes total sense. The default should be `false` so that it's not a breaking change. But the name is a bit ambiguous. We'd prefer `ErrorOnUnknownConfiguration`.
* `ErrorOnKeyMissing` seems a bit strange because options are mostly, well, optional. A better design might be to add an attribute that the user would put on their strongly typed option type property, akin to how serializers work. This design assumes the binder doesn't initialize unspecified properties to their default value.

```C#
namespace Microsoft.Extensions.Configuration
{
    public class BinderOptions
    {
        public bool ErrorOnUnknownConfiguration{ get; set; }
    }
}
```

## Expose Host.ConfigureDefaults

**Approved** | [#runtime/36003](https://github.com/dotnet/runtime/issues/36003#issuecomment-764900907) | [Video](https://www.youtube.com/watch?v=nIk0aU_Ty_w&t=1h48m20s)

* Let's not name it `CreateDefaultBuilder()` because it doesn't actually create a new builder, it just applies the defaults.
* Let's make an extension method so that the usage looks less noisy
* That means we should move it to `HostingHostBuilderExtensions` instead

```C#
namespace Microsoft.Extensions.Hosting
{
    public static partial class HostingHostBuilderExtensions
    {
        public static IHostBuilder ConfigureDefaults(this IHostBuilder builder, string[] args);
    }
}
```


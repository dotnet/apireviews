# Quick Reviews 11/24/2020

## Bmi2 MultiplyNoFlags2

**Rejected** | [#runtime/44926](https://github.com/dotnet/runtime/issues/44926#issuecomment-733154504) | [Video](https://www.youtube.com/watch?v=Dcu0Cz69onc&t=0h0m0s)

* It seems there are two separate issues:
    1. Performance issue, which we can solve with a private intrinsic that uses tuples.
    2. Usability issue due to use of pointers
* We should fix (1) which doesn't require new public APIs but we should also do a pass of all the APIs that we believe need `out` or tuple overloads

```C#
namespace System.Runtime.Intrinsics.X86
{
    public abstract class Bmi2 : X86Base
    {
        public static (uint Lower, uint Upper) MultiplyNoFlags2(uint left, uint right);

        public abstract class X64: X86Base.X64
        {
            public static (ulong Lower, ulong Upper) MultiplyNoFlags2(ulong left, ulong right);
        }
    }
}
```
## Override Stream.ReadAsync/WriteAsync

**Approved** | [#runtime/33789](https://github.com/dotnet/runtime/issues/33789#issuecomment-733167928) | [Video](https://www.youtube.com/watch?v=Dcu0Cz69onc&t=0h20m44s)

* We should scope it to cases is `Stream` (and not a derived type)
* The rule should flag types that override
    - `ReadAsync(byte[])` but that don't override the overload with `Memory<T>`
    - `WriteAsync(byte[])` but that don't override the overload with `Memory<T>`
* On by default with severity info
* Perf category
* No fixer needed, the default behavior of auto completing `override` or Ctrl + . Generate overrides is sufficient
## Activator.CreateFactory and ConstructorInfo.CreateDelegate

**Approved** | [#runtime/36194](https://github.com/dotnet/runtime/issues/36194#issuecomment-733185658) | [Video](https://www.youtube.com/watch?v=Dcu0Cz69onc&t=0h48m10s)

* We don't add a `new()` constraint to match the existing API

```C#
namespace System
{
    public partial class Activator
    {
        // Existing APIs
        // public static T CreateInstance<T>();
        // public static object? CreateInstance(Type type);
        // public static object? CreateInstance(Type type, bool nonPublic);

        public static Func<T> CreateFactory<T>();
        public static Func<object?> CreateFactory(Type type);
        public static Func<object?> CreateFactory(Type type, bool nonPublic);
    }
}
namespace System.Reflection
{
    public class ConstructorInfo
    {
      public static TDelegate CreateDelegate<TDelegate>();
      public static Delegate CreateDelegate(Type delegateType);
    }
}
```

## Add ConfigurationBinder.Bind<T> overloads

**NeedsWork** | [#runtime/43930](https://github.com/dotnet/runtime/issues/43930#issuecomment-733196396) | [Video](https://www.youtube.com/watch?v=Dcu0Cz69onc&t=1h25m0s)

* What are the linker annotations?
* Why do we need generic overloads? If we believe the compiler can always implicitly inferred, then it seems maybe we should able to do this inference by the linker as well. In other words, can we annotate the existing object based overloads? It seems local tracking is something the linker could be doing.
* How many APIs like this do we have in the framework? If it's just a couple, adding generic overloads is fine, but if that's a pervasive pattern, than a change in the linker seems more useful.

```C#
namespace Microsoft.Extensions.Configuration
{
    public partial class ConfigurationBinder
    {
        public static void Bind<T>(this IConfiguration configuration, string key, T instance);
        public static void Bind<T>(this IConfiguration configuration, T instance);
        public static void Bind<T>(this IConfiguration configuration, T instance, Action<BinderOptions> configureOptions);
    }
}
```

## Make it safer and easier to build an X500DistinguishedName

**Approved** | [#runtime/44738](https://github.com/dotnet/runtime/issues/44738#issuecomment-733204446) | [Video](https://www.youtube.com/watch?v=Dcu0Cz69onc&t=1h46m53s)

* Looks good as proposed
* We considered fluent pattern but concluded "meh"
* Consider overriding `ToString()` (or adding a `DebuggerDisplayAttribute`) so that during debugging people can see what's already in the builder.
* Let's use `string`, we can always add `ReadOnlySpan<char>` in the future. But we should overload the byte one.

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public sealed class X500DistinguishedNameBuilder
    {
        public void AddEmailAddress(string emailAddress, Asn1Tag? stringEncodingType = null);
        public void AddDomainComponent(string domainComponent, Asn1Tag? stringEncodingType = null);
        public void AddLocalityName(string localityName, Asn1Tag? stringEncodingType = null);
        public void AddCommonName(string commonName, Asn1Tag? stringEncodingType = null);
        public void AddCountryOrRegion(string twoLetterCode, Asn1Tag? stringEncodingType = null);
        public void AddOrganizationName(string organizationName, Asn1Tag? stringEncodingType = null);
        public void AddOrganizationalUnitName(string organizationalUnitName, Asn1Tag? stringEncodingType = null);
        public void AddStateOrProvinceName(string stateOrProvinceName, Asn1Tag? stringEncodingType = null);

        public void Add(Oid oid, string value, Asn1Tag? stringEncodingType = null);
        public void Add(string oidValue, string value, Asn1Tag? stringEncodingType = null);

        public void AddEncoded(Oid oid, byte[] encodedValue);
        public void AddEncoded(Oid oid, ReadOnlySpan<byte> encodedValue);
        public void AddEncoded(string oidValue, byte[] encodedValue);
        public void AddEncoded(string oidValue, ReadOnlySpan<byte> encodedValue);

        public X500DistinguishedName Build();
    }
}
```


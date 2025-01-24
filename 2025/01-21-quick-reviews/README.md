# API Review 01/21/2025

## Hide TypeInfo and references to it

**Approved** | [#runtime/89975](https://github.com/dotnet/runtime/issues/89975#issuecomment-2605438444) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h0m0s)

* Looks good as proposed

```diff
namespace System.Reflection;

+[EditorBrowsable(EditorBrowsableState.Never)]
public static partial class RuntimeReflectionExtensions
{
+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<FieldInfo> GetRuntimeFields([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.NonPublicFields)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<MethodInfo> GetRuntimeMethods([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.NonPublicMethods)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<PropertyInfo> GetRuntimeProperties([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties | DynamicallyAccessedMemberTypes.NonPublicProperties)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<EventInfo> GetRuntimeEvents([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents | DynamicallyAccessedMemberTypes.NonPublicEvents)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo? GetRuntimeField([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRuntimeMethod([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, string name, Type[] parameters);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetRuntimeProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo? GetRuntimeEvent([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRuntimeBaseDefinition(this MethodInfo method);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static InterfaceMapping GetRuntimeInterfaceMap(this TypeInfo typeInfo, [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.NonPublicMethods)] Type interfaceType);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo GetMethodInfo(this Delegate del);
}+[EditorBrowsable(EditorBrowsableState.Never)]
public static partial class RuntimeReflectionExtensions
{
+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<FieldInfo> GetRuntimeFields([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.NonPublicFields)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<MethodInfo> GetRuntimeMethods([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.NonPublicMethods)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<PropertyInfo> GetRuntimeProperties([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties | DynamicallyAccessedMemberTypes.NonPublicProperties)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static IEnumerable<EventInfo> GetRuntimeEvents([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents | DynamicallyAccessedMemberTypes.NonPublicEvents)] this Type type);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo? GetRuntimeField([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRuntimeMethod([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, string name, Type[] parameters);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetRuntimeProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo? GetRuntimeEvent([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents)] this Type type, string name);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRuntimeBaseDefinition(this MethodInfo method);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static InterfaceMapping GetRuntimeInterfaceMap(this TypeInfo typeInfo, [DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.NonPublicMethods)] Type interfaceType);

+    [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo GetMethodInfo(this Delegate del);
}

namespace System.Reflection;

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class AssemblyExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type[] GetExportedTypes(this Assembly assembly);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Module[] GetModules(this Assembly assembly);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type[] GetTypes(this Assembly assembly);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class EventInfoExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetAddMethod(this EventInfo eventInfo);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetAddMethod(this EventInfo eventInfo, bool nonPublic);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRaiseMethod(this EventInfo eventInfo);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRaiseMethod(this EventInfo eventInfo, bool nonPublic);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRemoveMethod(this EventInfo eventInfo);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetRemoveMethod(this EventInfo eventInfo, bool nonPublic);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class MemberInfoExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static int GetMetadataToken(this MemberInfo member);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static bool HasMetadataToken(this MemberInfo member);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class MethodInfoExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo GetBaseDefinition(this MethodInfo method);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static partial class ModuleExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static System.Guid GetModuleVersionId(this Module module);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static bool HasModuleVersionId(this Module module);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class PropertyInfoExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo[] GetAccessors(this PropertyInfo property);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo[] GetAccessors(this PropertyInfo property, bool nonPublic);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetGetMethod(this PropertyInfo property);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetGetMethod(this PropertyInfo property, bool nonPublic);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetSetMethod(this PropertyInfo property);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetSetMethod(this PropertyInfo property, bool nonPublic);
}

+[EditorBrowsable(EditorBrowsableState.Never)]
public static class TypeExtensions
{
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static ConstructorInfo? GetConstructor([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] this Type type, Type[] types);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static ConstructorInfo[] GetConstructors([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static ConstructorInfo[] GetConstructors([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicConstructors | DynamicallyAccessedMemberTypes.PublicConstructors)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MemberInfo[] GetDefaultMembers([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicEvents | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.PublicNestedTypes | DynamicallyAccessedMemberTypes.PublicProperties)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo? GetEvent([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents)] this Type type, string name);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo? GetEvent([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicEvents | DynamicallyAccessedMemberTypes.PublicEvents)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo[] GetEvents([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicEvents)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static EventInfo[] GetEvents([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicEvents | DynamicallyAccessedMemberTypes.PublicEvents)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo? GetField([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields)] this Type type, string name);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo? GetField([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicFields | DynamicallyAccessedMemberTypes.PublicFields)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo[] GetFields([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicFields)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static FieldInfo[] GetFields([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicFields | DynamicallyAccessedMemberTypes.PublicFields)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type[] GetGenericArguments(this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type[] GetInterfaces([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.Interfaces)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MemberInfo[] GetMember([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicEvents | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.PublicNestedTypes | DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MemberInfo[] GetMember([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.All)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MemberInfo[] GetMembers([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicConstructors | DynamicallyAccessedMemberTypes.PublicEvents | DynamicallyAccessedMemberTypes.PublicFields | DynamicallyAccessedMemberTypes.PublicMethods | DynamicallyAccessedMemberTypes.PublicNestedTypes | DynamicallyAccessedMemberTypes.PublicProperties)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MemberInfo[] GetMembers([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.All)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetMethod([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, string name);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetMethod([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicMethods | DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo? GetMethod([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, string name, Type[] types);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo[] GetMethods([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicMethods)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static MethodInfo[] GetMethods([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicMethods | DynamicallyAccessedMemberTypes.PublicMethods)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type? GetNestedType([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicNestedTypes | DynamicallyAccessedMemberTypes.PublicNestedTypes)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static Type[] GetNestedTypes([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicNestedTypes | DynamicallyAccessedMemberTypes.PublicNestedTypes)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo[] GetProperties([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo[] GetProperties([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicProperties | DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.NonPublicProperties | DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name, BindingFlags bindingAttr);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name, Type? returnType);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static PropertyInfo? GetProperty([DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.PublicProperties)] this Type type, string name, Type? returnType, Type[] types);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static bool IsAssignableFrom(this Type type, [.NotNullWhenAttribute(true)] Type? c);
+   [EditorBrowsable(EditorBrowsableState.Never)]
    public static bool IsInstanceOfType(this Type type, [.NotNullWhenAttribute(true)] object? o);
}
```
## Obsolete all Rfc2898DeriveBytes constructors

**Approved** | [#runtime/97221](https://github.com/dotnet/runtime/issues/97221#issuecomment-2605459333) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h16m26s)

* Looks good as proposed

```diff
namespace System.Security.Cryptography;

public partial class Rfc2898DeriveBytes : DeriveBytes
{
-       [Obsolete(Obsoletions.Rfc2898OutdatedCtorMessage, DiagnosticId = Obsoletions.Rfc2898OutdatedCtorDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(byte[] password, byte[] salt, int iterations);

+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(byte[] password, byte[] salt, int iterations, HashAlgorithmName hashAlgorithm);

-       [Obsolete(Obsoletions.Rfc2898OutdatedCtorMessage, DiagnosticId = Obsoletions.Rfc2898OutdatedCtorDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, byte[] salt);

-       [Obsolete(Obsoletions.Rfc2898OutdatedCtorMessage, DiagnosticId = Obsoletions.Rfc2898OutdatedCtorDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, byte[] salt, int iterations);

+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, byte[] salt, int iterations, HashAlgorithmName hashAlgorithm);

-       [Obsolete(Obsoletions.Rfc2898OutdatedCtorMessage, DiagnosticId = Obsoletions.Rfc2898OutdatedCtorDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, int saltSize);

-       [Obsolete(Obsoletions.Rfc2898OutdatedCtorMessage, DiagnosticId = Obsoletions.Rfc2898OutdatedCtorDiagId, UrlFormat = Obsoletions.SharedUrlFormat)]
+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, int saltSize, int iterations);

+       [Obsolete("Creating instances of Rfc2898DeriveBytes is obsolete. Use the static Pbkdf2 method instead.", DiagnosticId = "SYSLIBXXXX", UrlFormat = Obsoletions.SharedUrlFormat)]
        public Rfc2898DeriveBytes(string password, int saltSize, int iterations, HashAlgorithmName hashAlgorithm);
}
```
## Batched random for the CSPRNG (and System.Random)

**NeedsWork** | [#runtime/111211](https://github.com/dotnet/runtime/issues/111211#issuecomment-2605478022) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h27m19s)

* We are not convinced that accelerating `RandomNumberGenerator`/`Random` is needed
* Could we do a version of this that uses generic math, such as `Fill<T> where T: IBinaryInteger` such that the API surface is smaller?

```C#
namespace System.Security.Cryptography;

public partial class RandomNumberGenerator
{
    public static void FillInt32(int toExclusive, Span<int> destination);
    public static void FillInt32(int fromInclusive, int toExclusive, Span<int> destination);
    public static void FillUInt32(uint toExclusive, Span<uint> destination);    
    public static void FillUInt32(ReadOnlySpan<uint> toExclusives, Span<uint> destination);

    public static void FillUInt32(uint fromInclusive, uint toExclusive, Span<uint> destination);
    public static void FillInt64(long toExclusive, Span<long> destination);
    public static void FillInt64(long fromInclusive, long toExclusive, Span<long> destination);
    public static void FillUInt64(ulong toExclusive, Span<ulong> destination);
    public static void FillUInt64(ulong fromInclusive, ulong toExclusive, Span<ulong> destination);
    public static void FillUInt32(ReadOnlySpan<uint> fromInclusives, ReadOnlySpan<uint> toExclusives, Span<uint> destination);

    public static void FillInt32(ReadOnlySpan<int> toExclusives, Span<int> destination);
    public static void FillInt32(ReadOnlySpan<int> fromInclusives, ReadOnlySpan<int> toExclusives, Span<uint> destination);
\
    public static void FillInt64(ReadOnlySpan<long> toExclusives, Span<long> destination);
    public static void FillInt64(ReadOnlySpan<long> fromInclusives, ReadOnlySpan<long> toExclusives, Span<long> destination);

    public static void FillUInt64(ReadOnlySpan<ulong> toExclusives, Span<ulong> destination);
    public static void FillUInt64(ReadOnlySpan<ulong> fromInclusives, ReadOnlySpan<ulong> toExclusives, Span<ulong> destination);
}

public partial class RandomNumberGenerator
{
    public static ulong GetUInt64();
    public static ulong GetUInt64(uint toExclusive);

    public static int GetInt32();

    public static uint GetUInt32();
    public static uint GetUInt32(uint toExclusive);
    public static uint GetUInt32(uint fromInclusive, uint toExclusive);

    public static long GetInt64();
    public static long GetInt64(long toExclusive);
    public static long GetInt64(long fromInclusive, long toExclusive);

    public static ulong GetUInt64(uint fromInclusive, uint toExclusive);
}
```

```C#
namespace System;

public partial class Random
{
    public virtual void NextBatchUInt32(uint toExclusive, Span<uint> destination);
    public virtual void NextBatchUInt32(ReadOnlySpan<uint> toExclusives, Span<uint> destination);

    public void NextBatch(int toExclusive, Span<int> destination);
    public void NextBatch(int fromInclusive, int toExclusive, Span<int> destination);

    public void NextBatchUInt32(uint fromInclusive, uint toExclusive, Span<uint> destination);
    public void NextBatchUInt32(ReadOnlySpan<uint> fromInclusives, ReadOnlySpan<uint> toExclusives, Span<uint> destination);
    public void NextBatchInt32(ReadOnlySpan<int> toExclusives, Span<int> destination);
    public void NextBatchInt32(ReadOnlySpan<int> fromInclusives, ReadOnlySpan<int> toExclusives, Span<int> destination);
    public void NextBatchInt64(long toExclusive, Span<long> destination);
    public void NextBatchInt64(long fromInclusive, long toExclusive, Span<long> destination);
    public void NextBatchInt64(ReadOnlySpan<long> toExclusives, Span<long> destination);
    public void NextBatchInt64(ReadOnlySpan<long> fromInclusives, ReadOnlySpan<long> toExclusives, Span<long> destination);
    public virtual void NextBatchUInt64(ulong toExclusive, Span<ulong> destination);
    public void NextBatchUInt64(ulong fromInclusive, ulong toExclusive, Span<ulong> destination);
    public virtual void NextBatchUInt64(ReadOnlySpan<ulong> toExclusives, Span<ulong> destination);
    public void NextBatchUInt64(ReadOnlySpan<ulong> fromInclusives, ReadOnlySpan<ulong> toExclusives, Span<ulong> destination);
}
```
## [Analyzer]: Regex.Matches(...).Count to Regex.Count(...)

**Approved** | [#runtime/111240](https://github.com/dotnet/runtime/issues/111240#issuecomment-2605490705) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h37m11s)

* Looks good as proposed
    - Severity: Suggestion
    - Category: Performance

## [Analyzer] stringBuilder.Append(string.Substring(...)) to stringBuilder.Append(string, ...)

**Rejected** | [#runtime/111243](https://github.com/dotnet/runtime/issues/111243#issuecomment-2605502621) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h44m23s)

* We believe the existing analyzer that suggests `string.AsSpan(...)` is sufficient
## [Analyzer]: Regex.Match(...).Success to Regex.IsMatch

**Approved** | [#runtime/111238](https://github.com/dotnet/runtime/issues/111238#issuecomment-2605481864) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h50m55s)

* Looks good as proposed
    - Severity: Suggestion
    - Category: Performance

## [Analyzer]: Regex.IsMatch guarding Regex.Match

**Approved** | [#runtime/111239](https://github.com/dotnet/runtime/issues/111239) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h52m26s)

## Integrate Reactive's System.Linq.Async APIs into the Base Class Libraries

**Approved** | [#runtime/79782](https://github.com/dotnet/runtime/issues/79782#issuecomment-2605706550) | [Video](https://www.youtube.com/watch?v=afHNkJnLw4Q&t=0h52m35s)

* This would be an upgradable OOB (supports sample frameworks as `IAsyncEnumerable`)
* This won't unify with the existing `System.Linq.Async` but since these are only extension methods (not types) this is practically an implementation detail and the ecosystem can adopt the in-box version over time without harming their ability to operate with existing code
* We made some editorial choices regarding naming and combinatorics of overloads which means we expect people having to make code changes in to move to the in-box version. We believe consistency with the existing Linq methods and naming conventions.
* The maintainers of `System.Linq.Async` are on board with this plan
* We should remove `Min`/`Max` that takes a selector
* We should consider dropping `ToObservable()` because we still don't replace Reactive for observables
* Other decisions from @stephentoub:
    - **Naming.** I've only used the Async suffix on sinks.
    - **Overloads.** Only extensions on `IAsyncEnumerable`, all sync args or all async args.
    - **Comparers.** I've collapsed non-comparer / comparer overloads into a single one with optional comparer.
    - **CancellationToken.** I've only included CancellationToken parameters in the sinks.
    - **Removals.** I removed `Min`/`Max` non-generic overloads in favor of the generic ones.
    - **Additions.** I added `ToAsyncEnumerable(IEnumerable)` and `ToObservable(IAsyncEnumerable)`.
    - **Tweaks.** `OfType` and `Cast` now have two generic parameters.
    - **IGrouping.** I kept things directly equivalent with the synchronous LINQ methods, but there was feedback about possibly either using `IList` instead of `IEnumerable` or using `IAsyncEnumerable` instead of `IEnumerable`. Same for `ILookup`.
    - **SelectMany.** What overloads do we want? I have `Func<..., IEnumerable>`, `Func<..., ValueTask<IEnumerable>>`, and `Func<..., IAsyncEnumerable>`.

```C#
namespace System.Linq;

public static partial class AsyncEnumerable
{
    public static ValueTask<TSource> AggregateAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, TSource, CancellationToken, ValueTask<TSource>> func, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> AggregateAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, TSource, TSource> func, CancellationToken cancellationToken = default);
    public static ValueTask<TAccumulate> AggregateAsync<TSource, TAccumulate>(this IAsyncEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, CancellationToken, ValueTask<TAccumulate>> func, CancellationToken cancellationToken = default);
    public static ValueTask<TAccumulate> AggregateAsync<TSource, TAccumulate>(this IAsyncEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func, CancellationToken cancellationToken = default);
    public static ValueTask<TResult> AggregateAsync<TSource, TAccumulate, TResult>(this IAsyncEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, CancellationToken, ValueTask<TAccumulate>> func, Func<TAccumulate, CancellationToken, ValueTask<TResult>> resultSelector, CancellationToken cancellationToken = default);
    public static ValueTask<TResult> AggregateAsync<TSource, TAccumulate, TResult>(this IAsyncEnumerable<TSource> source, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func, Func<TAccumulate, TResult> resultSelector, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TKey, CancellationToken, ValueTask<TAccumulate>> seedSelector, Func<TAccumulate, TSource, CancellationToken, ValueTask<TAccumulate>> func, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static IAsyncEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, TAccumulate seed, Func<TAccumulate, TSource, CancellationToken, ValueTask<TAccumulate>> func, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static IAsyncEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TKey, TAccumulate> seedSelector, Func<TAccumulate, TSource, TAccumulate> func, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static IAsyncEnumerable<KeyValuePair<TKey, TAccumulate>> AggregateBy<TSource, TKey, TAccumulate>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, TAccumulate seed, Func<TAccumulate, TSource, TAccumulate> func, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static ValueTask<bool> AllAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<bool> AllAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<bool> AnyAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<bool> AnyAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<bool> AnyAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TSource> Append<TSource>(this IAsyncEnumerable<TSource> source, TSource element);
    public static ValueTask<decimal> AverageAsync(this IAsyncEnumerable<decimal> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync(this IAsyncEnumerable<double> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync(this IAsyncEnumerable<int> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync(this IAsyncEnumerable<long> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> AverageAsync(this IAsyncEnumerable<decimal?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync(this IAsyncEnumerable<double?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync(this IAsyncEnumerable<int?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync(this IAsyncEnumerable<long?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float?> AverageAsync(this IAsyncEnumerable<float?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float> AverageAsync(this IAsyncEnumerable<float> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, decimal> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, double> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, long> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, decimal?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, double?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, long?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, float?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, float> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<decimal>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<double>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<int>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<long>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<decimal?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<double?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<int?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<long?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float?> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<float?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float> AverageAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<float>> selector, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TResult> Cast<TSource, TResult>(this IAsyncEnumerable<TSource> source);
    public static IAsyncEnumerable<TSource[]> Chunk<TSource>(this IAsyncEnumerable<TSource> source, int size);
    public static IAsyncEnumerable<TSource> Concat<TSource>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second);
    public static ValueTask<bool> ContainsAsync<TSource>(this IAsyncEnumerable<TSource> source, TSource value, IEqualityComparer<TSource>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<int> CountAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<int> CountAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<int> CountAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static IAsyncEnumerable<KeyValuePair<TKey, int>> CountBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? keyComparer = null) where TKey : notnull;
    public static IAsyncEnumerable<TSource?> DefaultIfEmpty<TSource>(this IAsyncEnumerable<TSource> source);
    public static IAsyncEnumerable<TSource> DefaultIfEmpty<TSource>(this IAsyncEnumerable<TSource> source, TSource defaultValue);
    public static IAsyncEnumerable<TSource> DistinctBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> DistinctBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> Distinct<TSource>(this IAsyncEnumerable<TSource> source, IEqualityComparer<TSource>? comparer = null);
    public static ValueTask<TSource> ElementAtAsync<TSource>(this IAsyncEnumerable<TSource> source, System.Index index, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> ElementAtAsync<TSource>(this IAsyncEnumerable<TSource> source, int index, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> ElementAtOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, System.Index index, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> ElementAtOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, int index, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TResult> Empty<TResult>();
    public static IAsyncEnumerable<TSource> ExceptBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TKey> second, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> ExceptBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TKey> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> Except<TSource>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, IEqualityComparer<TSource>? comparer = null);
    public static ValueTask<TSource> FirstAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> FirstAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> FirstAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> FirstOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, TSource defaultValue, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<IGrouping<TKey, TSource>> GroupBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<IGrouping<TKey, TElement>> GroupBy<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TSource, CancellationToken, ValueTask<TElement>> elementSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupBy<TSource, TKey, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TKey, IEnumerable<TSource>, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<IGrouping<TKey, TElement>> GroupBy<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupBy<TSource, TKey, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TKey, IEnumerable<TSource>, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupBy<TSource, TKey, TElement, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TSource, CancellationToken, ValueTask<TElement>> elementSelector, Func<TKey, IEnumerable<TElement>, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupBy<TSource, TKey, TElement, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, Func<TKey, IEnumerable<TElement>, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector, Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector, Func<TOuter, IEnumerable<TInner>, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, IEnumerable<TInner>, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<(int Index, TSource Item)> Index<TSource>(this IAsyncEnumerable<TSource> source);
    public static IAsyncEnumerable<TSource> IntersectBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TKey> second, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> IntersectBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TKey> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> Intersect<TSource>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, IEqualityComparer<TSource>? comparer = null);
    public static IAsyncEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector, Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector, Func<TOuter, TInner, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> Join<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static ValueTask<TSource> LastAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> LastAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> LastAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> LastOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, TSource defaultValue, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector, Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector, Func<TOuter, TInner?, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> LeftJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter, TInner?, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static ValueTask<long> LongCountAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<long> LongCountAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<long> LongCountAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> MaxAsync(this IAsyncEnumerable<decimal> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> MaxAsync(this IAsyncEnumerable<double> source, CancellationToken cancellationToken = default);
    public static ValueTask<int> MaxAsync(this IAsyncEnumerable<int> source, CancellationToken cancellationToken = default);
    public static ValueTask<long> MaxAsync(this IAsyncEnumerable<long> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> MaxAsync(this IAsyncEnumerable<decimal?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> MaxAsync(this IAsyncEnumerable<double?> source, CancellationToken cancellationToken = default);
    public static ValueTask<int?> MaxAsync(this IAsyncEnumerable<int?> source, CancellationToken cancellationToken = default);
    public static ValueTask<long?> MaxAsync(this IAsyncEnumerable<long?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float?> MaxAsync(this IAsyncEnumerable<float?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float> MaxAsync(this IAsyncEnumerable<float> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MaxAsync<TSource>(this IAsyncEnumerable<TSource> source, IComparer<TSource>? comparer, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MaxAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TResult?> MaxAsync<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TResult>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<TResult?> MaxAsync<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> selector, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MaxByAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MaxByAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> MinAsync(this IAsyncEnumerable<decimal> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> MinAsync(this IAsyncEnumerable<double> source, CancellationToken cancellationToken = default);
    public static ValueTask<int> MinAsync(this IAsyncEnumerable<int> source, CancellationToken cancellationToken = default);
    public static ValueTask<long> MinAsync(this IAsyncEnumerable<long> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> MinAsync(this IAsyncEnumerable<decimal?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> MinAsync(this IAsyncEnumerable<double?> source, CancellationToken cancellationToken = default);
    public static ValueTask<int?> MinAsync(this IAsyncEnumerable<int?> source, CancellationToken cancellationToken = default);
    public static ValueTask<long?> MinAsync(this IAsyncEnumerable<long?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float?> MinAsync(this IAsyncEnumerable<float?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float> MinAsync(this IAsyncEnumerable<float> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MinAsync<TSource>(this IAsyncEnumerable<TSource> source, IComparer<TSource>? comparer, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MinAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TResult?> MinAsync<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TResult>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<TResult?> MinAsync<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> selector, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MinByAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> MinByAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TResult> OfType<TSource, TResult>(this IAsyncEnumerable<TSource> source);
    public static IOrderedAsyncEnumerable<TSource> OrderByDescending<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> OrderByDescending<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> OrderBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> OrderBy<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<T> OrderDescending<T>(this IAsyncEnumerable<T> source, IComparer<T>? comparer = null);
    public static IOrderedAsyncEnumerable<T> Order<T>(this IAsyncEnumerable<T> source, IComparer<T>? comparer = null);
    public static IAsyncEnumerable<TSource> Prepend<TSource>(this IAsyncEnumerable<TSource> source, TSource element);
    public static IAsyncEnumerable<int> Range(int start, int count);
    public static IAsyncEnumerable<TResult> Repeat<TResult>(TResult element, int count);
    public static IAsyncEnumerable<TSource> Reverse<TSource>(this IAsyncEnumerable<TSource> source);
    public static IAsyncEnumerable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, CancellationToken, ValueTask<TKey>> outerKeySelector, Func<TInner, CancellationToken, ValueTask<TKey>> innerKeySelector, Func<TOuter?, TInner, CancellationToken, ValueTask<TResult>> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> RightJoin<TOuter, TInner, TKey, TResult>(this IAsyncEnumerable<TOuter> outer, IAsyncEnumerable<TInner> inner, Func<TOuter, TKey> outerKeySelector, Func<TInner, TKey> innerKeySelector, Func<TOuter?, TInner, TResult> resultSelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, IAsyncEnumerable<TResult>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, IEnumerable<TResult>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, IAsyncEnumerable<TResult>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, IEnumerable<TResult>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<IEnumerable<TResult>>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<IEnumerable<TResult>>> selector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, IAsyncEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, CancellationToken, ValueTask<TResult>> resultSelector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, IAsyncEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, CancellationToken, ValueTask<TResult>> resultSelector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, IEnumerable<TCollection>> collectionSelector, Func<TSource, TCollection, TResult> resultSelector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<IEnumerable<TCollection>>> collectionSelector, Func<TSource, TCollection, CancellationToken, ValueTask<TResult>> resultSelector);
    public static IAsyncEnumerable<TResult> SelectMany<TSource, TCollection, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<IEnumerable<TCollection>>> collectionSelector, Func<TSource, TCollection, CancellationToken, ValueTask<TResult>> resultSelector);
    public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<TResult>> selector);
    public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, int, TResult> selector);
    public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TResult>> selector);
    public static IAsyncEnumerable<TResult> Select<TSource, TResult>(this IAsyncEnumerable<TSource> source, Func<TSource, TResult> selector);
    public static ValueTask<bool> SequenceEqualAsync<TSource>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, IEqualityComparer<TSource>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate, TSource defaultValue, CancellationToken cancellationToken = default);
    public static ValueTask<TSource?> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<TSource> SingleOrDefaultAsync<TSource>(this IAsyncEnumerable<TSource> source, TSource defaultValue, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TSource> SkipLast<TSource>(this IAsyncEnumerable<TSource> source, int count);
    public static IAsyncEnumerable<TSource> SkipWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate);
    public static IAsyncEnumerable<TSource> SkipWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, bool> predicate);
    public static IAsyncEnumerable<TSource> SkipWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<TSource> SkipWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<TSource> Skip<TSource>(this IAsyncEnumerable<TSource> source, int count);
    public static ValueTask<decimal> SumAsync(this IAsyncEnumerable<decimal> source, CancellationToken cancellationToken = default);
    public static ValueTask<double> SumAsync(this IAsyncEnumerable<double> source, CancellationToken cancellationToken = default);
    public static ValueTask<int> SumAsync(this IAsyncEnumerable<int> source, CancellationToken cancellationToken = default);
    public static ValueTask<long> SumAsync(this IAsyncEnumerable<long> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> SumAsync(this IAsyncEnumerable<decimal?> source, CancellationToken cancellationToken = default);
    public static ValueTask<double?> SumAsync(this IAsyncEnumerable<double?> source, CancellationToken cancellationToken = default);
    public static ValueTask<int?> SumAsync(this IAsyncEnumerable<int?> source, CancellationToken cancellationToken = default);
    public static ValueTask<long?> SumAsync(this IAsyncEnumerable<long?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float?> SumAsync(this IAsyncEnumerable<float?> source, CancellationToken cancellationToken = default);
    public static ValueTask<float> SumAsync(this IAsyncEnumerable<float> source, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, decimal> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, double> selector, CancellationToken cancellationToken = default);
    public static ValueTask<int> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int> selector, CancellationToken cancellationToken = default);
    public static ValueTask<long> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, long> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, decimal?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, double?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<int?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<long?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, long?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, float?> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, float> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<decimal>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<double>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<int> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<int>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<long> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<long>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<decimal?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<decimal?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<double?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<double?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<int?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<int?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<long?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<long?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float?> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<float?>> selector, CancellationToken cancellationToken = default);
    public static ValueTask<float> SumAsync<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<float>> selector, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TSource> TakeLast<TSource>(this IAsyncEnumerable<TSource> source, int count);
    public static IAsyncEnumerable<TSource> TakeWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate);
    public static IAsyncEnumerable<TSource> TakeWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, bool> predicate);
    public static IAsyncEnumerable<TSource> TakeWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<TSource> TakeWhile<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<TSource> Take<TSource>(this IAsyncEnumerable<TSource> source, int count);
    public static IAsyncEnumerable<TSource> Take<TSource>(this IAsyncEnumerable<TSource> source, System.Range range);
    public static IOrderedAsyncEnumerable<TSource> ThenByDescending<TSource, TKey>(this IOrderedAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> ThenByDescending<TSource, TKey>(this IOrderedAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> ThenBy<TSource, TKey>(this IOrderedAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer = null);
    public static IOrderedAsyncEnumerable<TSource> ThenBy<TSource, TKey>(this IOrderedAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IComparer<TKey>? comparer = null);
    public static ValueTask<TSource[]> ToArrayAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static IAsyncEnumerable<TSource> ToAsyncEnumerable<TSource>(this IEnumerable<TSource> source);
    public static ValueTask<Dictionary<TKey, TValue>> ToDictionaryAsync<TKey, TValue>(this IAsyncEnumerable<KeyValuePair<TKey, TValue>> source, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<Dictionary<TKey, TValue>> ToDictionaryAsync<TKey, TValue>(this IAsyncEnumerable<(TKey Key, TValue Value)> source, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<Dictionary<TKey, TSource>> ToDictionaryAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<Dictionary<TKey, TSource>> ToDictionaryAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<Dictionary<TKey, TElement>> ToDictionaryAsync<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TSource, CancellationToken, ValueTask<TElement>> elementSelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<Dictionary<TKey, TElement>> ToDictionaryAsync<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default) where TKey : notnull;
    public static ValueTask<HashSet<TSource>> ToHashSetAsync<TSource>(this IAsyncEnumerable<TSource> source, IEqualityComparer<TSource>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<List<TSource>> ToListAsync<TSource>(this IAsyncEnumerable<TSource> source, CancellationToken cancellationToken = default);
    public static ValueTask<ILookup<TKey, TSource>> ToLookupAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<ILookup<TKey, TSource>> ToLookupAsync<TSource, TKey>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<ILookup<TKey, TElement>> ToLookupAsync<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, Func<TSource, CancellationToken, ValueTask<TElement>> elementSelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static ValueTask<ILookup<TKey, TElement>> ToLookupAsync<TSource, TKey, TElement>(this IAsyncEnumerable<TSource> source, Func<TSource, TKey> keySelector, Func<TSource, TElement> elementSelector, IEqualityComparer<TKey>? comparer = null, CancellationToken cancellationToken = default);
    public static IObservable<TSource> ToObservable<TSource>(this IAsyncEnumerable<TSource> source);
    public static IAsyncEnumerable<TSource> UnionBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, Func<TSource, CancellationToken, ValueTask<TKey>> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> UnionBy<TSource, TKey>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, Func<TSource, TKey> keySelector, IEqualityComparer<TKey>? comparer = null);
    public static IAsyncEnumerable<TSource> Union<TSource>(this IAsyncEnumerable<TSource> first, IAsyncEnumerable<TSource> second, IEqualityComparer<TSource>? comparer = null);
    public static IAsyncEnumerable<TSource> Where<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, bool> predicate);
    public static IAsyncEnumerable<TSource> Where<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, bool> predicate);
    public static IAsyncEnumerable<TSource> Where<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, int, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<TSource> Where<TSource>(this IAsyncEnumerable<TSource> source, Func<TSource, CancellationToken, ValueTask<bool>> predicate);
    public static IAsyncEnumerable<(TFirst First, TSecond Second)> Zip<TFirst, TSecond>(this IAsyncEnumerable<TFirst> first, IAsyncEnumerable<TSecond> second);
    public static IAsyncEnumerable<(TFirst First, TSecond Second, TThird Third)> Zip<TFirst, TSecond, TThird>(this IAsyncEnumerable<TFirst> first, IAsyncEnumerable<TSecond> second, IAsyncEnumerable<TThird> third);
    public static IAsyncEnumerable<TResult> Zip<TFirst, TSecond, TResult>(this IAsyncEnumerable<TFirst> first, IAsyncEnumerable<TSecond> second, Func<TFirst, TSecond, CancellationToken, ValueTask<TResult>> resultSelector);
    public static IAsyncEnumerable<TResult> Zip<TFirst, TSecond, TResult>(this IAsyncEnumerable<TFirst> first, IAsyncEnumerable<TSecond> second, Func<TFirst, TSecond, TResult> resultSelector);
}
public partial interface IOrderedAsyncEnumerable<out TElement> : IAsyncEnumerable<TElement>
{
    IOrderedAsyncEnumerable<TElement> CreateOrderedAsyncEnumerable<TKey>(Func<TElement, CancellationToken, ValueTask<TKey>> keySelector, IComparer<TKey>? comparer, bool descending);
    IOrderedAsyncEnumerable<TElement> CreateOrderedAsyncEnumerable<TKey>(Func<TElement, TKey> keySelector, IComparer<TKey>? comparer, bool descending);
}
```

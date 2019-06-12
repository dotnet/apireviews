# Nullability Annotations for System.Private.CoreLib

Status: **Review in progress**

## General rules

* We shouldn't implement `IComparable<SomeType?>` and `IEquatable<SomeType?>`
  because many other things are constrained to `where T: IEquatable<T>` which
  the instantiation can't satisfy.
* All `TryParse` method should accept nullable input.
* All `out` parameters should be nullable and marked with `[NotNullWhen(true)]`.
* All `IFormatProviders` should be nullable
* All exception should have nullable `message`, `inner`, and `paramName`
* `Delegate` constructor: `target` should be nullable
* All operators `==`, `!=` should accept nullable reference types
* `BeginXxx`, `EndXxx` should take a nullable callback and nullable state
* Most attributes should probably a non-null values, even though we can't really
  enforce that because the constructors don't run unless someone pulls on the
  attribute. So while the values can be null, we should disallow. Unless, an
  overload exits that makes the value optional, in which case it's probably
  going to return `null` from the property, in which case we might as well
  accept `null`.
* `params` should generally not allow for the array to be `null`, elements could
  be, though. Also, if the calls has no `params`, the compiler will fill in
  `Array.Empty<T>()`.
* Events should accept a null sender
* Properties whose backing fields are nulled out after `Dispose()` shouldn't be
  marked as nullable. Instead, the implementation should bang the nulling out.
  After `Dispose()`, the behavior is undefined.
* Extensions on nullable receivers seem odd, but the compiler will do the right
  thing.

## System.Runtime

[API Ref](System.Runtime.md) |
[Video 1](https://youtu.be/O1LGUD8jL5M?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=411) |
[Video 2](https://youtu.be/MevY_iX6_TQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=97) |
[Video 3](https://youtu.be/nZraFvZgz_Y?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=241) |
[Video 4](https://youtu.be/Oi442B_VQEQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=170) |

* `AppContext.BaseDirectory`: shouldn't be `null`
* `Array.SetValue`: one overload misses a question mark
* `Array.Sort`: Should the `Comparison` argument be nullable? We don't allow it
  in code, but should we?
* Should `Exception.StackTrace` be nullable? Maybe return `String.Empty` upon
  creation?
* `Guid.TryParse()` should be nullable
* `String.Join(Enumerable<string>)` should accept nullable entries
* `String.Compare` doesn't accept null `Culture`, but `Replace` does
* `String.ToLower`/`ToUpper` doesn't accept null `Culture`
* `TimeSpan.TryParse` shouldn't accept `null` format strings. OTOH we accept
  null format strings for `TryParse` and `ToString()`. So maybe it should be
  nullable after all?
* `Type.FindXxx` should take null for `filterCriteria`
* `Type.GetProperty(name, returnType)` should have a nullable return type
* `Type.InvokeMember()` should take `object?[]?`
* `Type.GetTypeFromXxx(throwOnError)` should probably return `Type?`
* `Type.ReflectionOnlyGetType()` should return `Type?`
* `DefaultValueAttribute`.ctor should take a non-null Type
* Why is `Assembly.FullName` nullable?
* Why is `Assembly.ManifestModule` nullable?
* `Assembly.GetModule(string name)` should return `?`
* Why is `Assembly.GetName()` not null when `Assembly.FullName` is nullable?
* `Assembly.GetType` should all return nullable, even the one without `throwOnError`
* `Assembly.LoadFrom` should all accept non-null names.
* `Assembly.LoadModule(name, rawModule)` should accept non-nullables
* `Assembly.ReflectionOnlyLoad/From` should only accept non-null inputs
* `AssemblyName.GetPublicKeyToken()` should be marked nullable
* `CustomAttributeData.AttributeType` should be marked non-null
* Should our derivatives of `MemberInfo` assert `DeclaringType` to be non-null?
* `FieldInfo.SetValue(object, object)` should allow both values to be null
* `IReflect.Invoke`: `args` should be `object?[]?`
* `MethodInfo.ReturnType` should be non-null.
* `PropertyInfo.GetRawConstantValue()` and `GetConstantValue()` should be
  nullable because it's a valid default value.
* `PropertyInfo.SetValue` should take a null index, so `object?[]?`
* `TypeFilter` should take nullable criteria
* `AssemblyTargetedPatchBandAttribute` should take non-null.
* `TargetedPatchingOptOutAttribute` should take non-null.
* `StringBuilder.AppendFormat` should accept a null `IFormatProvider`
* `RuntimeHelpers.GetObjectValue()` should accept null and return null and the
  parameter should be marked with `[NotNullIfNotNull]`
* ~~`RuntimeWrappedException` constructor and property should probably allow
  `null`, but this seems like something we might want to test with C++/CLI or
  IL~~. Runtime doesn't allow for that.
* `IDeserializationCallback.OnDeserialization()` should accept nullable.
* `Encoding`. `BodyName`, `EncodingName`, `HeaderName`, `WebName` should
  probably all return non-null.
* `WaitHandle.SafeWaitHandle` should be marked as a not nullable, with allowing
  the setter to accept null.
* `WaitHandleExtensions.GetSafeWaitHandle()` should return non nullable.

## System.Runtime.Extensions

[API Ref](System.Runtime.Extensions.md) |
[Video 1](https://youtu.be/Oi442B_VQEQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=5045) |
[Video 2](https://youtu.be/CJLCnj82kSA?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=237) |

* `AppDomain.ExecuteAssemblyByName` the `params` should be non-null.
* `Environemnt.Exit` should be marked with `[DoesNotReturnAttribute]`
* `ResolveEventHandler` should return a nullable
* It's a bit odd that `BinaryWriter.Write(string)` doesn't accept `null` when
  `TextWriter.WriteString` does.
* `BufferedStream.UnderlyingStream` should be non-nullable. The implementation
  should bang the nulling out. After `Dispose`, the behavior is undefined.
* `TextWriter.Write[Async][Line](char[], int, int)` should accept a `null`
   buffer because there is an overload that accepts nulls. And that's the
   pattern we've used in other places.
* `WebUtility.HtmlDecode` and `HtmlEncode` should be marked with
  `[NotNullIfNotNull]`
* `WebUtility.UrlDecode`, `UrlDecodeToBytes`, `UrlEncode`, and `UrlEncodeBytes`
  should be marked with `[NotNullIfNotNull]`

## System.Collections

[API Ref](System.Collections.md) |
[Video 1](https://youtu.be/CJLCnj82kSA?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=3572) |

* CollectionExtensions: all methods should be constrained to where: `TKey : notnull`
* `HashSet<T>(int, IEqualityComparer<T>)` should accept a null comparer

## System.Runtime.Intrinsics

[API Ref](System.Runtime.Intrinsics.md) |
[Video 1](https://youtu.be/CJLCnj82kSA?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=237) |

* ~~`Vector64<T>.ToString()` shouldn't accept a nullable~~. Nullable is correct.
* ~~`Vector128<T>.ToString()` shouldn't accept a nullable~~. Nullable is correct.
* ~~`Vector256<T>.ToString()` shouldn't accept a nullable~~. Nullable is correct.

## System.Memory

[API Ref](System.Memory.md) |
[Video](https://youtu.be/ux-NsNHHHD4?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=182)

* `MemoryExtensions.BinarySearch` should mark the comparer as nullable, but the
  code today throws.
* `MemoryExtensions.BinarySearch` currently crashes when looking for a null
  value, which seems broken. We should check what `Array.BinarySearch` does and
  make sure consistent. It seems they should take `null` or we need to change
  the annotation to disallow null.
* Right now the `MemoryExtensions` don't seem to accept spans that can contain
  `null` values, due to the way the generics are expressed. From giving it a
  quick glance in the review, we may want to change that. However, we're unsure
  whether that's even possible.

## System.Threading.*

[API Ref](System.Threading.md) |
[Video](https://youtu.be/ux-NsNHHHD4?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=3857)

* `Overlapped` the `IAsyncResult` should be nullable

## System.Runtime.Loader

[API Ref](System.Runtime.Loader.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=203)

* `AssemblyLoadContext.LoadFromAssemblyName(AssemblyName assemblyName)` is
  marked as non-null. How does that work when the protected worker method
  returns a nullable?

## System.Runtime.InteropServices

[API Ref](System.Runtime.InteropServices.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=928)

* It seems that `BStrWrapper` should accept null values
* It seems `Marshal.GetObjectsForNativeVariants(IntPtr aSrcNativeVariant, int
  cVars)` an return a null array.

## System.Runtime.InteropServices.WindowsRuntime

[API Ref](System.Runtime.InteropServices.WindowsRuntime.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=3768)

* Looks good

## System.Diagnostics.Debug

[API Ref](System.Diagnostics.Debug.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=4135)

* Why is `Debugger.DefaultCategory` return null?
* Why does `DebuggerDisplayAttribute.ctor` accept null?

## System.Diagnostics.StackTrace

[API Ref](System.Diagnostics.StackTrace.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=4816)

* Why is `Stack.GetMethod()` nullable? .NET Framework has a
  `Contract.EnsuresNotNull()` promise.
* `StackFrame.GetFrames()` should return an empty array, not null.

## System.Diagnostics.Contracts

[API Ref](System.Diagnostics.Contracts.md) |
[Video](https://youtu.be/se235MK9PFY?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=5267)

* Assuming everything is meant to be non-null, that looks good.

<!--

## System.Numerics.Vectors

[API Ref](System.Numerics.Vectors.md) |
[Video]()

## System.Reflection.Emit

[API Ref](System.Reflection.Emit.md) |
[Video]()

## System.Reflection.Emit.ILGeneration

[API Ref](System.Reflection.Emit.ILGeneration.md) |
[Video]()

## System.Reflection.Emit.Lightweight

[API Ref](System.Reflection.Emit.Lightweight.md) |
[Video]()

## System.Reflection.Primitives

[API Ref](System.Reflection.Primitives.md) |
[Video]()

## System.Resources.ResourceManager

[API Ref](System.Resources.ResourceManager.md) |
[Video]()

## System.Security.Principal

[API Ref](System.Security.Principal.md) |
[Video]()

## System.Text.Encoding.Extensions

[API Ref](System.Text.Encoding.Extensions.md) |
[Video]()

## System.Buffers

[API Ref](System.Buffers.md) |
[Video]()

## System.Collections.Concurrent

[API Ref](System.Collections.Concurrent.md) |
[Video]()

## System.Diagnostics.Tools

[API Ref](System.Diagnostics.Tools.md) |
[Video]()

## System.Diagnostics.Tracing

[API Ref](System.Diagnostics.Tracing.md) |
[Video]()

-->

## Follow-ups

* We should make sure we have no global suppression for nullable warnings.
* We need to revisit what to do with `: class?` we could also decide to make it
  `: class` and put the question marks in the usage.
* We need another review for the attributes
* Do `AssemblyTargetedPatchBandAttribute` and `TargetedPatchingOptOutAttribute`
  still make sense in .NET Core?
* When `nameof()` is used in attribute applied to the return type, the
  parameters should be in scope. This could break existing scope.
* Should implementations of `IComparer<T>` and `IEqualityComparer<>` follow what
  we do for `IComparable<T>` and `IEqualityComparable<T>` and not use nullable
  annotations but use the attributes?
* ~~Does `nonullable` mean non-nullable reference types only or does it also
  excluded nullable values type?~~ Yes.
* Fix API Reviewer to not crash on attributes.

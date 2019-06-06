# Nullability Annotations for System.Private.CoreLib

Status: **Review in progress**

## General rules

* We shouldn't implement `IComparable<SomeType?>` and `IEquatable<SomeType?>`
  because many other things are constrained to `where T: IEquatable<T>` which
  the instantiation can't satisfy.
* All `TryParse` method should accept nullable input. The `out` should be
  nullable and marked with `[NotNullWhen(true)]`.
* All `IFormatProviders` should be nullable
* All exception should have nullable `message`, `inner`, and `paramName`
* `Delegate` constructor: should `target` be nullable?
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
* Continue with `System.CodeDom.Compiler`

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

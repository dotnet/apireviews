# Nullability Annotations for System.Runtime

Status: **Review in progress** |
[API Ref](System-Runtime.md) |
[Video 1](https://youtu.be/O1LGUD8jL5M?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=411) |
[Video 2](https://youtu.be/MevY_iX6_TQ?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=97) |
[Video 3](https://youtu.be/nZraFvZgz_Y?list=PL1rZQsJPBU2S49OQPjupSJF-qeIEz9_ju&t=241) |

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

## Notes

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
* `Assembly.ReflectionOnlyLoad/From` should only accept null inputs
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

## Follow-ups

* We should make sure we have no global suppression for nullable warnings.
* We need to revisit what to do with `: class?` we could also decide to make it
  `: class` and put the question marks in the usage.
* We need another review for the attributes

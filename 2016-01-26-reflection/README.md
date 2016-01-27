# API Review 2016-01-26

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c3qat6vd0ic623s0t53ccv6fb9k).

## Overview

In this API review we took a look at the differences in reflection between .NET Core and the .NET Framework. Specifically, we compared `TypeInfo` and `Type`.

## Notes

Status: **In Progress** |
[Video](https://plus.google.com/events/c3qat6vd0ic623s0t53ccv6fb9k) |
[API Difference](NetFx-vs-NetCore.md)

This is a continuation of the [previous meeting](https://github.com/dotnet/apireviews/tree/master/2016-01-19-reflection). We reviewed the [API differences between .NET Framework and .NET Core](NetFx-vs-NetCore.md).

1. Equality is odd
    * Can we introduce `==` and `!=` later on?
    * it might be better to introduce `==` and `!=` now and have it call `Equals`
    * However, can't do it for `TypeInfo` because it requires new APIs in .NET Framework
    * For now, we decided to not add any operators because we can't get consistency anyways -- even in .NET Framework `Equals` and `==` don't do the same on the `*Info` classes

2. For overload resolution tie breakers Roslyn needs a way to differentiate
   between a substituted generic and actual type, e.g:

    ```C#
        class Foo<T>
        {
            void Foo(T v);
            void Foo(int v);
        }
    ```

    for `Foo<int>`, overload resolution should pick the second method, instead of failing. The currently use `MetadataToken` to compare the original definitions. Given this API throws in certain cases now (e.g. CoreRT), it would be better if we could introduce a new APIs. However, as mentioned earlier this a separate issue. Filed [#5744](https://github.com/dotnet/corefx/issues/5744).

3. `ObfuscationAttribute` / `ObfuscationAssemblyAttribute`: Do not expose. Not meaningful to the runtime or the framework and usage is quite low.

4. Don't expose module resolve event (should be `AssemblyLoadContext`, if ever)

5. Expose `ICustomAttributeProvider`, but implement explicitly

6. Do not expose `IReflect`, used for `IDispatch`

7. We will expose the overloads that take `Binder` as object and make those extension methods in S.R.TE, which is a compat contract. We should add 'real' overloads that allow people to accomplish the same scenarios but take no Binder the next time we can add APIs to .NET Framework.

8. Expose `TargetException`
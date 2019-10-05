# Quick Reviews 8/29/2017

## Allow fire and forget CancellationTokenRegisteration.Dispose

**Approved** | [#corefx/14903](https://github.com/dotnet/corefx/issues/14903) | [Video](https://www.youtube.com/watch?v=VppFZYG-9cA&t=0h0m0s)

## Update Type.GetMethod() overloads to simplify finding generic methods via reflection

**Approved** | [#corefx/16567](https://github.com/dotnet/corefx/issues/16567#issuecomment-325756004) | [Video](https://www.youtube.com/watch?v=VppFZYG-9cA&t=0h13m18s)

This

```C#
public static Type MakeGenericMethodParameterSignatureType(int position);
```

should be 

```C#
public static Type MakeGenericMethodParameter(int position);
```

We should also rename `genericArity` to `genericParameterCount`:

```C#
public MethodInfo GetMethod(string name, int genericParameterCount, Type[] types)
public MethodInfo GetMethod(string name, int genericParameterCount, Type[] types, ParameterModifier[] modifiers)
public MethodInfo GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
public MethodInfo GetMethod(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
protected virtual MethodInfo GetMethodImpl(string name, int genericParameterCount, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers) => throw new NotSupported;
```

We should also expose a query API that identifies the new positional generic method parameters. We propose to go with some more foundational concept that could equally apply to references to generic type parameters. In the spec above @AtsushiKan used the term signature type. So we settled on:

```C#
public virtual bool IsSignatureType => false;
```
## Need api to determine if a type is "byref-like."

**Approved** | [#corefx/17232](https://github.com/dotnet/corefx/issues/17232#issuecomment-325756873) | [Video](https://www.youtube.com/watch?v=VppFZYG-9cA&t=1h28m34s)

We should align this with the attribute we approved earlier, which is named `IsByRefLikeAttribute`. Since the proposal matches that, it's approved as proposed.
## Add a way to opt out of TargetInvocationException wrapping on late-bound invokes.

**Approved** | [#corefx/22866](https://github.com/dotnet/corefx/issues/22866#issuecomment-325761139) | [Video](https://www.youtube.com/watch?v=VppFZYG-9cA&t=1h31m46s)

Looks good, the only suggestion is to rename `PassExceptions` to `DoNotWrapExceptions`. The rationale being that specifying `~PassExceptions` could be read as catch-all.

```C#
var bf = BindingFlags.Public | BindingFlags.DoNotWrapExceptions;
```
## Add ProcessStartInfo.StandardInputEncoding property

**Approved** | [#corefx/20497](https://github.com/dotnet/corefx/issues/20497#issuecomment-325762992) | [Video](https://www.youtube.com/watch?v=VppFZYG-9cA&t=1h45m55s)

Looks good as proposed.

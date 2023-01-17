# API Review 01/17/2023

## Reflection introspection support for FunctionPointer

**Approved** | [#runtime/69273](https://github.com/dotnet/runtime/issues/69273#issuecomment-1385839280) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=0h0m0s)

Looks good as proposed.

```C#
namespace System
{
    public partial class Type
    {
       public virtual bool IsFunctionPointer { get; }
       public virtual bool IsUnmanagedFunctionPointer { get; }

        // These throw InvalidOperationException if IsFunctionPointer = false:
       public virtual Type GetFunctionPointerReturnType();
       public virtual Type[] GetFunctionPointerParameterTypes();

        // These require a "modified type" to return custom modifier types:
       public virtual Type[] GetRequiredCustomModifiers();
       public virtual Type[] GetOptionalCustomModifiers();
       public virtual Type[] GetFunctionPointerCallingConventions(); // Throws if IsFunctionPointer = false
    }
}

// Return a "modified type" from a field, property or parameter if its type is a:
// - Function pointer type
// - Pointer or array since they may reference a function pointer
// - Parameter or return type from a function pointer
namespace System.Reflection
{
    public partial class FieldInfo
    {
       public virtual Type GetModifiedFieldType() => throw new NotSupportedException();
    }

    public partial class PropertyInfo
    {
       public virtual Type GetModifiedPropertyType() => throw new NotSupportedException();
    }

    public partial class ParameterInfo
    {
       public virtual Type GetModifiedParameterType() => throw new NotSupportedException();
    }
}
```
## Abstract System.Reflection.Emit public types

**NeedsWork** | [#runtime/78542](https://github.com/dotnet/runtime/issues/78542#issuecomment-1385895391) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=0h8m20s)

* Everything that was proposed as (non-virtual) to abstract should instead be non-virtual to virtual (don't mismatch across the ref and src)
* Consider instead using the Template Method Pattern, and introducing the virtualness through (e.g. `protected abstract [AssemblyBuilder::]DefineDynamicModuleCore(string name)`)
* Does everything that is being changed to virtual need to be?  For example, the parameterless ConstructorBuilder.GetILGenerator() probably doesn't need a virtual implementation.
* ModuleBuilder.GetTypeToken(Type) is already public in .NET Framework, but with a different signature, we need to converge (or change the name)
  * Probably applies to the other "new" methods on ModuleBuilder


```diff
namespace System.Reflection.Emit
{
-    public sealed partial class AssemblyBuilder : System.Reflection.Assembly
+    public abstract partial class AssemblyBuilder : System.Reflection.Assembly
    {
-        internal AssemblyBuilder();
+        protected AssemblyBuilder();
-        public System.Reflection.Emit.ModuleBuilder DefineDynamicModule(string name);
+        public virtual System.Reflection.Emit.ModuleBuilder DefineDynamicModule(string name);
-        public System.Reflection.Emit.ModuleBuilder? GetDynamicModule(string name);
+        public virtual System.Reflection.Emit.ModuleBuilder? GetDynamicModule(string name);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
   }
       
-    public sealed partial class ConstructorBuilder : System.Reflection.ConstructorInfo
+    public abstract partial class ConstructorBuilder : System.Reflection.ConstructorInfo
    {
-        internal ConstructorBuilder();
+        protected ConstructorBuilder();
-        public bool InitLocals { get; set; }
+        public virtual bool InitLocals { get; set; }
-        public System.Reflection.Emit.ParameterBuilder DefineParameter(int iSequence, System.Reflection.ParameterAttributes attributes, string? strParamName);
+        public virtual System.Reflection.Emit.ParameterBuilder DefineParameter(int iSequence, System.Reflection.ParameterAttributes attributes, string? strParamName);
-        public System.Reflection.Emit.ILGenerator GetILGenerator();
+        public virtual ILGenerator GetILGenerator();
-        public System.Reflection.Emit.ILGenerator GetILGenerator(int streamSize);
+        public virtual System.Reflection.Emit.ILGenerator GetILGenerator(int streamSize);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetImplementationFlags(System.Reflection.MethodImplAttributes attributes;
+        public virtual void SetImplementationFlags(System.Reflection.MethodImplAttributes attributes);
    }
    
-    public sealed partial class EnumBuilder : System.Reflection.TypeInfo
+    public abstract partial class EnumBuilder : System.Reflection.TypeInfo
    {
-        internal EnumBuilder();
+        protected EnumBuilder();
-        public System.Reflection.Emit.FieldBuilder UnderlyingField { get; }
+        public virtual System.Reflection.Emit.FieldBuilder UnderlyingField { get ; }
-        public System.Reflection.TypeInfo CreateTypeInfo();
+        public virtual System.Reflection.TypeInfo CreateTypeInfo();
-        public System.Reflection.Emit.FieldBuilder DefineLiteral(string literalName, object? literalValue);
+        public virtual System.Reflection.Emit.FieldBuilder DefineLiteral(string literalName, object? literalValue);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
    }
    
-    public sealed partial class EventBuilder
+    public abstract partial class EventBuilder
    {
-        internal EventBuilder();
+        protected EventBuilder();
-        public void AddOtherMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void AddOtherMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
-        public void SetAddOnMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void SetAddOnMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetRaiseMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void SetRaiseMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
-        public void SetRemoveOnMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void SetRemoveOnMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
    }
    
-    public sealed partial class FieldBuilder : System.Reflection.FieldInfo
+    public abstract partial class FieldBuilder : System.Reflection.FieldInfo
    {
-        internal FieldBuilder();
+        protected FieldBuilder();
-        public void SetConstant(object? defaultValue);
+        public virtual void SetConstant(object? defaultValue);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder)
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetOffset(int iOffset);
+        public virtual void SetOffset(int iOffset);
    }
    
-    public sealed partial class GenericTypeParameterBuilder : System.Reflection.TypeInfo
+    public abstract partial class GenericTypeParameterBuilder : System.Reflection.TypeInfo
    {
-        internal GenericTypeParameterBuilder();
+        protected GenericTypeParameterBuilder();
-         public void SetBaseTypeConstraint([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? baseTypeConstraint);
+        public virtual void SetBaseTypeConstraint([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? baseTypeConstraint);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetGenericParameterAttributes(System.Reflection.GenericParameterAttributes genericParameterAttributes);
+        public virtual void SetGenericParameterAttributes(System.Reflection.GenericParameterAttributes genericParameterAttributes);
-        public void SetInterfaceConstraints(params System.Type[]? interfaceConstraints);
+        public virtual void SetInterfaceConstraints(params System.Type[]? interfaceConstraints);
    }
    
-    public sealed partial class MethodBuilder : System.Reflection.MethodInfo
+    public abstract partial class MethodBuilder : System.Reflection.MethodInfo
    {
-        internal MethodBuilder();
+        protected MethodBuilder();
-        public bool InitLocals { get; set; }
+        public virtual bool InitLocals { get; set; }
-        public System.Reflection.Emit.GenericTypeParameterBuilder[] DefineGenericParameters(params string[] names);
+        public virtual System.Reflection.Emit.GenericTypeParameterBuilder[] DefineGenericParameters(params string[] names);
-        public System.Reflection.Emit.ParameterBuilder DefineParameter(int position, System.Reflection.ParameterAttributes attributes, string? strParamName);
+        public virtual System.Reflection.Emit.ParameterBuilder DefineParameter(int position, System.Reflection.ParameterAttributes attributes, string? strParamName);
-        public System.Reflection.Emit.ILGenerator GetILGenerator();
+        public virtual System.Reflection.Emit.ILGenerator GetILGenerator();
-        public System.Reflection.Emit.ILGenerator GetILGenerator(int size);
+        public virtual System.Reflection.Emit.ILGenerator GetILGenerator(int size);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetImplementationFlags(System.Reflection.MethodImplAttributes attributes);
+        public virtual void SetImplementationFlags(System.Reflection.MethodImplAttributes attributes);
-        public void SetParameters(params System.Type[] parameterTypes);
+        public virtual void SetParameters(params System.Type[] parameterTypes);
-        public void SetReturnType(System.Type? returnType);
+        public virtual void SetReturnType(System.Type? returnType);
-        public void SetSignature(System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
+        public virtual void SetSignature(System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
    }
    
-    public partial class ModuleBuilder : System.Reflection.Module
+    public abstract partial class ModuleBuilder : System.Reflection.Module
    {
-        internal ModuleBuilder();
+        protected ModuleBuilder();
-        public void CreateGlobalFunctions();
+        public virtual void CreateGlobalFunctions();
-        public System.Reflection.Emit.EnumBuilder DefineEnum(string name, System.Reflection.TypeAttributes visibility, System.Type underlyingType);
+        public virtual System.Reflection.Emit.EnumBuilder DefineEnum(string name, System.Reflection.TypeAttributes visibility, System.Type underlyingType);
-        public System.Reflection.Emit.MethodBuilder DefineGlobalMethod(string name, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? requiredReturnTypeCustomModifiers, System.Type[]? optionalReturnTypeCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? requiredParameterTypeCustomModifiers, System.Type[][]? optionalParameterTypeCustomModifiers);
+        public virtual System.Reflection.Emit.MethodBuilder DefineGlobalMethod(string name, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? requiredReturnTypeCustomModifiers, System.Type[]? optionalReturnTypeCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? requiredParameterTypeCustomModifiers, System.Type[][]? optionalParameterTypeCustomModifiers);
-        public System.Reflection.Emit.FieldBuilder DefineInitializedData(string name, byte[] data, System.Reflection.FieldAttributes attributes) ;
+        public virtual System.Reflection.Emit.FieldBuilder DefineInitializedData(string name, byte[] data, System.Reflection.FieldAttributes attributes);
-        public System.Reflection.Emit.MethodBuilder DefinePInvokeMethod(string name, string dllName, string entryName, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? parameterTypes, System.Runtime.InteropServices.CallingConvention nativeCallConv, System.Runtime.InteropServices.CharSet nativeCharSet);
+        public virtual System.Reflection.Emit.MethodBuilder DefinePInvokeMethod(string name, string dllName, string entryName, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? parameterTypes, System.Runtime.InteropServices.CallingConvention nativeCallConv, System.Runtime.InteropServices.CharSet nativeCharSet);
-        public System.Reflection.Emit.TypeBuilder DefineType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Reflection.Emit.PackingSize packingSize, int typesize);
+        public virtual System.Reflection.Emit.TypeBuilder DefineType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Reflection.Emit.PackingSize packingSize, int typesize);
-        public System.Reflection.Emit.TypeBuilder DefineType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Type[]? interfaces);
+        public virtual System.Reflection.Emit.TypeBuilder DefineType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Type[]? interfaces);
-        public System.Reflection.Emit.FieldBuilder DefineUninitializedData(string name, int size, System.Reflection.FieldAttributes attributes);
+        public virtual System.Reflection.Emit.FieldBuilder DefineUninitializedData(string name, int size, System.Reflection.FieldAttributes attributes);
-        public System.Reflection.MethodInfo GetArrayMethod(System.Type arrayClass, string methodName, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? parameterTypes);
+        public virtual System.Reflection.MethodInfo GetArrayMethod(System.Type arrayClass, string methodName, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? parameterTypes);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);

// New public APIs: these are existing internal methods used outside of the ModuleBuilder, so needs to make public 
+        public virtual int GetTypeToken(Type type);
+        public virtual int GetMethodToken(MethodInfo method);
+        public virtual int GetFieldToken(FieldInfo field);
+        public virtual int GetConstructorToken(ConstructorInfo con);
+        public virtual int GetSignatureToken(SignatureHelper sigHelper);
+        public virtual int GetStringConstant(string str);
    }
    
-    public sealed partial class PropertyBuilder : System.Reflection.PropertyInfo
+    public abstract partial class PropertyBuilder : System.Reflection.PropertyInfo
    {
-        internal PropertyBuilder();
+        protected PropertyBuilder();
-        public void AddOtherMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void AddOtherMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
-        public void SetConstant(object? defaultValue);
+        public virtual void SetConstant(object? defaultValue);
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetGetMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void SetGetMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
-        public void SetSetMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
+        public virtual void SetSetMethod(System.Reflection.Emit.MethodBuilder mdBuilder);
    }
   
-   public sealed partial class TypeBuilder : System.Reflection.TypeInfo
+   public abstract partial class TypeBuilder : System.Reflection.TypeInfo
    {
-        internal TypeBuilder();
+        protected TypeBuilder();
-        public System.Reflection.Emit.PackingSize PackingSize { get; }
+        public virtual System.Reflection.Emit.PackingSize PackingSize { get; }
-        public int Size { get; }
+        public virtual int Size { get; }
-        public void AddInterfaceImplementation([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type interfaceType);
+        public virtual void AddInterfaceImplementation([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type interfaceType);
-        public System.Reflection.TypeInfo CreateTypeInfo();
+        public virtual System.Reflection.TypeInfo CreateTypeInfo();
-        public System.Reflection.Emit.ConstructorBuilder DefineConstructor(System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type[]? parameterTypes, System.Type[][]? requiredCustomModifiers, System.Type[][]? optionalCustomModifiers);
+        public virtual System.Reflection.Emit.ConstructorBuilder DefineConstructor(System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type[]? parameterTypes, System.Type[][]? requiredCustomModifiers, System.Type[][]? optionalCustomModifiers);
-        public System.Reflection.Emit.ConstructorBuilder DefineDefaultConstructor(System.Reflection.MethodAttributes attributes);
+        public virtual System.Reflection.Emit.ConstructorBuilder DefineDefaultConstructor(System.Reflection.MethodAttributes attributes);
-        public System.Reflection.Emit.EventBuilder DefineEvent(string name, System.Reflection.EventAttributes attributes, System.Type eventtype);
+        public virtual System.Reflection.Emit.EventBuilder DefineEvent(string name, System.Reflection.EventAttributes attributes, System.Type eventtype);
-        public System.Reflection.Emit.FieldBuilder DefineField(string fieldName, System.Type type, System.Type[]? requiredCustomModifiers, System.Type[]? optionalCustomModifiers, System.Reflection.FieldAttributes attributes) { throw null; }
+        public virtual System.Reflection.Emit.FieldBuilder DefineField(string fieldName, System.Type type, System.Type[]? requiredCustomModifiers, System.Type[]? optionalCustomModifiers, System.Reflection.FieldAttributes attributes);
-        public System.Reflection.Emit.GenericTypeParameterBuilder[] DefineGenericParameters(params string[] names);
+        public virtual System.Reflection.Emit.GenericTypeParameterBuilder[] DefineGenericParameters(params string[] names);
-        public System.Reflection.Emit.FieldBuilder DefineInitializedData(string name, byte[] data, System.Reflection.FieldAttributes attributes);
+        public virtual System.Reflection.Emit.FieldBuilder DefineInitializedData(string name, byte[] data, System.Reflection.FieldAttributes attributes);
-        public System.Reflection.Emit.MethodBuilder DefineMethod(string name, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
+        public virtual System.Reflection.Emit.MethodBuilder DefineMethod(string name, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
-        public void DefineMethodOverride(System.Reflection.MethodInfo methodInfoBody, System.Reflection.MethodInfo methodInfoDeclaration);
+        public virtual void DefineMethodOverride(System.Reflection.MethodInfo methodInfoBody, System.Reflection.MethodInfo methodInfoDeclaration);
-        public System.Reflection.Emit.TypeBuilder DefineNestedType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Reflection.Emit.PackingSize packSize, int typeSize);
+        public virtual System.Reflection.Emit.TypeBuilder DefineNestedType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Reflection.Emit.PackingSize packSize, int typeSize);
-        public System.Reflection.Emit.TypeBuilder DefineNestedType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Type[]? interfaces);
+        public virtual System.Reflection.Emit.TypeBuilder DefineNestedType(string name, System.Reflection.TypeAttributes attr, [System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent, System.Type[]? interfaces);
-        public System.Reflection.Emit.MethodBuilder DefinePInvokeMethod(string name, string dllName, string entryName, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers, System.Runtime.InteropServices.CallingConvention nativeCallConv, System.Runtime.InteropServices.CharSet nativeCharSet);
+        public virtual System.Reflection.Emit.MethodBuilder DefinePInvokeMethod(string name, string dllName, string entryName, System.Reflection.MethodAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type? returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers, System.Runtime.InteropServices.CallingConvention nativeCallConv, System.Runtime.InteropServices.CharSet nativeCharSet);
-        public System.Reflection.Emit.PropertyBuilder DefineProperty(string name, System.Reflection.PropertyAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
+        public virtual System.Reflection.Emit.PropertyBuilder DefineProperty(string name, System.Reflection.PropertyAttributes attributes, System.Reflection.CallingConventions callingConvention, System.Type returnType, System.Type[]? returnTypeRequiredCustomModifiers, System.Type[]? returnTypeOptionalCustomModifiers, System.Type[]? parameterTypes, System.Type[][]? parameterTypeRequiredCustomModifiers, System.Type[][]? parameterTypeOptionalCustomModifiers);
-        public System.Reflection.Emit.ConstructorBuilder DefineTypeInitializer();
+        public virtual System.Reflection.Emit.ConstructorBuilder DefineTypeInitializer();
-        public System.Reflection.Emit.FieldBuilder DefineUninitializedData(string name, int size, System.Reflection.FieldAttributes attributes);
+        public virtual System.Reflection.Emit.FieldBuilder DefineUninitializedData(string name, int size, System.Reflection.FieldAttributes attributes);
-        public bool IsCreated();
+        public virtual bool IsCreated();
-        public void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
+        public virtual void SetCustomAttribute(System.Reflection.ConstructorInfo con, byte[] binaryAttribute);
-        public void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
+        public virtual void SetCustomAttribute(System.Reflection.Emit.CustomAttributeBuilder customBuilder);
-        public void SetParent([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent);
+        public virtual void SetParent([System.Diagnostics.CodeAnalysis.DynamicallyAccessedMembersAttribute(System.Diagnostics.CodeAnalysis.DynamicallyAccessedMemberTypes.All)] System.Type? parent);
    }
}
```
## System.Text.Json fast path serialization not working with combined contexts

**Approved** | [#runtime/71933](https://github.com/dotnet/runtime/issues/71933#issuecomment-1385920314) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=0h44m31s)

Looks good as proposed.  Based on discussion it seems like EditorBrowsable(Never) is justified, since it's mostly distracting to callers outside of the serializer/sourcegen dichotomy.

```C#
namespace System.Text.Json.Serialization.Metadata;

public class JsonTypeInfo
{
    [EditorBrowsable(EditorBrowsableState.Never)]
    public IJsonTypeInfoResolver? OriginatingResolver { get; set; } = null;
}
```
## Boost performance of localized, non-static or incremental string formatting.

**Approved** | [#runtime/50330](https://github.com/dotnet/runtime/issues/50330#issuecomment-1385963832) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=1h1m9s)

Looks good as proposed

* We discussed whether CompositeFormat could just be FormattableString, and the answer was no.
* We discussed namespaces for FormattableString/StandardFormat/CompositeFormat and didn't feel like there was a better place for the new type than System.Text.
* We discussed the TArg0...TArgN being after the `out` parameter for TryWrite, and decided it is correct.

```C#
namespace System
{
    public class String
    {
        public static string Format<TArg0>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0);
        public static string Format<TArg0, TArg1>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0, TArg1 arg1);
        public static string Format<TArg0, TArg1, TArg2>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0, TArg1 arg1, TArg2 arg2);
        public static string Format(IFormatProvider? provider, CompositeFormat format, params object?[] args);
        public static string Format(IFormatProvider? provider, CompositeFormat format, ReadOnlySpan<object?> args);
    }

    public static class MemoryExtensions
    {
        public static bool TryWrite<TArg0>(this Span<char> destination, IFormatProvider? provider, CompositeFormat format, out int charsWritten, TArg0 arg);
        public static bool TryWrite<TArg0, TArg1>(this Span<char> destination, IFormatProvider? provider, CompositeFormat format, out int charsWritten, TArg0 arg, TArg1 arg1);
        public static bool TryWrite<TArg0, TArg1, TArg2>(this Span<char> destination, IFormatProvider? provider, CompositeFormat format, out int charsWritten, TArg0 arg, TArg1 arg1, TArg2 arg2);
        public static bool TryWrite(this Span<char> destination, IFormatProvider? provider, CompositeFormat format, out int charsWritten, params object?[] args);
        public static bool TryWrite(this Span<char> destination, IFormatProvider? provider, CompositeFormat format, out int charsWritten, ReadOnlySpan<object?> args);
    }
}

namespace System.Text
{
    public class StringBuilder
    {
        public static StringBuilder AppendFormat<TArg0>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0);
        public static StringBuilder AppendFormat<TArg0, TArg1>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0, TArg1 arg1);
        public static StringBuilder AppendFormat<TArg0, TArg1, TArg2>(IFormatProvider? provider, CompositeFormat format, TArg0 arg0, TArg1 arg1, TArg2 arg2);
        public static StringBuilder AppendFormat(IFormatProvider? provider, CompositeFormat format, params object?[] args);
        public static StringBuilder AppendFormat(IFormatProvider? provider, CompositeFormat format, ReadOnlySpan<object?> args);
    }
}

namespace System.Text
{
    public sealed class CompositeFormat
    {
        public static CompositeFormat Parse([StringSyntax(StringSyntaxAttribute.CompositeFormat)] string format);
        public static bool TryParse([StringSyntax(StringSyntaxAttribute.CompositeFormat)] string format, [NotNullWhen(true)] out CompositeFormat? compositeFormat);

        public string Format { get; }
    }
}
```
## Matrix4x4.CreateViewport

**Approved** | [#runtime/79961](https://github.com/dotnet/runtime/issues/79961#issuecomment-1385970279) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=1h40m1s)

Looks good as proposed

```C#
namespace System.Numerics;

public partial struct Matrix4x4
{
    public static Matrix4x4 CreateViewport(float x, float y, float width, float height, float minDepth, float maxDepth);
}
```
## IFloatingPointIeee754<T>.Lerp

**Approved** | [#runtime/80044](https://github.com/dotnet/runtime/issues/80044#issuecomment-1385986292) | [Video](https://www.youtube.com/watch?v=vPr5M94iSro&t=1h45m49s)

Looks good as proposed

* Our existing Lerp implementations seem to use value1/value2 everywhere, and there's not an industry standard naming pair.
* This will be our first time ever that we've modified a shipped interface with a DIM.
  * Time to learn!

```C#
namespace System.Numerics
{
    public interface IFloatingPointIeee754<T> where T : IFloatingPointIeee754<T>?
    {
        static virtual T Lerp(T value1, T value2, T amount) => (value1 * (T.One - amount)) + (value2 * amount);
    }
}

namespace System
{
    public readonly struct Half : IFloatingPointIeee754<Half>
    {
        public static Half Lerp(Half value1, Half value2, Half amount)
        {
            return value1 * (One - amount) + value2 * amount;
        }
    }

    public readonly struct Single : IFloatingPointIeee754<float>
    {
        public static float Lerp(float value1, float value2, float amount)
        {
            return value1 * (One - amount) + value2 * amount;
        }
    }

    public readonly struct Double : IFloatingPointIeee754<double>
    {
        public static double Lerp(double value1, double value2, double amount)
        {
            return value1 * (One - amount) + value2 * amount;
        }
    }
}

namespace System.Runtime.InteropServices
{
    public readonly struct NFloat : IFloatingPointIeee754<NFloat>
    {
        public static NFloat Lerp(NFloat value1, NFloat value2, NFloat amount)
        {
            return value1 * (One - amount) + value2 * amount;
        }
    }
}
```

# API Review 2016-01-19

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c4vg1g7j7nhbf7b19o6ou6vdhhs).

## Overview

In this API review we took a look at the differences in reflection between .NET Core and the .NET Framework. Specifically, we compared `TypeInfo` and `Type`.

## Notes

Status: **In Progress**

As outlined in our [porting notes](https://github.com/dotnet/corefx/pull/5480), we generally want to bring .NET Core closer to the API richness of the .NET Framework. However, .NET Core and .NET Framework have different design goals. We need to be mindful to make sure we don't regress the advancements we made in .NET Core, particularly around the factoring and componentization.

### Stakes in the ground

* We want to maintain the split between `Type` and `TypeInfo`, i.e. we don't intend to bring back reflection APIs directly on `Type`
* We don't want to allow developers to extend the reflection object model by inheritance, so we don't need to expose any of the protected `*Impl` methods.
* We generally don't want make changes that require adding APIs to the .NET Framework, because that would limit portability between .NET Core and .NET Framework to higher versions only.
* While we can't implement `MetadataToken` on .NET Native, we think it's better to expose and throw because it, especially as it also does it on the .NE Framework in certain cases. If this becomes a problem, we can add a a capability API, such as `HasMetadataToken`.
* We'd like to avoid adding support for the `Binder` type. However, many of the reflection overloads require one to be passed, although they accept `null`, which is what most consumers pass in. We could support these API by simply adding a dummy version of the `Binder` class (i.e. no members or public constructor).
* We don't want to expose the `CLSID` and `ProgID` APIs because they really belong in interop and are also exposed on `Marshal.`

### Open issues

* When we bring back APIs, we need to make sure we don't have conflicts with
  `System.Reflection.Extensions` and `System.Reflection.TypeExtensions`.
* We need to close on what to do with `Binder`
    - Add them as extension methods in `System.Reflection.TypeExtensions`
    - Add replacement overloads that don't take `Binder`
    - Dummy out `Binder`
* Check that most, if not all, of the APIS that are on `Type`, are also on `TypeInfo`

### Comparison with notes

```diff
--- System.Reflection.TypeInfo in .NET Core
+++ System.Type in .NET Framework
@@ -1,41 +1,44 @@
-    public abstract class TypeInfo : MemberInfo {
+    public abstract class Type : MemberInfo {
         // Expose on Type
+        public static readonly char Delimiter;
         // Should already be in .NET Core's Type
+        public static readonly object Missing;
         // Expose on TypeInfo
+        public static readonly MemberFilter FilterAttribute;
         // Expose on TypeInfo
+        public static readonly MemberFilter FilterName;
         // Expose on TypeInfo
+        public static readonly MemberFilter FilterNameIgnoreCase;
         // Should already be in .NET Core's Type
+        public static readonly Type[] EmptyTypes;
         // Don't expose a constructor
+        protected Type();
         public abstract Assembly Assembly { get; }
         public abstract string AssemblyQualifiedName { get; }
-        public abstract TypeAttributes Attributes { get; }
+        public TypeAttributes Attributes { get; }
         public abstract Type BaseType { get; }
-        public abstract bool ContainsGenericParameters { get; }
-        public virtual IEnumerable<ConstructorInfo> DeclaredConstructors { get; }
-        public virtual IEnumerable<EventInfo> DeclaredEvents { get; }
-        public virtual IEnumerable<FieldInfo> DeclaredFields { get; }
-        public virtual IEnumerable<MemberInfo> DeclaredMembers { get; }
-        public virtual IEnumerable<MethodInfo> DeclaredMethods { get; }
-        public virtual IEnumerable<TypeInfo> DeclaredNestedTypes { get; }
-        public virtual IEnumerable<PropertyInfo> DeclaredProperties { get; }
-        public abstract MethodBase DeclaringMethod { get; }
+        public virtual bool ContainsGenericParameters { get; }
         // Currently an extension method
+        public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
+        public virtual MethodBase DeclaringMethod { get; }
         // Already on Type
+        public override Type DeclaringType { get; }
         // Don't expose
+        public static Binder DefaultBinder { get; }
         public abstract string FullName { get; }
-        public abstract GenericParameterAttributes GenericParameterAttributes { get; }
-        public abstract int GenericParameterPosition { get; }
-        public abstract Type[] GenericTypeArguments { get; }
-        public virtual Type[] GenericTypeParameters { get; }
+        public virtual GenericParameterAttributes GenericParameterAttributes { get; }
+        public virtual int GenericParameterPosition { get; }
+        public virtual Type[] GenericTypeArguments { get; }
         public abstract Guid GUID { get; }
         public bool HasElementType { get; }
-        public virtual IEnumerable<Type> ImplementedInterfaces { get; }
         public bool IsAbstract { get; }
         public bool IsAnsiClass { get; }
         public bool IsArray { get; }
         public bool IsAutoClass { get; }
         public bool IsAutoLayout { get; }
         public bool IsByRef { get; }
         public bool IsClass { get; }
-        public virtual bool IsCOMObject { get; }
-        public abstract bool IsEnum { get; }
+        public bool IsCOMObject { get; }
         // Expose on TypeInfo
+        public virtual bool IsConstructedGenericType { get; }
         // Don't expose. Is related to remoting.
+        public bool IsContextful { get; }
+        public virtual bool IsEnum { get; }
         public bool IsExplicitLayout { get; }
-        public abstract bool IsGenericParameter { get; }
-        public abstract bool IsGenericType { get; }
-        public abstract bool IsGenericTypeDefinition { get; }
+        public virtual bool IsGenericParameter { get; }
+        public virtual bool IsGenericType { get; }
+        public virtual bool IsGenericTypeDefinition { get; }
         public bool IsImport { get; }
         public bool IsInterface { get; }
         public bool IsLayoutSequential { get; }
         public bool IsMarshalByRef { get; }
         public bool IsNested { get; }
         public bool IsNestedAssembly { get; }
@@ -43,35 +46,155 @@
         public bool IsNestedFamily { get; }
         public bool IsNestedFamORAssem { get; }
         public bool IsNestedPrivate { get; }
         public bool IsNestedPublic { get; }
         public bool IsNotPublic { get; }
         public bool IsPointer { get; }
-        public virtual bool IsPrimitive { get; }
+        public bool IsPrimitive { get; }
         public bool IsPublic { get; }
         public bool IsSealed { get; }
-        public abstract bool IsSerializable { get; }
         // Don't expose
+        public virtual bool IsSecurityCritical { get; }
         // Don't expose
+        public virtual bool IsSecuritySafeCritical { get; }
         // Don't expose
+        public virtual bool IsSecurityTransparent { get; }
+        public virtual bool IsSerializable { get; }
         public bool IsSpecialName { get; }
         public bool IsUnicodeClass { get; }
-        public virtual bool IsValueType { get; }
+        public bool IsValueType { get; }
         public bool IsVisible { get; }
         // Expose on TypeInfo
+        public override MemberTypes MemberType { get; }
         // Can't be implemented in ProjectN, but should be exposed
+        public virtual int MetadataToken { get; }
         // Expose on TypeInfo
+        public abstract new Module Module { get; }
+        public abstract string Name { get; }
         public abstract string Namespace { get; }
-        public virtual Type AsType();
-        public abstract int GetArrayRank();
-        public virtual EventInfo GetDeclaredEvent(string name);
-        public virtual FieldInfo GetDeclaredField(string name);
-        public virtual MethodInfo GetDeclaredMethod(string name);
-        public virtual IEnumerable<MethodInfo> GetDeclaredMethods(string name);
-        public virtual TypeInfo GetDeclaredNestedType(string name);
-        public virtual PropertyInfo GetDeclaredProperty(string name);
         // Usage ~11%. Lot's of hacks. If ProjectN supports it, expose it. Otherwise don't.
+        public override Type ReflectedType { get; }
         // Expose on TypeInfo
+        public virtual StructLayoutAttribute StructLayoutAttribute { get; }
+        public virtual RuntimeTypeHandle TypeHandle { get; }
         // Expose on TypeInfo
+        public ConstructorInfo TypeInitializer { get; }
         // Expose on TypeInfo
+        public abstract Type UnderlyingSystemType { get; }
+        public override bool Equals(object o);
+        public static bool Equals(object objA, object objB);
+        public virtual bool Equals(Type o);
+        ~Object();
         // Expose on TypeInfo
+        public virtual Type[] FindInterfaces(TypeFilter filter, object filterCriteria);
         // Expose on TypeInfo
+        public virtual MemberInfo[] FindMembers(MemberTypes memberType, BindingFlags bindingAttr, MemberFilter filter, object filterCriteria);
+        public virtual int GetArrayRank();
         // Don't expose
+        protected abstract TypeAttributes GetAttributeFlagsImpl();
         // Expose. See discussion on Binder.
+        public ConstructorInfo GetConstructor(BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
         // Expose. See discussion on Binder.
+        public ConstructorInfo GetConstructor(BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
         // Expose on TypeInfo
+        public ConstructorInfo GetConstructor(Type[] types);
         // Don't expose
+        protected abstract ConstructorInfo GetConstructorImpl(BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
+        public ConstructorInfo[] GetConstructors();
+        public abstract ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
         // Can't be added, we already have an extension with the same signatures
+        public abstract object[] GetCustomAttributes(bool inherit);
         // Can't be added, we already have an extension with the same signatures
+        public abstract object[] GetCustomAttributes(Type attributeType, bool inherit);
         // Don't expose. Already a property on TypeInfo
+        public virtual IList<CustomAttributeData> GetCustomAttributesData();
         // Expose on TypeInfo
+        public virtual MemberInfo[] GetDefaultMembers();
         public abstract Type GetElementType();
-        public abstract Type[] GetGenericParameterConstraints();
-        public abstract Type GetGenericTypeDefinition();
-        public virtual bool IsAssignableFrom(TypeInfo typeInfo);
         // Expose on TypeInfo
+        public virtual string GetEnumName(object value);
         // Expose on TypeInfo
+        public virtual string[] GetEnumNames();
         // Expose on TypeInfo
+        public virtual Type GetEnumUnderlyingType();
         // Expose on TypeInfo
+        public virtual Array GetEnumValues();
+        public EventInfo GetEvent(string name);
+        public abstract EventInfo GetEvent(string name, BindingFlags bindingAttr);
+        public virtual EventInfo[] GetEvents();
+        public abstract EventInfo[] GetEvents(BindingFlags bindingAttr);
+        public FieldInfo GetField(string name);
+        public abstract FieldInfo GetField(string name, BindingFlags bindingAttr);
+        public FieldInfo[] GetFields();
+        public abstract FieldInfo[] GetFields(BindingFlags bindingAttr);
+        public virtual Type[] GetGenericArguments();
+        public virtual Type[] GetGenericParameterConstraints();
+        public virtual Type GetGenericTypeDefinition();
+        public override int GetHashCode();
+        public Type GetInterface(string name);
+        public abstract Type GetInterface(string name, bool ignoreCase);
+        public virtual InterfaceMapping GetInterfaceMap(Type interfaceType);
+        public abstract Type[] GetInterfaces();
+        public MemberInfo[] GetMember(string name);
+        public virtual MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
+        public virtual MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
+        public MemberInfo[] GetMembers();
+        public abstract MemberInfo[] GetMembers(BindingFlags bindingAttr);
+        public MethodInfo GetMethod(string name);
+        public MethodInfo GetMethod(string name, BindingFlags bindingAttr);
         // Expose. See discussion on Binder.
+        public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
         // Expose. See discussion on Binder.
+        public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
+        public MethodInfo GetMethod(string name, Type[] types);
+        public MethodInfo GetMethod(string name, Type[] types, ParameterModifier[] modifiers);
         // Don't expose
+        protected abstract MethodInfo GetMethodImpl(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
+        public MethodInfo[] GetMethods();
+        public abstract MethodInfo[] GetMethods(BindingFlags bindingAttr);
+        public Type GetNestedType(string name);
+        public abstract Type GetNestedType(string name, BindingFlags bindingAttr);
+        public Type[] GetNestedTypes();
+        public abstract Type[] GetNestedTypes(BindingFlags bindingAttr);
+        public PropertyInfo[] GetProperties();
+        public abstract PropertyInfo[] GetProperties(BindingFlags bindingAttr);
+        public PropertyInfo GetProperty(string name);
+        public PropertyInfo GetProperty(string name, BindingFlags bindingAttr);
         // Expose. See discussion on Binder.
+        public PropertyInfo GetProperty(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
+        public PropertyInfo GetProperty(string name, Type returnType);
+        public PropertyInfo GetProperty(string name, Type returnType, Type[] types);
+        public PropertyInfo GetProperty(string name, Type returnType, Type[] types, ParameterModifier[] modifiers);
+        public PropertyInfo GetProperty(string name, Type[] types);
         // Don't expose
+        protected abstract PropertyInfo GetPropertyImpl(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
+        public new Type GetType();
         // Expose on TypeInfo
+        public static Type GetType(string typeName);
         // Expose on TypeInfo
+        public static Type GetType(string typeName, bool throwOnError);
         // Expose on TypeInfo
+        public static Type GetType(string typeName, bool throwOnError, bool ignoreCase);
         // Expose on TypeInfo
+        public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver);
         // Expose on TypeInfo
+        public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver, bool throwOnError);
         // Expose on TypeInfo
+        public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver, bool throwOnError, bool ignoreCase);
         // Don't expose
+        public static Type[] GetTypeArray(object[] args);
         // Expose on TypeInfo
+        public static TypeCode GetTypeCode(Type type);
         // Don't expose
+        protected virtual TypeCode GetTypeCodeImpl();
         // Don't expose
+        public static Type GetTypeFromCLSID(Guid clsid);
         // Don't expose
+        public static Type GetTypeFromCLSID(Guid clsid, bool throwOnError);
         // Don't expose
+        public static Type GetTypeFromCLSID(Guid clsid, string server);
         // Don't expose
+        public static Type GetTypeFromCLSID(Guid clsid, string server, bool throwOnError);
         // TODO
+        public static Type GetTypeFromHandle(RuntimeTypeHandle handle);
         // Don't expose
+        public static Type GetTypeFromProgID(string progID);
         // Don't expose
+        public static Type GetTypeFromProgID(string progID, bool throwOnError);
         // Don't expose
+        public static Type GetTypeFromProgID(string progID, string server);
         // Don't expose
+        public static Type GetTypeFromProgID(string progID, string server, bool throwOnError);
         // TODO
+        public static RuntimeTypeHandle GetTypeHandle(object o);
         // Don't expose
+        protected abstract bool HasElementTypeImpl();
         // Expose. See discussion on Binder.
+        public object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args);
         // Expose. See discussion on Binder.
+        public object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, CultureInfo culture);
         // Expose. See discussion on Binder.
+        public abstract object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, ParameterModifier[] modifiers, CultureInfo culture, string[] namedParameters);
         // Don't expose
+        protected abstract bool IsArrayImpl();
+        public virtual bool IsAssignableFrom(Type c);
         // Don't expose
+        protected abstract bool IsByRefImpl();
         // Don't expose
+        protected abstract bool IsCOMObjectImpl();
         // Don't expose
+        protected virtual bool IsContextfulImpl();
         // Expose on TypeInfo
+        public abstract bool IsDefined(Type attributeType, bool inherit);
         // Expose on TypeInfo
+        public virtual bool IsEnumDefined(object value);
         // Expose on TypeInfo
+        public virtual bool IsEquivalentTo(Type other);
         // Should already be there
+        public virtual bool IsInstanceOfType(object o);
         // Don't expose
+        protected virtual bool IsMarshalByRefImpl();
         // Don't expose
+        protected abstract bool IsPointerImpl();
         // Don't expose
+        protected abstract bool IsPrimitiveImpl();
         public virtual bool IsSubclassOf(Type c);
-        public abstract Type MakeArrayType();
-        public abstract Type MakeArrayType(int rank);
-        public abstract Type MakeByRefType();
-        public abstract Type MakeGenericType(params Type[] typeArguments);
-        public abstract Type MakePointerType();
         // Don't expose
+        protected virtual bool IsValueTypeImpl();
+        public virtual Type MakeArrayType();
+        public virtual Type MakeArrayType(int rank);
+        public virtual Type MakeByRefType();
+        public virtual Type MakeGenericType(params Type[] typeArguments);
+        public virtual Type MakePointerType();
+        protected object MemberwiseClone();
         // Shouldn't be exposed -- discuss
+        public static bool operator ==(MemberInfo left, MemberInfo right);
         // Shouldn't be exposed -- discuss
+        public static bool operator ==(Type left, Type right);
         // Shouldn't be exposed -- discuss
+        public static bool operator !=(MemberInfo left, MemberInfo right);
         // Shouldn't be exposed -- discuss
+        public static bool operator !=(Type left, Type right);
+        public static bool ReferenceEquals(object objA, object objB);
         // Don't expose
+        public static Type ReflectionOnlyGetType(string typeName, bool throwIfNotFound, bool ignoreCase);
+        public override string ToString();
     }
```

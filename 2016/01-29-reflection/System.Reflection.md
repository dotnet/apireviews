```diff
---System.Reflection.dll Old
+++System.Reflection.dll New
 namespace System.Reflection {
  public sealed class AmbiguousMatchException : Exception {
    public AmbiguousMatchException();
    public AmbiguousMatchException(string message);
    public AmbiguousMatchException(string message, Exception inner);
  }
  public abstract class Assembly {
+   public virtual string CodeBase { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract IEnumerable<TypeInfo> DefinedTypes { get; }
    public virtual MethodInfo EntryPoint { get; }
+   public virtual string EscapedCodeBase { get; }
    public virtual IEnumerable<Type> ExportedTypes { get; }
    public virtual string FullName { get; }
    public virtual bool IsDynamic { get; }
    public virtual string Location { get; }
    public virtual Module ManifestModule { get; }
    public abstract IEnumerable<Module> Modules { get; }
    public override bool Equals(object o);
    public static Assembly GetEntryAssembly();
    public override int GetHashCode();
    public virtual ManifestResourceInfo GetManifestResourceInfo(string resourceName);
    public virtual string[] GetManifestResourceNames();
    public virtual Stream GetManifestResourceStream(string name);
    public virtual AssemblyName GetName();
    public virtual Type GetType(string name);
    public virtual Type GetType(string name, bool throwOnError, bool ignoreCase);
    public static Assembly Load(AssemblyName assemblyRef);
    public override string ToString();
  }
  public enum AssemblyContentType {
    Default = 0,
    WindowsRuntime = 1,
  }
  public sealed class AssemblyName {
    public AssemblyName();
    public AssemblyName(string assemblyName);
    public AssemblyContentType ContentType { get; set; }
    public string CultureName { get; set; }
    public AssemblyNameFlags Flags { get; set; }
    public string FullName { get; }
    public string Name { get; set; }
    public ProcessorArchitecture ProcessorArchitecture { get; set; }
    public Version Version { get; set; }
    public byte[] GetPublicKey();
    public byte[] GetPublicKeyToken();
    public void SetPublicKey(byte[] publicKey);
    public void SetPublicKeyToken(byte[] publicKeyToken);
    public override string ToString();
  }
+ public abstract class Binder {
+   protected Binder();
  }
+ public enum BindingFlags {
+   CreateInstance = 512,
+   DeclaredOnly = 2,
+   Default = 0,
+   FlattenHierarchy = 64,
+   GetField = 1024,
+   GetProperty = 4096,
+   IgnoreCase = 1,
+   Instance = 4,
+   InvokeMethod = 256,
+   NonPublic = 32,
+   Public = 16,
+   SetField = 2048,
+   SetProperty = 8192,
+   Static = 8,
  }
  public abstract class ConstructorInfo : MethodBase {
    public static readonly string ConstructorName;
    public static readonly string TypeConstructorName;
    public abstract MethodAttributes Attributes { get; }
    public virtual CallingConventions CallingConvention { get; }
    public virtual bool ContainsGenericParameters { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public bool IsAbstract { get; }
    public bool IsAssembly { get; }
    public bool IsConstructor { get; }
    public bool IsFamily { get; }
    public bool IsFamilyAndAssembly { get; }
    public bool IsFamilyOrAssembly { get; }
    public bool IsFinal { get; }
    public virtual bool IsGenericMethod { get; }
    public virtual bool IsGenericMethodDefinition { get; }
    public bool IsHideBySig { get; }
    public bool IsPrivate { get; }
    public bool IsPublic { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
    public bool IsVirtual { get; }
+   public override MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
+   public virtual RuntimeMethodHandle MethodHandle { get; }
    public abstract MethodImplAttributes MethodImplementationFlags { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
+   public virtual Type ReflectedType { get; }
    public override bool Equals(object obj);
+   public static MethodBase GetCurrentMethod();
    public virtual Type[] GetGenericArguments();
    public override int GetHashCode();
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle);
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle, RuntimeTypeHandle declaringType);
+   public virtual MethodImplAttributes GetMethodImplementationFlags();
    public abstract ParameterInfo[] GetParameters();
    public virtual object Invoke(object obj, object[] parameters);
    public virtual object Invoke(object[] parameters);
  }
  public class CustomAttributeData {
    public virtual Type AttributeType { get; }
+   public virtual ConstructorInfo Constructor { get; }
    public virtual IList<CustomAttributeTypedArgument> ConstructorArguments { get; }
    public virtual IList<CustomAttributeNamedArgument> NamedArguments { get; }
+   public static IList<CustomAttributeData> GetCustomAttributes(Assembly target);
+   public static IList<CustomAttributeData> GetCustomAttributes(MemberInfo target);
+   public static IList<CustomAttributeData> GetCustomAttributes(Module target);
+   public static IList<CustomAttributeData> GetCustomAttributes(ParameterInfo target);
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomAttributeNamedArgument {
    public bool IsField { get; }
+   public MemberInfo MemberInfo { get; }
    public string MemberName { get; }
    public CustomAttributeTypedArgument TypedValue { get; }
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomAttributeTypedArgument {
    public Type ArgumentType { get; }
    public object Value { get; }
  }
  public abstract class EventInfo : MemberInfo {
    public virtual MethodInfo AddMethod { get; }
    public abstract EventAttributes Attributes { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public virtual Type EventHandlerType { get; }
+   public virtual bool IsMulticast { get; }
    public bool IsSpecialName { get; }
+   public override MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
    public virtual MethodInfo RaiseMethod { get; }
+   public virtual Type ReflectedType { get; }
    public virtual MethodInfo RemoveMethod { get; }
    public virtual void AddEventHandler(object target, Delegate handler);
    public override bool Equals(object obj);
+   public MethodInfo GetAddMethod();
+   public virtual MethodInfo GetAddMethod(bool nonPublic);
    public override int GetHashCode();
+   public MethodInfo GetRaiseMethod();
+   public virtual MethodInfo GetRaiseMethod(bool nonPublic);
+   public MethodInfo GetRemoveMethod();
+   public virtual MethodInfo GetRemoveMethod(bool nonPublic);
    public virtual void RemoveEventHandler(object target, Delegate handler);
  }
  public abstract class FieldInfo : MemberInfo {
    public abstract FieldAttributes Attributes { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
+   public virtual RuntimeFieldHandle FieldHandle { get; }
    public abstract Type FieldType { get; }
    public bool IsAssembly { get; }
    public bool IsFamily { get; }
    public bool IsFamilyAndAssembly { get; }
    public bool IsFamilyOrAssembly { get; }
    public bool IsInitOnly { get; }
    public bool IsLiteral { get; }
    public bool IsPrivate { get; }
    public bool IsPublic { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
+   public override MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
+   public virtual Type ReflectedType { get; }
    public override bool Equals(object obj);
    public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle);
    public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle, RuntimeTypeHandle declaringType);
    public override int GetHashCode();
    public abstract object GetValue(object obj);
    public virtual void SetValue(object obj, object value);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct InterfaceMapping {
+   public MethodInfo[] InterfaceMethods;
+   public MethodInfo[] TargetMethods;
+   public Type InterfaceType;
+   public Type TargetType;
  }
  public static class IntrospectionExtensions {
    public static TypeInfo GetTypeInfo(this Type type);
  }
+ public class InvalidFilterCriteriaException : Exception {
+   public InvalidFilterCriteriaException();
+   public InvalidFilterCriteriaException(string message);
+   public InvalidFilterCriteriaException(string message, Exception inner);
  }
  public interface IReflectableType {
    TypeInfo GetTypeInfo();
  }
  public class LocalVariableInfo {
    protected LocalVariableInfo();
    public virtual bool IsPinned { get; }
    public virtual int LocalIndex { get; }
    public virtual Type LocalType { get; }
    public override string ToString();
  }
  public class ManifestResourceInfo {
    public ManifestResourceInfo(Assembly containingAssembly, string containingFileName, ResourceLocation resourceLocation);
    public virtual string FileName { get; }
    public virtual Assembly ReferencedAssembly { get; }
    public virtual ResourceLocation ResourceLocation { get; }
  }
+ public delegate bool MemberFilter(MemberInfo m, object filterCriteria);
  public abstract class MemberInfo {
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
+   public virtual MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
+   public virtual Type ReflectedType { get; }
    public override bool Equals(object obj);
    public override int GetHashCode();
  }
+ public enum MemberTypes {
+   All = 191,
+   Constructor = 1,
+   Custom = 64,
+   Event = 2,
+   Field = 4,
+   Method = 8,
+   NestedType = 128,
+   Property = 16,
+   TypeInfo = 32,
  }
  public abstract class MethodBase : MemberInfo {
    public abstract MethodAttributes Attributes { get; }
    public virtual CallingConventions CallingConvention { get; }
    public virtual bool ContainsGenericParameters { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public bool IsAbstract { get; }
    public bool IsAssembly { get; }
    public bool IsConstructor { get; }
    public bool IsFamily { get; }
    public bool IsFamilyAndAssembly { get; }
    public bool IsFamilyOrAssembly { get; }
    public bool IsFinal { get; }
    public virtual bool IsGenericMethod { get; }
    public virtual bool IsGenericMethodDefinition { get; }
    public bool IsHideBySig { get; }
    public bool IsPrivate { get; }
    public bool IsPublic { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
    public bool IsVirtual { get; }
+   public virtual MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
+   public virtual RuntimeMethodHandle MethodHandle { get; }
    public abstract MethodImplAttributes MethodImplementationFlags { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
+   public virtual Type ReflectedType { get; }
    public override bool Equals(object obj);
+   public static MethodBase GetCurrentMethod();
    public virtual Type[] GetGenericArguments();
    public override int GetHashCode();
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle);
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle, RuntimeTypeHandle declaringType);
+   public virtual MethodImplAttributes GetMethodImplementationFlags();
    public abstract ParameterInfo[] GetParameters();
    public virtual object Invoke(object obj, object[] parameters);
  }
  public abstract class MethodInfo : MethodBase {
    public abstract MethodAttributes Attributes { get; }
    public virtual CallingConventions CallingConvention { get; }
    public virtual bool ContainsGenericParameters { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public bool IsAbstract { get; }
    public bool IsAssembly { get; }
    public bool IsConstructor { get; }
    public bool IsFamily { get; }
    public bool IsFamilyAndAssembly { get; }
    public bool IsFamilyOrAssembly { get; }
    public bool IsFinal { get; }
    public virtual bool IsGenericMethod { get; }
    public virtual bool IsGenericMethodDefinition { get; }
    public bool IsHideBySig { get; }
    public bool IsPrivate { get; }
    public bool IsPublic { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
    public bool IsVirtual { get; }
+   public override MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
+   public virtual RuntimeMethodHandle MethodHandle { get; }
    public abstract MethodImplAttributes MethodImplementationFlags { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
+   public virtual Type ReflectedType { get; }
    public virtual ParameterInfo ReturnParameter { get; }
    public virtual Type ReturnType { get; }
    public virtual Delegate CreateDelegate(Type delegateType);
    public virtual Delegate CreateDelegate(Type delegateType, object target);
    public override bool Equals(object obj);
+   public virtual MethodInfo GetBaseDefinition();
+   public static MethodBase GetCurrentMethod();
    public override Type[] GetGenericArguments();
    public virtual MethodInfo GetGenericMethodDefinition();
    public override int GetHashCode();
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle);
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle, RuntimeTypeHandle declaringType);
+   public virtual MethodImplAttributes GetMethodImplementationFlags();
    public abstract ParameterInfo[] GetParameters();
    public virtual object Invoke(object obj, object[] parameters);
    public virtual MethodInfo MakeGenericMethod(params Type[] typeArguments);
  }
  public abstract class Module {
+   public static readonly TypeFilter FilterTypeName;
+   public static readonly TypeFilter FilterTypeNameIgnoreCase;
    public virtual Assembly Assembly { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtual string FullyQualifiedName { get; }
+   public virtual int MDStreamVersion { get; }
+   public virtual int MetadataToken { get; }
+   public virtual Guid ModuleVersionId { get; }
    public virtual string Name { get; }
+   public virtual string ScopeName { get; }
    public override bool Equals(object o);
+   public virtual Type[] FindTypes(TypeFilter filter, object filterCriteria);
+   public FieldInfo GetField(string name);
+   public virtual FieldInfo GetField(string name, BindingFlags bindingAttr);
+   public FieldInfo[] GetFields();
+   public virtual FieldInfo[] GetFields(BindingFlags bindingFlags);
    public override int GetHashCode();
+   public MethodInfo GetMethod(string name);
+   public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
+   public MethodInfo GetMethod(string name, Type[] types);
+   public MethodInfo[] GetMethods();
+   public virtual MethodInfo[] GetMethods(BindingFlags bindingFlags);
+   public virtual Type GetType(string className);
+   public virtual Type GetType(string className, bool ignoreCase);
    public virtual Type GetType(string className, bool throwOnError, bool ignoreCase);
+   public virtual Type[] GetTypes();
+   public FieldInfo ResolveField(int metadataToken);
+   public virtual FieldInfo ResolveField(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
+   public MemberInfo ResolveMember(int metadataToken);
+   public virtual MemberInfo ResolveMember(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
+   public MethodBase ResolveMethod(int metadataToken);
+   public virtual MethodBase ResolveMethod(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
+   public virtual byte[] ResolveSignature(int metadataToken);
+   public virtual string ResolveString(int metadataToken);
+   public Type ResolveType(int metadataToken);
+   public virtual Type ResolveType(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
    public override string ToString();
  }
  public class ParameterInfo {
    public virtual ParameterAttributes Attributes { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtual object DefaultValue { get; }
    public virtual bool HasDefaultValue { get; }
    public bool IsIn { get; }
    public bool IsOptional { get; }
    public bool IsOut { get; }
    public bool IsRetval { get; }
    public virtual MemberInfo Member { get; }
+   public virtual int MetadataToken { get; }
    public virtual string Name { get; }
    public virtual Type ParameterType { get; }
    public virtual int Position { get; }
+   public virtual object RawDefaultValue { get; }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ParameterModifier {
+   public ParameterModifier(int parameterCount);
+   public bool this[int index] { get; set; }
  }
  public abstract class PropertyInfo : MemberInfo {
    public abstract PropertyAttributes Attributes { get; }
    public abstract bool CanRead { get; }
    public abstract bool CanWrite { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public virtual MethodInfo GetMethod { get; }
    public bool IsSpecialName { get; }
+   public override MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
    public abstract Type PropertyType { get; }
+   public virtual Type ReflectedType { get; }
    public virtual MethodInfo SetMethod { get; }
    public override bool Equals(object obj);
+   public MethodInfo[] GetAccessors();
+   public virtual MethodInfo[] GetAccessors(bool nonPublic);
    public virtual object GetConstantValue();
+   public MethodInfo GetGetMethod();
+   public virtual MethodInfo GetGetMethod(bool nonPublic);
    public override int GetHashCode();
    public abstract ParameterInfo[] GetIndexParameters();
+   public MethodInfo GetSetMethod();
+   public virtual MethodInfo GetSetMethod(bool nonPublic);
    public object GetValue(object obj);
    public virtual object GetValue(object obj, object[] index);
    public void SetValue(object obj, object value);
    public virtual void SetValue(object obj, object value, object[] index);
  }
  public abstract class ReflectionContext {
    protected ReflectionContext();
    public virtual TypeInfo GetTypeForObject(object value);
    public abstract Assembly MapAssembly(Assembly assembly);
    public abstract TypeInfo MapType(TypeInfo type);
  }
  public sealed class ReflectionTypeLoadException : Exception {
    public ReflectionTypeLoadException(Type[] classes, Exception[] exceptions);
    public ReflectionTypeLoadException(Type[] classes, Exception[] exceptions, string message);
    public Exception[] LoaderExceptions { get; }
    public Type[] Types { get; }
  }
  public enum ResourceLocation {
    ContainedInAnotherAssembly = 2,
    ContainedInManifestFile = 4,
    Embedded = 1,
  }
+ public class TargetException : Exception {
+   public TargetException();
+   public TargetException(string message);
+   public TargetException(string message, Exception inner);
  }
  public sealed class TargetInvocationException : Exception {
    public TargetInvocationException(Exception inner);
    public TargetInvocationException(string message, Exception inner);
  }
  public sealed class TargetParameterCountException : Exception {
    public TargetParameterCountException();
    public TargetParameterCountException(string message);
    public TargetParameterCountException(string message, Exception inner);
  }
+ public delegate bool TypeFilter(Type m, object filterCriteria);
  public abstract class TypeInfo : MemberInfo, IReflectableType {
    public abstract Assembly Assembly { get; }
    public abstract string AssemblyQualifiedName { get; }
    public abstract TypeAttributes Attributes { get; }
    public abstract Type BaseType { get; }
    public abstract bool ContainsGenericParameters { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtual IEnumerable<ConstructorInfo> DeclaredConstructors { get; }
    public virtual IEnumerable<EventInfo> DeclaredEvents { get; }
    public virtual IEnumerable<FieldInfo> DeclaredFields { get; }
    public virtual IEnumerable<MemberInfo> DeclaredMembers { get; }
    public virtual IEnumerable<MethodInfo> DeclaredMethods { get; }
    public virtual IEnumerable<TypeInfo> DeclaredNestedTypes { get; }
    public virtual IEnumerable<PropertyInfo> DeclaredProperties { get; }
    public abstract MethodBase DeclaringMethod { get; }
    public abstract Type DeclaringType { get; }
    public abstract string FullName { get; }
    public abstract GenericParameterAttributes GenericParameterAttributes { get; }
    public abstract int GenericParameterPosition { get; }
    public abstract Type[] GenericTypeArguments { get; }
    public virtual Type[] GenericTypeParameters { get; }
    public abstract Guid GUID { get; }
    public bool HasElementType { get; }
    public virtual IEnumerable<Type> ImplementedInterfaces { get; }
    public bool IsAbstract { get; }
    public bool IsAnsiClass { get; }
    public bool IsArray { get; }
    public bool IsAutoClass { get; }
    public bool IsAutoLayout { get; }
    public bool IsByRef { get; }
    public bool IsClass { get; }
    public virtual bool IsCOMObject { get; }
    public abstract bool IsEnum { get; }
    public bool IsExplicitLayout { get; }
    public abstract bool IsGenericParameter { get; }
    public abstract bool IsGenericType { get; }
    public abstract bool IsGenericTypeDefinition { get; }
    public bool IsImport { get; }
    public bool IsInterface { get; }
    public bool IsLayoutSequential { get; }
    public bool IsMarshalByRef { get; }
    public bool IsNested { get; }
    public bool IsNestedAssembly { get; }
    public bool IsNestedFamANDAssem { get; }
    public bool IsNestedFamily { get; }
    public bool IsNestedFamORAssem { get; }
    public bool IsNestedPrivate { get; }
    public bool IsNestedPublic { get; }
    public bool IsNotPublic { get; }
    public bool IsPointer { get; }
    public virtual bool IsPrimitive { get; }
    public bool IsPublic { get; }
    public bool IsSealed { get; }
    public abstract bool IsSerializable { get; }
    public bool IsSpecialName { get; }
    public bool IsUnicodeClass { get; }
    public virtual bool IsValueType { get; }
    public bool IsVisible { get; }
+   public virtual MemberTypes MemberType { get; }
+   public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
    public abstract string Namespace { get; }
+   public virtual Type ReflectedType { get; }
+   public virtual StructLayoutAttribute StructLayoutAttribute { get; }
+   public ConstructorInfo TypeInitializer { get; }
+   public abstract Type UnderlyingSystemType { get; }
    public virtual Type AsType();
    public override bool Equals(object obj);
+   public virtual Type[] FindInterfaces(TypeFilter filter, object filterCriteria);
+   public virtual MemberInfo[] FindMembers(MemberTypes memberType, BindingFlags bindingAttr, MemberFilter filter, object filterCriteria);
    public abstract int GetArrayRank();
+   public ConstructorInfo GetConstructor(Type[] types);
+   public ConstructorInfo[] GetConstructors();
+   public abstract ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
    public virtual EventInfo GetDeclaredEvent(string name);
    public virtual FieldInfo GetDeclaredField(string name);
    public virtual MethodInfo GetDeclaredMethod(string name);
    public virtual IEnumerable<MethodInfo> GetDeclaredMethods(string name);
    public virtual TypeInfo GetDeclaredNestedType(string name);
    public virtual PropertyInfo GetDeclaredProperty(string name);
+   public virtual MemberInfo[] GetDefaultMembers();
    public abstract Type GetElementType();
+   public virtual string GetEnumName(object value);
+   public virtual string[] GetEnumNames();
+   public virtual Type GetEnumUnderlyingType();
+   public virtual Array GetEnumValues();
+   public EventInfo GetEvent(string name);
+   public abstract EventInfo GetEvent(string name, BindingFlags bindingAttr);
+   public virtual EventInfo[] GetEvents();
+   public abstract EventInfo[] GetEvents(BindingFlags bindingAttr);
+   public FieldInfo GetField(string name);
+   public abstract FieldInfo GetField(string name, BindingFlags bindingAttr);
+   public FieldInfo[] GetFields();
+   public abstract FieldInfo[] GetFields(BindingFlags bindingAttr);
+   public virtual Type[] GetGenericArguments();
    public abstract Type[] GetGenericParameterConstraints();
    public abstract Type GetGenericTypeDefinition();
    public override int GetHashCode();
+   public Type GetInterface(string name);
+   public abstract Type GetInterface(string name, bool ignoreCase);
+   public virtual InterfaceMapping GetInterfaceMap(Type interfaceType);
+   public abstract Type[] GetInterfaces();
+   public MemberInfo[] GetMember(string name);
+   public virtual MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
+   public virtual MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
+   public MemberInfo[] GetMembers();
+   public abstract MemberInfo[] GetMembers(BindingFlags bindingAttr);
+   public MethodInfo GetMethod(string name);
+   public MethodInfo GetMethod(string name, BindingFlags bindingAttr);
+   public MethodInfo GetMethod(string name, Type[] types);
+   public MethodInfo GetMethod(string name, Type[] types, ParameterModifier[] modifiers);
+   public MethodInfo[] GetMethods();
+   public abstract MethodInfo[] GetMethods(BindingFlags bindingAttr);
+   public Type GetNestedType(string name);
+   public abstract Type GetNestedType(string name, BindingFlags bindingAttr);
+   public Type[] GetNestedTypes();
+   public abstract Type[] GetNestedTypes(BindingFlags bindingAttr);
+   public PropertyInfo[] GetProperties();
+   public abstract PropertyInfo[] GetProperties(BindingFlags bindingAttr);
+   public PropertyInfo GetProperty(string name);
+   public PropertyInfo GetProperty(string name, BindingFlags bindingAttr);
+   public PropertyInfo GetProperty(string name, Type returnType);
+   public PropertyInfo GetProperty(string name, Type returnType, Type[] types);
+   public PropertyInfo GetProperty(string name, Type returnType, Type[] types, ParameterModifier[] modifiers);
+   public PropertyInfo GetProperty(string name, Type[] types);
    public virtual bool IsAssignableFrom(TypeInfo typeInfo);
+   public virtual bool IsAssignableFrom(Type c);
+   public abstract bool IsDefined(Type attributeType, bool inherit);
+   public virtual bool IsEnumDefined(object value);
    public virtual bool IsEquivalentTo(Type other);
+   public virtual bool IsInstanceOfType(object o);
    public virtual bool IsSubclassOf(Type c);
    public abstract Type MakeArrayType();
    public abstract Type MakeArrayType(int rank);
    public abstract Type MakeByRefType();
    public abstract Type MakeGenericType(params Type[] typeArguments);
    public abstract Type MakePointerType();
    TypeInfo System.Reflection.IReflectableType.GetTypeInfo();
  }
 }
```

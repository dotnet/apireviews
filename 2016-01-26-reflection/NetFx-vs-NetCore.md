```diff
---.NET Framework
+++.NET Core
namespace System {
  public abstract class Type : MemberInfo, _Type, IReflect {
    public static readonly char Delimiter;
    ^ Pallavi Taneja: Added on the Type.
    public static readonly object Missing;
    ^ Pallavi Taneja: Already present.
-   public static readonly MemberFilter FilterAttribute;
    ^ Pallavi Taneja: Filter* can't be added to TypeInfo in this contract revision.
-   public static readonly MemberFilter FilterName;
-   public static readonly MemberFilter FilterNameIgnoreCase;
    public static readonly Type[] EmptyTypes;
    ^ Pallavi Taneja: Already present.
-   protected Type();
-   public abstract Assembly Assembly { get; }
    public abstract string AssemblyQualifiedName { get; }
-   public TypeAttributes Attributes { get; }
-   public abstract Type BaseType { get; }
-   public virtual bool ContainsGenericParameters { get; }
-   public virtual MethodBase DeclaringMethod { get; }
    public overrideabstract Type DeclaringType { get; }
-   public static Binder DefaultBinder { get; }
    ^ Pallavi Taneja: [Pending discussion] Whether Binder should be empty or exposed via a separate contract.
    public abstract string FullName { get; }
-   public virtual GenericParameterAttributes GenericParameterAttributes { get; }
    public virtualabstract int GenericParameterPosition { get; }
    public virtualabstract Type[] GenericTypeArguments { get; }
-   public abstract Guid GUID { get; }
    public bool HasElementType { get; }
-   public bool IsAbstract { get; }
-   public bool IsAnsiClass { get; }
    public virtual bool IsArray { get; }
-   public bool IsAutoClass { get; }
-   public bool IsAutoLayout { get; }
    public virtual bool IsByRef { get; }
-   public bool IsClass { get; }
-   public bool IsCOMObject { get; }
    public virtualabstract bool IsConstructedGenericType { get; }
    ^ Pallavi Taneja: Already exposed in Type.
-   public bool IsContextful { get; }
    ^ Pallavi Taneja: Used only in remoting.
-   public virtual bool IsEnum { get; }
-   public bool IsExplicitLayout { get; }
    public virtualabstract bool IsGenericParameter { get; }
-   public virtual bool IsGenericType { get; }
-   public virtual bool IsGenericTypeDefinition { get; }
-   public bool IsImport { get; }
-   public bool IsInterface { get; }
-   public bool IsLayoutSequential { get; }
-   public bool IsMarshalByRef { get; }
    public bool IsNested { get; }
-   public bool IsNestedAssembly { get; }
-   public bool IsNestedFamANDAssem { get; }
-   public bool IsNestedFamily { get; }
-   public bool IsNestedFamORAssem { get; }
-   public bool IsNestedPrivate { get; }
-   public bool IsNestedPublic { get; }
-   public bool IsNotPublic { get; }
    public virtual bool IsPointer { get; }
-   public bool IsPrimitive { get; }
-   public bool IsPublic { get; }
-   public bool IsSealed { get; }
-   public virtual bool IsSecurityCritical { get; }
    ^ Pallavi Taneja: Obsolete concept.
-   public virtual bool IsSecuritySafeCritical { get; }
-   public virtual bool IsSecurityTransparent { get; }
-   public virtual bool IsSerializable { get; }
-   public bool IsSpecialName { get; }
-   public bool IsUnicodeClass { get; }
-   public bool IsValueType { get; }
-   public bool IsVisible { get; }
-   public override MemberTypes MemberType { get; }
-   public abstract new Module Module { get; }
+   public abstract string Name { get; }
    public abstract string Namespace { get; }
-   public override Type ReflectedType { get; }
-   public virtual StructLayoutAttribute StructLayoutAttribute { get; }
    public virtual RuntimeTypeHandle TypeHandle { get; }
    ^ Pallavi Taneja: Already exposed in Type
-   public ConstructorInfo TypeInitializer { get; }
-   public abstract Type UnderlyingSystemType { get; }
    public override bool Equals(object o);
    public virtual bool Equals(Type o);
-   public virtual Type[] FindInterfaces(TypeFilter filter, object filterCriteria);
-   public virtual MemberInfo[] FindMembers(MemberTypes memberType, BindingFlags bindingAttr, MemberFilter filter, object filterCriteria);
    public virtualabstract int GetArrayRank();
-   protected abstract TypeAttributes GetAttributeFlagsImpl();
-   public ConstructorInfo GetConstructor(BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
    ^ Pallavi Taneja: Binder decision pending.
-   public ConstructorInfo GetConstructor(BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
-   public ConstructorInfo GetConstructor(Type[] types);
-   protected abstract ConstructorInfo GetConstructorImpl(BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
-   public ConstructorInfo[] GetConstructors();
-   public abstract ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
-   public virtual MemberInfo[] GetDefaultMembers();
    public abstract Type GetElementType();
-   public virtual string GetEnumName(object value);
-   public virtual string[] GetEnumNames();
-   public virtual Type GetEnumUnderlyingType();
-   public virtual Array GetEnumValues();
-   public EventInfo GetEvent(string name);
-   public abstract EventInfo GetEvent(string name, BindingFlags bindingAttr);
-   public virtual EventInfo[] GetEvents();
-   public abstract EventInfo[] GetEvents(BindingFlags bindingAttr);
-   public FieldInfo GetField(string name);
-   public abstract FieldInfo GetField(string name, BindingFlags bindingAttr);
-   public FieldInfo[] GetFields();
-   public abstract FieldInfo[] GetFields(BindingFlags bindingAttr);
-   public virtual Type[] GetGenericArguments();
-   public virtual Type[] GetGenericParameterConstraints();
    public virtualabstract Type GetGenericTypeDefinition();
    public override int GetHashCode();
-   public Type GetInterface(string name);
-   public abstract Type GetInterface(string name, bool ignoreCase);
-   public virtual InterfaceMapping GetInterfaceMap(Type interfaceType);
-   public abstract Type[] GetInterfaces();
-   public MemberInfo[] GetMember(string name);
-   public virtual MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
-   public virtual MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
-   public MemberInfo[] GetMembers();
-   public abstract MemberInfo[] GetMembers(BindingFlags bindingAttr);
-   public MethodInfo GetMethod(string name);
-   public MethodInfo GetMethod(string name, BindingFlags bindingAttr);
-   public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
    ^ Pallavi Taneja: Binder decision pending.
-   public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
-   public MethodInfo GetMethod(string name, Type[] types);
-   public MethodInfo GetMethod(string name, Type[] types, ParameterModifier[] modifiers);
-   protected abstract MethodInfo GetMethodImpl(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
-   public MethodInfo[] GetMethods();
-   public abstract MethodInfo[] GetMethods(BindingFlags bindingAttr);
-   public Type GetNestedType(string name);
-   public abstract Type GetNestedType(string name, BindingFlags bindingAttr);
-   public Type[] GetNestedTypes();
-   public abstract Type[] GetNestedTypes(BindingFlags bindingAttr);
-   public PropertyInfo[] GetProperties();
-   public abstract PropertyInfo[] GetProperties(BindingFlags bindingAttr);
-   public PropertyInfo GetProperty(string name);
-   public PropertyInfo GetProperty(string name, BindingFlags bindingAttr);
-   public PropertyInfo GetProperty(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
    ^ Pallavi Taneja: Binder decision pending.
-   public PropertyInfo GetProperty(string name, Type returnType);
-   public PropertyInfo GetProperty(string name, Type returnType, Type[] types);
-   public PropertyInfo GetProperty(string name, Type returnType, Type[] types, ParameterModifier[] modifiers);
-   public PropertyInfo GetProperty(string name, Type[] types);
-   protected abstract PropertyInfo GetPropertyImpl(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
-   public new Type GetType();
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName);
    ^ Pallavi Taneja: GetType* can't be added to TypeInfo in the current contract version.
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName, bool throwOnError);
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName, bool throwOnError, bool ignoreCase);
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver);
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver, bool throwOnError);
-   [MethodImpl(NoInlining)]public static Type GetType(string typeName, Func<AssemblyName, Assembly> assemblyResolver, Func<Assembly, string, bool, Type> typeResolver, bool throwOnError, bool ignoreCase);
-   public static Type[] GetTypeArray(object[] args);
    ^ Pallavi Taneja: Can't be added to TypeInfo in the current contract version.
    public static TypeCode GetTypeCode(Type type);
-   protected virtual TypeCode GetTypeCodeImpl();
-   public static Type GetTypeFromCLSID(Guid clsid);
    ^ Pallavi Taneja: Interop concept
-   public static Type GetTypeFromCLSID(Guid clsid, bool throwOnError);
-   public static Type GetTypeFromCLSID(Guid clsid, string server);
-   public static Type GetTypeFromCLSID(Guid clsid, string server, bool throwOnError);
-   [MethodImpl(InternalCall)]public static Type GetTypeFromHandle(RuntimeTypeHandle handle);
    ^ Pallavi Taneja: Already implemented.
-   public static Type GetTypeFromProgID(string progID);
    ^ Pallavi Taneja: Interop concept
-   public static Type GetTypeFromProgID(string progID, bool throwOnError);
-   public static Type GetTypeFromProgID(string progID, string server);
-   public static Type GetTypeFromProgID(string progID, string server, bool throwOnError);
-   public static RuntimeTypeHandle GetTypeHandle(object o);
    ^ Pallavi Taneja: Decision pending.
-   protected abstract bool HasElementTypeImpl();
-   public object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args);
    ^ Pallavi Taneja: Binder decision pending.
-   public object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, CultureInfo culture);
-   public abstract object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, ParameterModifier[] modifiers, CultureInfo culture, string[] namedParameters);
-   protected abstract bool IsArrayImpl();
-   public virtual bool IsAssignableFrom(Type c);
-   protected abstract bool IsByRefImpl();
-   protected abstract bool IsCOMObjectImpl();
-   protected virtual bool IsContextfulImpl();
-   public virtual bool IsEnumDefined(object value);
-   public virtual bool IsEquivalentTo(Type other);
-   public virtual bool IsInstanceOfType(object o);
-   protected virtual bool IsMarshalByRefImpl();
-   protected abstract bool IsPointerImpl();
-   protected abstract bool IsPrimitiveImpl();
-   public virtual bool IsSubclassOf(Type c);
-   protected virtual bool IsValueTypeImpl();
    public virtualabstract Type MakeArrayType();
    public virtualabstract Type MakeArrayType(int rank);
    public virtualabstract Type MakeByRefType();
    public virtualabstract Type MakeGenericType(params Type[] typeArguments);
    public virtualabstract Type MakePointerType();
-   [MethodImpl(InternalCall)]public static bool operator ==(Type left, Type right);
-   [MethodImpl(InternalCall)]public static bool operator !=(Type left, Type right);
-   [MethodImpl(NoInlining)]public static Type ReflectionOnlyGetType(string typeName, bool throwIfNotFound, bool ignoreCase);
-   void System.Runtime.InteropServices._Type.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   void System.Runtime.InteropServices._Type.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._Type.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._Type.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
    public override string ToString();
  }
 namespace System.Reflection {
  public sealed class AmbiguousMatchException : SystemExceptionException {
    public AmbiguousMatchException();
    public AmbiguousMatchException(string message);
    public AmbiguousMatchException(string message, Exception inner);
  }
  public abstract class Assembly : _Assembly, ICustomAttributeProvider, IEvidenceFactory, ISerializable {
-   protected Assembly();
    public virtual string CodeBase { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtualabstract IEnumerable<TypeInfo> DefinedTypes { get; }
    public virtual MethodInfo EntryPoint { get; }
    public virtual string EscapedCodeBase { get; }
-   public virtual Evidence Evidence { get; }
    public virtual IEnumerable<Type> ExportedTypes { get; }
    public virtual string FullName { get; }
-   public virtual bool GlobalAssemblyCache { get; }
-   public virtual long HostContext { get; }
-   public virtual string ImageRuntimeVersion { get; }
    public virtual bool IsDynamic { get; }
-   public bool IsFullyTrusted { get; }
    public virtual string Location { get; }
    public virtual Module ManifestModule { get; }
    public virtualabstract IEnumerable<Module> Modules { get; }
-   public virtual PermissionSet PermissionSet { get; }
-   public virtual bool ReflectionOnly { get; }
-   public virtual SecurityRuleSet SecurityRuleSet { get; }
-   public virtual event ModuleResolveEventHandler ModuleResolve;
-   public object CreateInstance(string typeName);
-   public object CreateInstance(string typeName, bool ignoreCase);
-   public virtual object CreateInstance(string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder binder, object[] args, CultureInfo culture, object[] activationAttributes);
-   public static string CreateQualifiedName(string assemblyName, string typeName);
    public override bool Equals(object o);
-   public static Assembly GetAssembly(Type type);
-   [MethodImpl(NoInlining)]public static Assembly GetCallingAssembly();
-   public virtual object[] GetCustomAttributes(bool inherit);
-   public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
-   public virtual IList<CustomAttributeData> GetCustomAttributesData();
    public static Assembly GetEntryAssembly();
-   [MethodImpl(NoInlining)]public static Assembly GetExecutingAssembly();
-   public virtual Type[] GetExportedTypes();
-   public virtual FileStream GetFile(string name);
-   public virtual FileStream[] GetFiles();
-   public virtual FileStream[] GetFiles(bool getResourceModules);
    public override int GetHashCode();
-   public Module[] GetLoadedModules();
-   public virtual Module[] GetLoadedModules(bool getResourceModules);
    public virtual ManifestResourceInfo GetManifestResourceInfo(string resourceName);
    public virtual string[] GetManifestResourceNames();
    public virtual Stream GetManifestResourceStream(string name);
-   public virtual Stream GetManifestResourceStream(Type type, string name);
-   public virtual Module GetModule(string name);
-   public Module[] GetModules();
-   public virtual Module[] GetModules(bool getResourceModules);
    public virtual AssemblyName GetName();
-   public virtual AssemblyName GetName(bool copiedName);
-   public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
-   public virtual AssemblyName[] GetReferencedAssemblies();
-   public virtual Assembly GetSatelliteAssembly(CultureInfo culture);
-   public virtual Assembly GetSatelliteAssembly(CultureInfo culture, Version version);
    public virtual Type GetType(string name);
-   public virtual Type GetType(string name, bool throwOnError);
    public virtual Type GetType(string name, bool throwOnError, bool ignoreCase);
-   public virtual Type[] GetTypes();
-   public virtual bool IsDefined(Type attributeType, bool inherit);
-   [MethodImpl(NoInlining)]public static Assembly Load(byte[] rawAssembly);
-   [MethodImpl(NoInlining)]public static Assembly Load(byte[] rawAssembly, byte[] rawSymbolStore);
-   [MethodImpl(NoInlining)]public static Assembly Load(byte[] rawAssembly, byte[] rawSymbolStore, Evidence securityEvidence);
-   [MethodImpl(NoInlining)]public static Assembly Load(byte[] rawAssembly, byte[] rawSymbolStore, SecurityContextSource securityContextSource);
-   [MethodImpl(NoInlining)]public static Assembly Load(AssemblyName assemblyRef);
-   [MethodImpl(NoInlining)]public static Assembly Load(AssemblyName assemblyRef, Evidence assemblySecurity);
-   [MethodImpl(NoInlining)]public static Assembly Load(string assemblyString);
-   [MethodImpl(NoInlining)]public static Assembly Load(string assemblyString, Evidence assemblySecurity);
-   public static Assembly LoadFile(string path);
-   public static Assembly LoadFile(string path, Evidence securityEvidence);
-   [MethodImpl(NoInlining)]public static Assembly LoadFrom(string assemblyFile);
-   [MethodImpl(NoInlining)]public static Assembly LoadFrom(string assemblyFile, byte[] hashValue, AssemblyHashAlgorithm hashAlgorithm);
-   [MethodImpl(NoInlining)]public static Assembly LoadFrom(string assemblyFile, Evidence securityEvidence);
-   [MethodImpl(NoInlining)]public static Assembly LoadFrom(string assemblyFile, Evidence securityEvidence, byte[] hashValue, AssemblyHashAlgorithm hashAlgorithm);
-   public Module LoadModule(string moduleName, byte[] rawModule);
-   public virtual Module LoadModule(string moduleName, byte[] rawModule, byte[] rawSymbolStore);
-   [MethodImpl(NoInlining)]public static Assembly LoadWithPartialName(string partialName);
-   [MethodImpl(NoInlining)]public static Assembly LoadWithPartialName(string partialName, Evidence securityEvidence);
-   public static bool operator ==(Assembly left, Assembly right);
-   public static bool operator !=(Assembly left, Assembly right);
-   [MethodImpl(NoInlining)]public static Assembly ReflectionOnlyLoad(byte[] rawAssembly);
-   [MethodImpl(NoInlining)]public static Assembly ReflectionOnlyLoad(string assemblyString);
-   [MethodImpl(NoInlining)]public static Assembly ReflectionOnlyLoadFrom(string assemblyFile);
-   Type System.Runtime.InteropServices._Assembly.GetType();
    public override string ToString();
-   [MethodImpl(NoInlining)]public static Assembly UnsafeLoadFrom(string assemblyFile);
  }
- public sealed class AssemblyAlgorithmIdAttribute : Attribute {
-   public AssemblyAlgorithmIdAttribute(AssemblyHashAlgorithm algorithmId);
-   public AssemblyAlgorithmIdAttribute(uint algorithmId);
-   public uint AlgorithmId { get; }
  }
  public sealed class AssemblyCompanyAttribute : Attribute {
    public AssemblyCompanyAttribute(string company);
    public string Company { get; }
  }
  public sealed class AssemblyConfigurationAttribute : Attribute {
    public AssemblyConfigurationAttribute(string configuration);
    public string Configuration { get; }
  }
  public enum AssemblyContentType {
    Default = 0,
    WindowsRuntime = 1,
  }
  public sealed class AssemblyCopyrightAttribute : Attribute {
    public AssemblyCopyrightAttribute(string copyright);
    public string Copyright { get; }
  }
  public sealed class AssemblyCultureAttribute : Attribute {
    public AssemblyCultureAttribute(string culture);
    public string Culture { get; }
  }
  public sealed class AssemblyDefaultAliasAttribute : Attribute {
    public AssemblyDefaultAliasAttribute(string defaultAlias);
    public string DefaultAlias { get; }
  }
  public sealed class AssemblyDelaySignAttribute : Attribute {
    public AssemblyDelaySignAttribute(bool delaySign);
    public bool DelaySign { get; }
  }
  public sealed class AssemblyDescriptionAttribute : Attribute {
    public AssemblyDescriptionAttribute(string description);
    public string Description { get; }
  }
  public sealed class AssemblyFileVersionAttribute : Attribute {
    public AssemblyFileVersionAttribute(string version);
    public string Version { get; }
  }
  public sealed class AssemblyFlagsAttribute : Attribute {
-   public AssemblyFlagsAttribute(int assemblyFlags);
    public AssemblyFlagsAttribute(AssemblyNameFlags assemblyFlags);
-   public AssemblyFlagsAttribute(uint flags);
    public int AssemblyFlags { get; }
-   public uint Flags { get; }
  }
  public sealed class AssemblyInformationalVersionAttribute : Attribute {
    public AssemblyInformationalVersionAttribute(string informationalVersion);
    public string InformationalVersion { get; }
  }
  public sealed class AssemblyKeyFileAttribute : Attribute {
    public AssemblyKeyFileAttribute(string keyFile);
    public string KeyFile { get; }
  }
  public sealed class AssemblyKeyNameAttribute : Attribute {
    public AssemblyKeyNameAttribute(string keyName);
    public string KeyName { get; }
  }
  public sealed class AssemblyMetadataAttribute : Attribute {
    public AssemblyMetadataAttribute(string key, string value);
    public string Key { get; }
    public string Value { get; }
  }
  public sealed class AssemblyName : _AssemblyName, ICloneable, IDeserializationCallback, ISerializable {
    public AssemblyName();
    public AssemblyName(string assemblyName);
-   public string CodeBase { get; set; }
    public AssemblyContentType ContentType { get; set; }
-   public CultureInfo CultureInfo { get; set; }
    public string CultureName { get; set; }
-   public string EscapedCodeBase { get; }
    public AssemblyNameFlags Flags { get; set; }
    public string FullName { get; }
-   public AssemblyHashAlgorithm HashAlgorithm { get; set; }
-   public StrongNameKeyPair KeyPair { get; set; }
    public string Name { get; set; }
    public ProcessorArchitecture ProcessorArchitecture { get; set; }
    public Version Version { get; set; }
-   public AssemblyVersionCompatibility VersionCompatibility { get; set; }
-   public object Clone();
-   public static AssemblyName GetAssemblyName(string assemblyFile);
-   public void GetObjectData(SerializationInfo info, StreamingContext context);
    public byte[] GetPublicKey();
    public byte[] GetPublicKeyToken();
-   public void OnDeserialization(object sender);
-   public static bool ReferenceMatchesDefinition(AssemblyName reference, AssemblyName definition);
    public void SetPublicKey(byte[] publicKey);
    public void SetPublicKeyToken(byte[] publicKeyToken);
-   void System.Runtime.InteropServices._AssemblyName.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   void System.Runtime.InteropServices._AssemblyName.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._AssemblyName.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._AssemblyName.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
    public override string ToString();
  }
  public enum AssemblyNameFlags {
-   EnableJITcompileOptimizer = 16384,
    ^ Immo Landwerth: Why are those not exposed?
-   EnableJITcompileTracking = 32768,
    ^ Immo Landwerth: Why are those not exposed?
    None = 0,
    PublicKey = 1,
    Retargetable = 256,
  }
- public class AssemblyNameProxy : MarshalByRefObject {
-   public AssemblyNameProxy();
-   public AssemblyName GetAssemblyName(string assemblyFile);
  }
  public sealed class AssemblyProductAttribute : Attribute {
    public AssemblyProductAttribute(string product);
    public string Product { get; }
  }
  public sealed class AssemblySignatureKeyAttribute : Attribute {
    public AssemblySignatureKeyAttribute(string publicKey, string countersignature);
    public string Countersignature { get; }
    public string PublicKey { get; }
  }
  public sealed class AssemblyTitleAttribute : Attribute {
    public AssemblyTitleAttribute(string title);
    public string Title { get; }
  }
  public sealed class AssemblyTrademarkAttribute : Attribute {
    public AssemblyTrademarkAttribute(string trademark);
    public string Trademark { get; }
  }
  public sealed class AssemblyVersionAttribute : Attribute {
    public AssemblyVersionAttribute(string version);
    public string Version { get; }
  }
  public abstract class Binder {
    ^ Pallavi Taneja: Expose as Empty in System.Reflection. Options added separately.
    protected Binder();
    ^ Immo Landwerth: If we don't expose any members, i.e. make it a dummy, the .ctor should be private/internal.
-   public abstract FieldInfo BindToField(BindingFlags bindingAttr, FieldInfo[] match, object value, CultureInfo culture);
-   public abstract MethodBase BindToMethod(BindingFlags bindingAttr, MethodBase[] match, ref object[] args, ParameterModifier[] modifiers, CultureInfo culture, string[] names, out object state);
-   public abstract object ChangeType(object value, Type type, CultureInfo culture);
-   public abstract void ReorderArgumentArray(ref object[] args, object state);
-   public abstract MethodBase SelectMethod(BindingFlags bindingAttr, MethodBase[] match, Type[] types, ParameterModifier[] modifiers);
-   public abstract PropertyInfo SelectProperty(BindingFlags bindingAttr, PropertyInfo[] match, Type returnType, Type[] indexes, ParameterModifier[] modifiers);
  }
  public enum BindingFlags {
    CreateInstance = 512,
    DeclaredOnly = 2,
    Default = 0,
-   ExactBinding = 65536,
    ^ Pallavi Taneja: Used only in case Binder is extensible.
    FlattenHierarchy = 64,
    GetField = 1024,
    GetProperty = 4096,
    IgnoreCase = 1,
-   IgnoreReturn = 16777216,
    ^ Pallavi Taneja: Only used in Interop.
    Instance = 4,
    InvokeMethod = 256,
    NonPublic = 32,
-   OptionalParamBinding = 262144,
    ^ Pallavi Taneja: [Unsure] used most only in varArgs scenario.
    Public = 16,
-   PutDispProperty = 16384,
    ^ Pallavi Taneja: Interop scenario.
-   PutRefDispProperty = 32768,
    SetField = 2048,
    SetProperty = 8192,
    Static = 8,
-   SuppressChangeType = 131072,
  }
  public enum CallingConventions {
    Any = 3,
    ExplicitThis = 64,
    HasThis = 32,
    Standard = 1,
    VarArgs = 2,
  }
  public abstract class ConstructorInfo : MethodBase, _ConstructorInfo {
    public static readonly string ConstructorName;
    public static readonly string TypeConstructorName;
-   protected ConstructorInfo();
    public override MemberTypes MemberType { get; }
    public override bool Equals(object obj);
    public override int GetHashCode();
    public virtual object Invoke(object[] parameters);
-   public abstract object Invoke(BindingFlags invokeAttr, Binder binder, object[] parameters, CultureInfo culture);
    ^ Pallavi Taneja: System.Globalization dependency. Usage < 2%. No reflection contract currently take glob dep not just S.R.dll
-   public static bool operator ==(ConstructorInfo left, ConstructorInfo right);
-   public static bool operator !=(ConstructorInfo left, ConstructorInfo right);
-   void System.Runtime.InteropServices._ConstructorInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._ConstructorInfo.GetType();
-   void System.Runtime.InteropServices._ConstructorInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._ConstructorInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._ConstructorInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
-   object System.Runtime.InteropServices._ConstructorInfo.Invoke_2(object obj, BindingFlags invokeAttr, Binder binder, object[] parameters, CultureInfo culture);
-   object System.Runtime.InteropServices._ConstructorInfo.Invoke_3(object obj, object[] parameters);
-   object System.Runtime.InteropServices._ConstructorInfo.Invoke_4(BindingFlags invokeAttr, Binder binder, object[] parameters, CultureInfo culture);
-   object System.Runtime.InteropServices._ConstructorInfo.Invoke_5(object[] parameters);
  }
  public class CustomAttributeData {
-   protected CustomAttributeData();
    public virtual Type AttributeType { get; }
    public virtual ConstructorInfo Constructor { get; }
    public virtual IList<CustomAttributeTypedArgument> ConstructorArguments { get; }
    public virtual IList<CustomAttributeNamedArgument> NamedArguments { get; }
-   public override bool Equals(object obj);
    public static IList<CustomAttributeData> GetCustomAttributes(Assembly target);
    public static IList<CustomAttributeData> GetCustomAttributes(MemberInfo target);
    public static IList<CustomAttributeData> GetCustomAttributes(Module target);
    public static IList<CustomAttributeData> GetCustomAttributes(ParameterInfo target);
-   public override int GetHashCode();
-   public override string ToString();
  }
  public static class CustomAttributeExtensions {
    public static Attribute GetCustomAttribute(this Assembly element, Type attributeType);
    public static Attribute GetCustomAttribute(this MemberInfo element, Type attributeType);
    public static Attribute GetCustomAttribute(this MemberInfo element, Type attributeType, bool inherit);
    public static Attribute GetCustomAttribute(this Module element, Type attributeType);
    public static Attribute GetCustomAttribute(this ParameterInfo element, Type attributeType);
    public static Attribute GetCustomAttribute(this ParameterInfo element, Type attributeType, bool inherit);
    public static T GetCustomAttribute<T>(this Assembly element) where T : Attribute;
    public static T GetCustomAttribute<T>(this MemberInfo element) where T : Attribute;
    public static T GetCustomAttribute<T>(this MemberInfo element, bool inherit) where T : Attribute;
    public static T GetCustomAttribute<T>(this Module element) where T : Attribute;
    public static T GetCustomAttribute<T>(this ParameterInfo element) where T : Attribute;
    public static T GetCustomAttribute<T>(this ParameterInfo element, bool inherit) where T : Attribute;
    public static IEnumerable<Attribute> GetCustomAttributes(this Assembly element);
    public static IEnumerable<Attribute> GetCustomAttributes(this Assembly element, Type attributeType);
    public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element);
    public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, bool inherit);
    public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, Type attributeType);
    public static IEnumerable<Attribute> GetCustomAttributes(this MemberInfo element, Type attributeType, bool inherit);
    public static IEnumerable<Attribute> GetCustomAttributes(this Module element);
    public static IEnumerable<Attribute> GetCustomAttributes(this Module element, Type attributeType);
    public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element);
    public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, bool inherit);
    public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, Type attributeType);
    public static IEnumerable<Attribute> GetCustomAttributes(this ParameterInfo element, Type attributeType, bool inherit);
    public static IEnumerable<T> GetCustomAttributes<T>(this Assembly element) where T : Attribute;
    public static IEnumerable<T> GetCustomAttributes<T>(this MemberInfo element) where T : Attribute;
    public static IEnumerable<T> GetCustomAttributes<T>(this MemberInfo element, bool inherit) where T : Attribute;
    public static IEnumerable<T> GetCustomAttributes<T>(this Module element) where T : Attribute;
    public static IEnumerable<T> GetCustomAttributes<T>(this ParameterInfo element) where T : Attribute;
    public static IEnumerable<T> GetCustomAttributes<T>(this ParameterInfo element, bool inherit) where T : Attribute;
    public static bool IsDefined(this Assembly element, Type attributeType);
    public static bool IsDefined(this MemberInfo element, Type attributeType);
    public static bool IsDefined(this MemberInfo element, Type attributeType, bool inherit);
    public static bool IsDefined(this Module element, Type attributeType);
    public static bool IsDefined(this ParameterInfo element, Type attributeType);
    public static bool IsDefined(this ParameterInfo element, Type attributeType, bool inherit);
  }
- public class CustomAttributeFormatException : FormatException {
    ^ Immo Landwerth: If we throw it from exposed APIs, then we should expose it.
    ^ Pallavi Taneja: [Not sure] Used mainly in case a tool formatted a custom attribute incorrectly.
-   public CustomAttributeFormatException();
-   protected CustomAttributeFormatException(SerializationInfo info, StreamingContext context);
-   public CustomAttributeFormatException(string message);
-   public CustomAttributeFormatException(string message, Exception inner);
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomAttributeNamedArgument {
    ^ Immo Landwerth: Expose Equals, GetHashCode, ==, and !=.
-   public CustomAttributeNamedArgument(MemberInfo memberInfo, object value);
-   public CustomAttributeNamedArgument(MemberInfo memberInfo, CustomAttributeTypedArgument typedArgument);
    public bool IsField { get; }
    public MemberInfo MemberInfo { get; }
    public string MemberName { get; }
    public CustomAttributeTypedArgument TypedValue { get; }
-   public override bool Equals(object obj);
-   public override int GetHashCode();
-   public static bool operator ==(CustomAttributeNamedArgument left, CustomAttributeNamedArgument right);
    ^ Pallavi Taneja: I have left out equality operators across reflection since, there were concerns earlier that its behavior might not be consistent with .Equals
-   public static bool operator !=(CustomAttributeNamedArgument left, CustomAttributeNamedArgument right);
-   public override string ToString();
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomAttributeTypedArgument {
    ^ Immo Landwerth: Expose Equals, GetHashCode, ==, and !=.
-   public CustomAttributeTypedArgument(object value);
-   public CustomAttributeTypedArgument(Type argumentType, object value);
    public Type ArgumentType { get; }
    public object Value { get; }
-   public override bool Equals(object obj);
-   public override int GetHashCode();
-   public static bool operator ==(CustomAttributeTypedArgument left, CustomAttributeTypedArgument right);
-   public static bool operator !=(CustomAttributeTypedArgument left, CustomAttributeTypedArgument right);
-   public override string ToString();
  }
  public sealed class DefaultMemberAttribute : Attribute {
    public DefaultMemberAttribute(string memberName);
    public string MemberName { get; }
  }
  public enum EventAttributes {
    None = 0,
-   ReservedMask = 1024,
    RTSpecialName = 1024,
    SpecialName = 512,
  }
  public abstract class EventInfo : MemberInfo, _EventInfo {
-   protected EventInfo();
    public virtual MethodInfo AddMethod { get; }
    public abstract EventAttributes Attributes { get; }
    public virtual Type EventHandlerType { get; }
    public virtual bool IsMulticast { get; }
    public bool IsSpecialName { get; }
    public override MemberTypes MemberType { get; }
    public virtual MethodInfo RaiseMethod { get; }
    public virtual MethodInfo RemoveMethod { get; }
    public virtual void AddEventHandler(object target, Delegate handler);
    public override bool Equals(object obj);
    public MethodInfo GetAddMethod();
    ^ Pallavi Taneja: Get*Method are exposed via S.R.TE. Consider removing from there.
    public abstractvirtual MethodInfo GetAddMethod(bool nonPublic);
    public override int GetHashCode();
-   public MethodInfo[] GetOtherMethods();
    ^ Pallavi Taneja: .other directive in events is not exposed in any language semantics we care about AFAIK and currently can be leveraged using ilasm only.
-   public virtual MethodInfo[] GetOtherMethods(bool nonPublic);
    public MethodInfo GetRaiseMethod();
    public abstractvirtual MethodInfo GetRaiseMethod(bool nonPublic);
    public MethodInfo GetRemoveMethod();
    public abstractvirtual MethodInfo GetRemoveMethod(bool nonPublic);
-   public static bool operator ==(EventInfo left, EventInfo right);
-   public static bool operator !=(EventInfo left, EventInfo right);
    public virtual void RemoveEventHandler(object target, Delegate handler);
-   void System.Runtime.InteropServices._EventInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._EventInfo.GetType();
-   void System.Runtime.InteropServices._EventInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._EventInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._EventInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
- public class ExceptionHandlingClause {
    ^ Pallavi Taneja: Not sure if we would want to expose SEH inspection or IL inspection in general.
-   protected ExceptionHandlingClause();
-   public virtual Type CatchType { get; }
-   public virtual int FilterOffset { get; }
-   public virtual ExceptionHandlingClauseOptions Flags { get; }
-   public virtual int HandlerLength { get; }
-   public virtual int HandlerOffset { get; }
-   public virtual int TryLength { get; }
-   public virtual int TryOffset { get; }
-   public override string ToString();
  }
- public enum ExceptionHandlingClauseOptions {
    ^ Pallavi Taneja: Same as above.
-   Clause = 0,
-   Fault = 4,
-   Filter = 1,
-   Finally = 2,
  }
  public enum FieldAttributes {
    Assembly = 3,
    FamANDAssem = 2,
    Family = 4,
    FamORAssem = 5,
    FieldAccessMask = 7,
    HasDefault = 32768,
    HasFieldMarshal = 4096,
    HasFieldRVA = 256,
    InitOnly = 32,
    Literal = 64,
    NotSerialized = 128,
    PinvokeImpl = 8192,
    Private = 1,
    PrivateScope = 0,
    Public = 6,
-   ReservedMask = 38144,
    RTSpecialName = 1024,
    SpecialName = 512,
    Static = 16,
  }
  public abstract class FieldInfo : MemberInfo, _FieldInfo {
-   protected FieldInfo();
    public abstract FieldAttributes Attributes { get; }
    public abstractvirtual RuntimeFieldHandle FieldHandle { get; }
    public abstract Type FieldType { get; }
    public bool IsAssembly { get; }
    public bool IsFamily { get; }
    public bool IsFamilyAndAssembly { get; }
    public bool IsFamilyOrAssembly { get; }
    public bool IsInitOnly { get; }
    public bool IsLiteral { get; }
-   public bool IsNotSerialized { get; }
    ^ Pallavi Taneja: We do not expose NotSerializedAttribute notion.
-   public bool IsPinvokeImpl { get; }
    public bool IsPrivate { get; }
    public bool IsPublic { get; }
-   public virtual bool IsSecurityCritical { get; }
-   public virtual bool IsSecuritySafeCritical { get; }
-   public virtual bool IsSecurityTransparent { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
    public override MemberTypes MemberType { get; }
    public override bool Equals(object obj);
    public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle);
    public static FieldInfo GetFieldFromHandle(RuntimeFieldHandle handle, RuntimeTypeHandle declaringType);
    public override int GetHashCode();
-   public virtual Type[] GetOptionalCustomModifiers();
    ^ Immo Landwerth: Expose
    ^ Pallavi Taneja: [Not sure] Used in Compiler only context. Usage < 1% except xamarin using GetRawConstantValue 10%. Should there not be an alternative in metadata instead?
-   public virtual object GetRawConstantValue();
-   public virtual Type[] GetRequiredCustomModifiers();
    public abstract object GetValue(object obj);
-   public virtual object GetValueDirect(TypedReference obj);
    ^ Pallavi Taneja: TypedReference is not exposed.
-   public static bool operator ==(FieldInfo left, FieldInfo right);
-   public static bool operator !=(FieldInfo left, FieldInfo right);
    public virtual void SetValue(object obj, object value);
-   public abstract void SetValue(object obj, object value, BindingFlags invokeAttr, Binder binder, CultureInfo culture);
-   public virtual void SetValueDirect(TypedReference obj, object value);
-   void System.Runtime.InteropServices._FieldInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._FieldInfo.GetType();
-   void System.Runtime.InteropServices._FieldInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._FieldInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._FieldInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
  public enum GenericParameterAttributes {
    Contravariant = 2,
    Covariant = 1,
    DefaultConstructorConstraint = 16,
    None = 0,
    NotNullableValueTypeConstraint = 8,
    ReferenceTypeConstraint = 4,
    SpecialConstraintMask = 28,
    VarianceMask = 3,
  }
- public interface ICustomAttributeProvider {
    ^ Pallavi Taneja: We do not expose ICustomAttributeProvider, however we expose the members via extension methods via S.R.E. Hence, exposing this correctly will be difficult.
-   object[] GetCustomAttributes(bool inherit);
-   object[] GetCustomAttributes(Type attributeType, bool inherit);
-   bool IsDefined(Type attributeType, bool inherit);
  }
- public enum ImageFileMachine {
    ^ Pallavi Taneja: Use S.R.I.RuntimeInformation instead
-   AMD64 = 34404,
-   ARM = 452,
-   I386 = 332,
-   IA64 = 512,
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct InterfaceMapping {
    public MethodInfo[] InterfaceMethods;
    public MethodInfo[] TargetMethods;
    public Type InterfaceType;
    public Type TargetType;
  }
  public static class IntrospectionExtensions {
    public static TypeInfo GetTypeInfo(this Type type);
  }
  public class InvalidFilterCriteriaException : ApplicationExceptionException {
    ^ Pallavi Taneja: Used my Module and TYpe. Although Type.Filter* can't be exposed yet, we can expose Module.Filter* members.
    public InvalidFilterCriteriaException();
-   protected InvalidFilterCriteriaException(SerializationInfo info, StreamingContext context);
    public InvalidFilterCriteriaException(string message);
    public InvalidFilterCriteriaException(string message, Exception inner);
  }
- public interface IReflect {
    ^ Pallavi Taneja: Exposed in S.R.IS as used with IDispatch.
-   Type UnderlyingSystemType { get; }
-   FieldInfo GetField(string name, BindingFlags bindingAttr);
-   FieldInfo[] GetFields(BindingFlags bindingAttr);
-   MemberInfo[] GetMember(string name, BindingFlags bindingAttr);
-   MemberInfo[] GetMembers(BindingFlags bindingAttr);
-   MethodInfo GetMethod(string name, BindingFlags bindingAttr);
-   MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, Type[] types, ParameterModifier[] modifiers);
-   MethodInfo[] GetMethods(BindingFlags bindingAttr);
-   PropertyInfo[] GetProperties(BindingFlags bindingAttr);
-   PropertyInfo GetProperty(string name, BindingFlags bindingAttr);
-   PropertyInfo GetProperty(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
-   object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, ParameterModifier[] modifiers, CultureInfo culture, string[] namedParameters);
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
  public delegate bool MemberFilter(MemberInfo m, object filterCriteria);
  public abstract class MemberInfo : _MemberInfo, ICustomAttributeProvider {
-   protected MemberInfo();
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public abstract Type DeclaringType { get; }
    public abstractvirtual MemberTypes MemberType { get; }
    public virtual int MetadataToken { get; }
    public virtual Module Module { get; }
    public abstract string Name { get; }
    public abstractvirtual Type ReflectedType { get; }
    ^ Pallavi Taneja: Some concerns around PN implementation.
    public override bool Equals(object obj);
-   public abstract object[] GetCustomAttributes(bool inherit);
    ^ Pallavi Taneja: Already exposed via extensions.
-   public abstract object[] GetCustomAttributes(Type attributeType, bool inherit);
-   public virtual IList<CustomAttributeData> GetCustomAttributesData();
    public override int GetHashCode();
-   public abstract bool IsDefined(Type attributeType, bool inherit);
-   public static bool operator ==(MemberInfo left, MemberInfo right);
    ^ Pallavi Taneja: Operator overloads behave different than .Equals().
-   public static bool operator !=(MemberInfo left, MemberInfo right);
-   void System.Runtime.InteropServices._MemberInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._MemberInfo.GetType();
-   void System.Runtime.InteropServices._MemberInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._MemberInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._MemberInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
  public enum MemberTypes {
    All = 191,
    Constructor = 1,
    Custom = 64,
    Event = 2,
    Field = 4,
    Method = 8,
    NestedType = 128,
    Property = 16,
    TypeInfo = 32,
  }
  public enum MethodAttributes {
    Abstract = 1024,
    Assembly = 3,
    CheckAccessOnOverride = 512,
    FamANDAssem = 2,
    Family = 4,
    FamORAssem = 5,
    Final = 32,
    HasSecurity = 16384,
    HideBySig = 128,
    MemberAccessMask = 7,
    NewSlot = 256,
    PinvokeImpl = 8192,
    Private = 1,
    PrivateScope = 0,
    Public = 6,
    RequireSecObject = 32768,
-   ReservedMask = 53248,
    ReuseSlot = 0,
    RTSpecialName = 4096,
    SpecialName = 2048,
    Static = 16,
    UnmanagedExport = 8,
    Virtual = 64,
    VtableLayoutMask = 256,
  }
  public abstract class MethodBase : MemberInfo, _MethodBase {
-   protected MethodBase();
    public abstract MethodAttributes Attributes { get; }
    public virtual CallingConventions CallingConvention { get; }
    public virtual bool ContainsGenericParameters { get; }
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
-   public virtual bool IsSecurityCritical { get; }
-   public virtual bool IsSecuritySafeCritical { get; }
-   public virtual bool IsSecurityTransparent { get; }
    public bool IsSpecialName { get; }
    public bool IsStatic { get; }
    public bool IsVirtual { get; }
    public abstractvirtual RuntimeMethodHandle MethodHandle { get; }
    public virtualabstract MethodImplAttributes MethodImplementationFlags { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsAbstract { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsAssembly { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsConstructor { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsFamily { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsFamilyAndAssembly { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsFamilyOrAssembly { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsFinal { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsHideBySig { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsPrivate { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsPublic { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsSpecialName { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsStatic { get; }
-   bool System.Runtime.InteropServices._MethodBase.IsVirtual { get; }
    public override bool Equals(object obj);
-   [MethodImpl(NoInlining)]public static MethodBase GetCurrentMethod();
    ^ Immo Landwerth: Don't expose
    public virtual Type[] GetGenericArguments();
    public override int GetHashCode();
-   public virtual MethodBody GetMethodBody();
    ^ Pallavi Taneja: [Not sure] Used for IL inspection. Usage ~ 1%
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle);
    public static MethodBase GetMethodFromHandle(RuntimeMethodHandle handle, RuntimeTypeHandle declaringType);
    public abstractvirtual MethodImplAttributes GetMethodImplementationFlags();
    public abstract ParameterInfo[] GetParameters();
    public virtual object Invoke(object obj, object[] parameters);
-   public abstract object Invoke(object obj, BindingFlags invokeAttr, Binder binder, object[] parameters, CultureInfo culture);
    ^ Pallavi Taneja: Dependency on System.Glob
-   public static bool operator ==(MethodBase left, MethodBase right);
-   public static bool operator !=(MethodBase left, MethodBase right);
-   void System.Runtime.InteropServices._MethodBase.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._MethodBase.GetType();
-   void System.Runtime.InteropServices._MethodBase.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._MethodBase.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._MethodBase.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
- public class MethodBody {
    ^ Pallavi Taneja: [Not sure] IL inspection
-   protected MethodBody();
-   public virtual IList<ExceptionHandlingClause> ExceptionHandlingClauses { get; }
-   public virtual bool InitLocals { get; }
-   public virtual int LocalSignatureMetadataToken { get; }
-   public virtual IList<LocalVariableInfo> LocalVariables { get; }
-   public virtual int MaxStackSize { get; }
-   public virtual byte[] GetILAsByteArray();
  }
  public enum MethodImplAttributes {
    AggressiveInlining = 256,
    CodeTypeMask = 3,
    ForwardRef = 16,
    IL = 0,
    InternalCall = 4096,
    Managed = 0,
    ManagedMask = 4,
-   MaxMethodImplVal = 65535,
    Native = 1,
    NoInlining = 8,
    NoOptimization = 64,
    OPTIL = 2,
    PreserveSig = 128,
    Runtime = 3,
    Synchronized = 32,
    Unmanaged = 4,
  }
  public abstract class MethodInfo : MethodBase, _MethodInfo {
-   protected MethodInfo();
    public override MemberTypes MemberType { get; }
    public virtual ParameterInfo ReturnParameter { get; }
    public virtual Type ReturnType { get; }
-   public abstract ICustomAttributeProvider ReturnTypeCustomAttributes { get; }
    ^ Pallavi Taneja: Issues with exposing ICustomAttributeProvider
    public virtual Delegate CreateDelegate(Type delegateType);
    public virtual Delegate CreateDelegate(Type delegateType, object target);
    public override bool Equals(object obj);
    public abstractvirtual MethodInfo GetBaseDefinition();
    public override Type[] GetGenericArguments();
    public virtual MethodInfo GetGenericMethodDefinition();
    public override int GetHashCode();
    public virtual MethodInfo MakeGenericMethod(params Type[] typeArguments);
-   public static bool operator ==(MethodInfo left, MethodInfo right);
-   public static bool operator !=(MethodInfo left, MethodInfo right);
-   void System.Runtime.InteropServices._MethodInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._MethodInfo.GetType();
-   void System.Runtime.InteropServices._MethodInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._MethodInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._MethodInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
- public sealed class Missing : ISerializable {
    ^ Immo Landwerth: Already exposed in S.R.IS
    ^ Pallavi Taneja: Interop sceanrio, used in IDispatch.
-   public static readonly Missing Value;
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
  }
  public abstract class Module : _Module, ICustomAttributeProvider, ISerializable {
    public static readonly TypeFilter FilterTypeName;
    public static readonly TypeFilter FilterTypeNameIgnoreCase;
-   protected Module();
    public virtual Assembly Assembly { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtual string FullyQualifiedName { get; }
    public virtual int MDStreamVersion { get; }
    public virtual int MetadataToken { get; }
-   public ModuleHandle ModuleHandle { get; }
    ^ Pallavi Taneja: ModuleHandle is not exposed.
    public virtual Guid ModuleVersionId { get; }
    public virtual string Name { get; }
    public virtual string ScopeName { get; }
    public override bool Equals(object o);
    public virtual Type[] FindTypes(TypeFilter filter, object filterCriteria);
-   public virtual object[] GetCustomAttributes(bool inherit);
-   public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
-   public virtual IList<CustomAttributeData> GetCustomAttributesData();
    public FieldInfo GetField(string name);
    public virtual FieldInfo GetField(string name, BindingFlags bindingAttr);
    public FieldInfo[] GetFields();
    public virtual FieldInfo[] GetFields(BindingFlags bindingFlags);
    public override int GetHashCode();
    public MethodInfo GetMethod(string name);
    public MethodInfo GetMethod(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
    public MethodInfo GetMethod(string name, Type[] types);
-   protected virtual MethodInfo GetMethodImpl(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
    public MethodInfo[] GetMethods();
    public virtual MethodInfo[] GetMethods(BindingFlags bindingFlags);
-   public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
-   public virtual void GetPEKind(out PortableExecutableKinds peKind, out ImageFileMachine machine);
    ^ Immo Landwerth: Don't expose.
-   public virtual X509Certificate GetSignerCertificate();
    ^ Immo Landwerth: Don't expose. Bad layering.
    public virtual Type GetType(string className);
    public virtual Type GetType(string className, bool ignoreCase);
    public virtual Type GetType(string className, bool throwOnError, bool ignoreCase);
    public virtual Type[] GetTypes();
-   public virtual bool IsDefined(Type attributeType, bool inherit);
-   public virtual bool IsResource();
-   public static bool operator ==(Module left, Module right);
-   public static bool operator !=(Module left, Module right);
    public FieldInfo ResolveField(int metadataToken);
    public virtual FieldInfo ResolveField(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
    public MemberInfo ResolveMember(int metadataToken);
    public virtual MemberInfo ResolveMember(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
    public MethodBase ResolveMethod(int metadataToken);
    public virtual MethodBase ResolveMethod(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
    public virtual byte[] ResolveSignature(int metadataToken);
    public virtual string ResolveString(int metadataToken);
    public Type ResolveType(int metadataToken);
    public virtual Type ResolveType(int metadataToken, Type[] genericTypeArguments, Type[] genericMethodArguments);
-   void System.Runtime.InteropServices._Module.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   void System.Runtime.InteropServices._Module.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._Module.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._Module.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
    public override string ToString();
  }
- public delegate Module ModuleResolveEventHandler(object sender, ResolveEventArgs e);
- public sealed class ObfuscateAssemblyAttribute : Attribute {
    ^ Pallavi Taneja: Not a reflection concept.
-   public ObfuscateAssemblyAttribute(bool assemblyIsPrivate);
-   public bool AssemblyIsPrivate { get; }
-   public bool StripAfterObfuscation { get; set; }
  }
- public sealed class ObfuscationAttribute : Attribute {
    ^ Pallavi Taneja: Same as above.s
-   public ObfuscationAttribute();
-   public bool ApplyToMembers { get; set; }
-   public bool Exclude { get; set; }
-   public string Feature { get; set; }
-   public bool StripAfterObfuscation { get; set; }
  }
  public enum ParameterAttributes {
    HasDefault = 4096,
    HasFieldMarshal = 8192,
    In = 1,
    Lcid = 4,
    None = 0,
    Optional = 16,
    Out = 2,
-   Reserved3 = 16384,
-   Reserved4 = 32768,
-   ReservedMask = 61440,
    Retval = 8,
  }
  public class ParameterInfo : _ParameterInfo, ICustomAttributeProvider, IObjectReference {
-   protected int PositionImpl;
-   protected object DefaultValueImpl;
-   protected MemberInfo MemberImpl;
-   protected ParameterAttributes AttrsImpl;
-   protected string NameImpl;
-   protected Type ClassImpl;
-   protected ParameterInfo();
    public virtual ParameterAttributes Attributes { get; }
    public virtual IEnumerable<CustomAttributeData> CustomAttributes { get; }
    public virtual object DefaultValue { get; }
    public virtual bool HasDefaultValue { get; }
    public bool IsIn { get; }
-   public bool IsLcid { get; }
    public bool IsOptional { get; }
    public bool IsOut { get; }
    public bool IsRetval { get; }
    public virtual MemberInfo Member { get; }
    public virtual int MetadataToken { get; }
    public virtual string Name { get; }
    public virtual Type ParameterType { get; }
    public virtual int Position { get; }
    public virtual object RawDefaultValue { get; }
-   public virtual object[] GetCustomAttributes(bool inherit);
-   public virtual object[] GetCustomAttributes(Type attributeType, bool inherit);
-   public virtual IList<CustomAttributeData> GetCustomAttributesData();
-   public virtual Type[] GetOptionalCustomModifiers();
    ^ Immo Landwerth: Expose
-   public object GetRealObject(StreamingContext context);
-   public virtual Type[] GetRequiredCustomModifiers();
    ^ Immo Landwerth: Expose
    ^ Pallavi Taneja: [Not sure] Compiler only scenario.
-   public virtual bool IsDefined(Type attributeType, bool inherit);
-   void System.Runtime.InteropServices._ParameterInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   void System.Runtime.InteropServices._ParameterInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._ParameterInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._ParameterInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
-   public override string ToString();
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ParameterModifier {
    public ParameterModifier(int parameterCount);
    public bool this[int index] { get; set; }
  }
- public sealed class Pointer : ISerializable {
    ^ Pallavi Taneja: Interop scenario.
-   public unsafe static object Box(void* ptr, Type type);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
-   public unsafe static void* Unbox(object ptr);
  }
- public enum PortableExecutableKinds {
    ^ Immo Landwerth: Is this needed? How does this relate to S.R.Metadata? It would suck to have two very similar enums in the same namespace. If the S.R.M lives in the S.R.M namespace, it's less of an issue.
-   ILOnly = 1,
-   NotAPortableExecutableImage = 0,
-   PE32Plus = 4,
-   Preferred32Bit = 16,
-   Required32Bit = 2,
-   Unmanaged32Bit = 8,
  }
  public enum ProcessorArchitecture {
    Amd64 = 4,
    Arm = 5,
    IA64 = 3,
    MSIL = 1,
    None = 0,
    X86 = 2,
  }
  public enum PropertyAttributes {
    HasDefault = 4096,
    None = 0,
-   Reserved2 = 8192,
-   Reserved3 = 16384,
-   Reserved4 = 32768,
-   ReservedMask = 62464,
    RTSpecialName = 1024,
    SpecialName = 512,
  }
  public abstract class PropertyInfo : MemberInfo, _PropertyInfo {
-   protected PropertyInfo();
    public abstract PropertyAttributes Attributes { get; }
    public abstract bool CanRead { get; }
    public abstract bool CanWrite { get; }
    public virtual MethodInfo GetMethod { get; }
    public bool IsSpecialName { get; }
    public override MemberTypes MemberType { get; }
    public abstract Type PropertyType { get; }
    public virtual MethodInfo SetMethod { get; }
    public override bool Equals(object obj);
    public MethodInfo[] GetAccessors();
    ^ Pallavi Taneja: Get* APIs exposed via TypeExtensions
    public abstractvirtual MethodInfo[] GetAccessors(bool nonPublic);
    public virtual object GetConstantValue();
    public MethodInfo GetGetMethod();
    public abstractvirtual MethodInfo GetGetMethod(bool nonPublic);
    public override int GetHashCode();
    public abstract ParameterInfo[] GetIndexParameters();
-   public virtual Type[] GetOptionalCustomModifiers();
    ^ Pallavi Taneja: Compiler only scenario.
-   public virtual object GetRawConstantValue();
-   public virtual Type[] GetRequiredCustomModifiers();
    public MethodInfo GetSetMethod();
    public abstractvirtual MethodInfo GetSetMethod(bool nonPublic);
    public object GetValue(object obj);
    public virtual object GetValue(object obj, object[] index);
-   public abstract object GetValue(object obj, BindingFlags invokeAttr, Binder binder, object[] index, CultureInfo culture);
    ^ Pallavi Taneja: CultureInfo dependency.
    ^ Immo Landwerth: Why does it take BindingFlags and CultureInfo.
-   public static bool operator ==(PropertyInfo left, PropertyInfo right);
-   public static bool operator !=(PropertyInfo left, PropertyInfo right);
    public void SetValue(object obj, object value);
    public virtual void SetValue(object obj, object value, object[] index);
-   public abstract void SetValue(object obj, object value, BindingFlags invokeAttr, Binder binder, object[] index, CultureInfo culture);
-   void System.Runtime.InteropServices._PropertyInfo.GetIDsOfNames(ref Guid riid, IntPtr rgszNames, uint cNames, uint lcid, IntPtr rgDispId);
-   Type System.Runtime.InteropServices._PropertyInfo.GetType();
-   void System.Runtime.InteropServices._PropertyInfo.GetTypeInfo(uint iTInfo, uint lcid, IntPtr ppTInfo);
-   void System.Runtime.InteropServices._PropertyInfo.GetTypeInfoCount(out uint pcTInfo);
-   void System.Runtime.InteropServices._PropertyInfo.Invoke(uint dispIdMember, ref Guid riid, uint lcid, short wFlags, IntPtr pDispParams, IntPtr pVarResult, IntPtr pExcepInfo, IntPtr puArgErr);
  }
  public abstract class ReflectionContext {
    protected ReflectionContext();
    public virtual TypeInfo GetTypeForObject(object value);
    public abstract Assembly MapAssembly(Assembly assembly);
    public abstract TypeInfo MapType(TypeInfo type);
  }
  public sealed class ReflectionTypeLoadException : SystemExceptionException, ISerializable {
    public ReflectionTypeLoadException(Type[] classes, Exception[] exceptions);
    public ReflectionTypeLoadException(Type[] classes, Exception[] exceptions, string message);
    public Exception[] LoaderExceptions { get; }
    public Type[] Types { get; }
-   public override void GetObjectData(SerializationInfo info, StreamingContext context);
  }
- public enum ResourceAttributes {
-   Private = 2,
-   Public = 1,
  }
  public enum ResourceLocation {
    ContainedInAnotherAssembly = 2,
    ContainedInManifestFile = 4,
    Embedded = 1,
  }
  public static class RuntimeReflectionExtensions {
    public static MethodInfo GetMethodInfo(this Delegate del);
    public static MethodInfo GetRuntimeBaseDefinition(this MethodInfo method);
    public static EventInfo GetRuntimeEvent(this Type type, string name);
    public static IEnumerable<EventInfo> GetRuntimeEvents(this Type type);
    public static FieldInfo GetRuntimeField(this Type type, string name);
    public static IEnumerable<FieldInfo> GetRuntimeFields(this Type type);
    public static InterfaceMapping GetRuntimeInterfaceMap(this TypeInfo typeInfo, Type interfaceType);
    public static MethodInfo GetRuntimeMethod(this Type type, string name, Type[] parameters);
    public static IEnumerable<MethodInfo> GetRuntimeMethods(this Type type);
    public static IEnumerable<PropertyInfo> GetRuntimeProperties(this Type type);
    public static PropertyInfo GetRuntimeProperty(this Type type, string name);
  }
- public class StrongNameKeyPair : IDeserializationCallback, ISerializable {
    ^ Pallavi Taneja: Is System.Reflection the right place for this?
-   public StrongNameKeyPair(byte[] keyPairArray);
-   public StrongNameKeyPair(FileStream keyPairFile);
-   protected StrongNameKeyPair(SerializationInfo info, StreamingContext context);
-   public StrongNameKeyPair(string keyPairContainer);
-   public byte[] PublicKey { get; }
-   void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
-   void System.Runtime.Serialization.ISerializable.GetObjectData(SerializationInfo info, StreamingContext context);
  }
  public class TargetException : ApplicationExceptionException {
    public TargetException();
-   protected TargetException(SerializationInfo info, StreamingContext context);
    public TargetException(string message);
    public TargetException(string message, Exception inner);
  }
  public sealed class TargetInvocationException : ApplicationExceptionException {
    public TargetInvocationException(Exception inner);
    public TargetInvocationException(string message, Exception inner);
  }
  public sealed class TargetParameterCountException : ApplicationExceptionException {
    public TargetParameterCountException();
    public TargetParameterCountException(string message);
    public TargetParameterCountException(string message, Exception inner);
  }
  public enum TypeAttributes {
    Abstract = 128,
    AnsiClass = 0,
    AutoClass = 131072,
    AutoLayout = 0,
    BeforeFieldInit = 1048576,
    Class = 0,
    ClassSemanticsMask = 32,
    CustomFormatClass = 196608,
    CustomFormatMask = 12582912,
    ExplicitLayout = 16,
    HasSecurity = 262144,
    Import = 4096,
    Interface = 32,
    LayoutMask = 24,
    NestedAssembly = 5,
    NestedFamANDAssem = 6,
    NestedFamily = 4,
    NestedFamORAssem = 7,
    NestedPrivate = 3,
    NestedPublic = 2,
    NotPublic = 0,
    Public = 1,
-   ReservedMask = 264192,
    RTSpecialName = 2048,
    Sealed = 256,
    SequentialLayout = 8,
    Serializable = 8192,
    SpecialName = 1024,
    StringFormatMask = 196608,
    UnicodeClass = 65536,
    VisibilityMask = 7,
    WindowsRuntime = 16384,
  }
- public class TypeDelegator : TypeInfo {
    ^ Pallavi Taneja: Used in extension scenarios.
-   protected Type typeImpl;
-   protected TypeDelegator();
-   public TypeDelegator(Type delegatingType);
-   public override Assembly Assembly { get; }
-   public override string AssemblyQualifiedName { get; }
-   public override Type BaseType { get; }
-   public override string FullName { get; }
-   public override Guid GUID { get; }
-   public override bool IsConstructedGenericType { get; }
-   public override int MetadataToken { get; }
-   public override Module Module { get; }
-   public override string Name { get; }
-   public override string Namespace { get; }
-   public override RuntimeTypeHandle TypeHandle { get; }
-   public override Type UnderlyingSystemType { get; }
-   protected override TypeAttributes GetAttributeFlagsImpl();
-   protected override ConstructorInfo GetConstructorImpl(BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
-   public override ConstructorInfo[] GetConstructors(BindingFlags bindingAttr);
-   public override object[] GetCustomAttributes(bool inherit);
-   public override object[] GetCustomAttributes(Type attributeType, bool inherit);
-   public override Type GetElementType();
-   public override EventInfo GetEvent(string name, BindingFlags bindingAttr);
-   public override EventInfo[] GetEvents();
-   public override EventInfo[] GetEvents(BindingFlags bindingAttr);
-   public override FieldInfo GetField(string name, BindingFlags bindingAttr);
-   public override FieldInfo[] GetFields(BindingFlags bindingAttr);
-   public override Type GetInterface(string name, bool ignoreCase);
-   public override InterfaceMapping GetInterfaceMap(Type interfaceType);
-   public override Type[] GetInterfaces();
-   public override MemberInfo[] GetMember(string name, MemberTypes type, BindingFlags bindingAttr);
-   public override MemberInfo[] GetMembers(BindingFlags bindingAttr);
-   protected override MethodInfo GetMethodImpl(string name, BindingFlags bindingAttr, Binder binder, CallingConventions callConvention, Type[] types, ParameterModifier[] modifiers);
-   public override MethodInfo[] GetMethods(BindingFlags bindingAttr);
-   public override Type GetNestedType(string name, BindingFlags bindingAttr);
-   public override Type[] GetNestedTypes(BindingFlags bindingAttr);
-   public override PropertyInfo[] GetProperties(BindingFlags bindingAttr);
-   protected override PropertyInfo GetPropertyImpl(string name, BindingFlags bindingAttr, Binder binder, Type returnType, Type[] types, ParameterModifier[] modifiers);
-   protected override bool HasElementTypeImpl();
-   public override object InvokeMember(string name, BindingFlags invokeAttr, Binder binder, object target, object[] args, ParameterModifier[] modifiers, CultureInfo culture, string[] namedParameters);
-   protected override bool IsArrayImpl();
-   public override bool IsAssignableFrom(TypeInfo typeInfo);
-   protected override bool IsByRefImpl();
-   protected override bool IsCOMObjectImpl();
-   public override bool IsDefined(Type attributeType, bool inherit);
-   protected override bool IsPointerImpl();
-   protected override bool IsPrimitiveImpl();
-   protected override bool IsValueTypeImpl();
  }
  public delegate bool TypeFilter(Type m, object filterCriteria);
  public abstract class TypeInfo : TypeMemberInfo, IReflectableType {
+   public abstract Assembly Assembly { get; }
+   public abstract string AssemblyQualifiedName { get; }
+   public abstract TypeAttributes Attributes { get; }
+   public abstract Type BaseType { get; }
+   public abstract bool ContainsGenericParameters { get; }
    public virtual IEnumerable<ConstructorInfo> DeclaredConstructors { get; }
    public virtual IEnumerable<EventInfo> DeclaredEvents { get; }
    public virtual IEnumerable<FieldInfo> DeclaredFields { get; }
    public virtual IEnumerable<MemberInfo> DeclaredMembers { get; }
    public virtual IEnumerable<MethodInfo> DeclaredMethods { get; }
    public virtual IEnumerable<TypeInfo> DeclaredNestedTypes { get; }
    public virtual IEnumerable<PropertyInfo> DeclaredProperties { get; }
+   public abstract MethodBase DeclaringMethod { get; }
+   public abstract string FullName { get; }
+   public abstract GenericParameterAttributes GenericParameterAttributes { get; }
+   public abstract int GenericParameterPosition { get; }
+   public abstract Type[] GenericTypeArguments { get; }
    public virtual Type[] GenericTypeParameters { get; }
+   public abstract Guid GUID { get; }
+   public bool HasElementType { get; }
    public virtual IEnumerable<Type> ImplementedInterfaces { get; }
+   public bool IsAbstract { get; }
+   public bool IsAnsiClass { get; }
+   public bool IsArray { get; }
+   public bool IsAutoClass { get; }
+   public bool IsAutoLayout { get; }
+   public bool IsByRef { get; }
+   public bool IsClass { get; }
+   public virtual bool IsCOMObject { get; }
+   public abstract bool IsEnum { get; }
+   public bool IsExplicitLayout { get; }
+   public abstract bool IsGenericParameter { get; }
+   public abstract bool IsGenericType { get; }
+   public abstract bool IsGenericTypeDefinition { get; }
+   public bool IsImport { get; }
+   public bool IsInterface { get; }
+   public bool IsLayoutSequential { get; }
+   public bool IsMarshalByRef { get; }
+   public bool IsNested { get; }
+   public bool IsNestedAssembly { get; }
+   public bool IsNestedFamANDAssem { get; }
+   public bool IsNestedFamily { get; }
+   public bool IsNestedFamORAssem { get; }
+   public bool IsNestedPrivate { get; }
+   public bool IsNestedPublic { get; }
+   public bool IsNotPublic { get; }
+   public bool IsPointer { get; }
+   public virtual bool IsPrimitive { get; }
+   public bool IsPublic { get; }
+   public bool IsSealed { get; }
+   public abstract bool IsSerializable { get; }
+   public bool IsSpecialName { get; }
+   public bool IsUnicodeClass { get; }
+   public virtual bool IsValueType { get; }
+   public bool IsVisible { get; }
+   public abstract string Namespace { get; }
+   public virtual StructLayoutAttribute StructLayoutAttribute { get; }
+   public ConstructorInfo TypeInitializer { get; }
+   public abstract Type UnderlyingSystemType { get; }
    public virtual Type AsType();
+   public virtual Type[] FindInterfaces(TypeFilter filter, object filterCriteria);
+   public virtual MemberInfo[] FindMembers(MemberTypes memberType, BindingFlags bindingAttr, MemberFilter filter, object filterCriteria);
+   public abstract int GetArrayRank();
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
+   public abstract Type GetElementType();
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
+   public abstract Type[] GetGenericParameterConstraints();
+   public abstract Type GetGenericTypeDefinition();
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
+   public virtual bool IsEquivalentTo(Type other);
+   public virtual bool IsInstanceOfType(object o);
+   public virtual bool IsSubclassOf(Type c);
+   public abstract Type MakeArrayType();
+   public abstract Type MakeArrayType(int rank);
+   public abstract Type MakeByRefType();
+   public abstract Type MakeGenericType(params Type[] typeArguments);
+   public abstract Type MakePointerType();
    TypeInfo System.Reflection.IReflectableType.GetTypeInfo();
  }
 }
```

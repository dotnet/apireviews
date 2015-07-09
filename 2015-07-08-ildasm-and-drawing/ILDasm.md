```diff
---C:\Users\t-safer\Documents\GitHub\corefx\src\ILDasmLibrary\ILDasmLibrary\bin\Debug\ILDasmLibrary.dll
assembly ILDasmLibrary {
  namespace ILDasmLibrary {
    public enum HandlerKind {
      Catch = 1,
      Fault = 4,
      Filter = 3,
      Finally = 2,
      Try = 0,
    }
    public class ILDasm {
      public ILDasm(Stream fileStream);
      ^ Immo Landwerth: Seems like the .ctors should be static methods on ILDasmAssembly
      public ILDasm(string path);
      public ILDasmAssembly Assembly { get; }
      ^ Immo Landwerth: If the MD reader exposes modules, so should this library
    }
    public class ILDasmAssembly : ILDasmObject {
      ^ Immo Landwerth: We should remove the ILDasm prefix from all the types; however,
        we should probably avoid naming conflicts with the MD reader as consumers are
        likely using both APIs.
      public IEnumerable<AssemblyReference> AssemblyReferences { get; }
      public string Culture { get; }
      public IEnumerable<CustomAttribute> CustomAttributes { get; }
      public string Flags { get; }
      public int HashAlgorithm { get; }
      public string Name { get; }
      public string PublicKey { get; }
      public IEnumerable<ILDasmTypeDefinition> TypeDefinitions { get; }
      public Version Version { get; }
      public string GetFormattedHashAlgorithm();
      public string GetFormattedVersion();
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ILDasmField {
      public ILDasmField(FieldDefinition fieldDefinition, MetadataReader mdReader);
      public FieldAttributes Attributes { get; }
      public bool HasDefault { get; }
      public string Name { get; }
      public string Signature { get; }
      public string Type { get; }
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ILDasmLocal {
      public ILDasmLocal(string name, string type);
      public string Name { get; }
      public string Type { get; }
    }
    public class ILDasmMethodDefinition : ILDasmObject {
      public MethodAttributes Attributes { get; }
      public IEnumerable<CustomAttribute> CustomAttributes { get; }
      public ImmutableArray<ExceptionRegion> ExceptionRegions { get; }
      public IEnumerable<string> GenericParameters { get; }
      public bool HasLocals { get; }
      public ImmutableArray<ILInstruction> Instructions { get; }
      public bool IsEntryPoint { get; }
      public bool IsImplementation { get; }
      public ILDasmLocal[] Locals { get; }
      public bool LocalVariablesInitialized { get; }
      public int MaxStack { get; }
      public int MethodDeclarationToken { get; }
      public string Name { get; }
      public ILDasmParameter[] Parameters { get; }
      public int RelativeVirtualAddress { get; }
      public MethodSignature<ILDasmType> Signature { get; }
      public int Size { get; }
      public int Token { get; }
      public string DumpMethod(bool showBytes=false);
      public string GetDecodedSignature();
      public string GetFormattedRva();
      public ILDasmLocal GetLocal(int index);
      public ILDasmParameter GetParameter(int index);
    }
    public abstract class ILDasmObject {
      ^ Immo Landwerth: Do we need this type at all? It seems it simply used for
        code sharing that should be handled via composition rather than inheritance
      protected string GetCachedValue(StringHandle value, ref string storage);
      ^ Immo Landwerth: Internal
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ILDasmParameter {
      public ILDasmParameter(string name, string type, bool optional);
      public bool IsOptional { get; }
      public string Name { get; }
      public string Type { get; }
    }
    public class ILDasmTypeDefinition : ILDasmObject {
      public TypeAttributes Attributes { get; }
      public string BaseType { get; }
      public IEnumerable<CustomAttribute> CustomAttributes { get; }
      public IEnumerable<EventDefinition> Events { get; }
      public IEnumerable<ILDasmField> FieldDefinitions { get; }
      public string FullName { get; }
      public IEnumerable<string> GenericParameters { get; }
      public IEnumerable<InterfaceImplementation> InterfaceImplementations { get; }
      public bool IsGeneric { get; }
      public bool IsInterface { get; }
      public bool IsNested { get; }
      public IEnumerable<ILDasmMethodDefinition> MethodDefinitions { get; }
      public string Name { get; }
      public string Namespace { get; }
      public IEnumerable<ILDasmTypeDefinition> NestedTypes { get; }
      public IEnumerable<PropertyDefinition> Properties { get; }
      public int Token { get; }
      public string Dump(bool showBytes=false);
      public int GetOverridenMethodToken(int methodBodyToken);
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
    public struct ILDasmWriter {
      public ILDasmWriter(string indent="  ", int indentation=0);
      public void DumpAssembly(ILDasmAssembly _assembly, StreamWriter file);
      public string DumpMethod(ILDasmMethodDefinition _methodDefinition, bool showBytes=false);
      public string DumpType(ILDasmTypeDefinition _typeDefinition, bool showBytes=false);
    }
  }
  namespace ILDasmLibrary.Decoder {
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential, Size=1)]
    public struct ILDasmDecoder {
      public static string DecodeCustomAttribute(CustomAttribute attribute, ILDasmMethodDefinition _methodDefinition);
      public static IEnumerable<ILInstruction> DecodeMethodBody(ILDasmMethodDefinition _methodDefinition);
      public static MethodSignature<ILDasmType> DecodeMethodSignature(MethodDefinition _methodDefinition, ILDasmTypeProvider _provider);
      public static string GetCustomAttributeBytes(CustomAttribute attribute, MetadataReader MdReader);
      public static bool IsFieldDefinition(int token);
      public static bool IsMemberReference(int token);
      public static bool IsMethodDefinition(int token);
      public static bool IsMethodSpecification(int token);
      public static bool IsStandaloneSignature(int token);
      public static bool IsTypeDefinition(int token);
      public static bool IsTypeReference(int token);
      public static bool IsTypeSpecification(int token);
      public static bool IsUserString(int token);
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ILDasmType {
      public ILDasmType(string name, bool isValueType, bool isClassType);
      public bool IsClassType { get; }
      public bool IsValueType { get; }
      public string Name { get; }
      public void Append(string str);
      public override string ToString();
      public string ToString(bool showBaseType);
    }
    [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ILDasmTypeProvider : ISignatureTypeProvider<ILDasmType>, ITypeProvider<ILDasmType> {
      public ILDasmTypeProvider(MetadataReader reader);
      public MetadataReader Reader { get; }
      public ILDasmType GetArrayType(ILDasmType elementType, ArrayShape shape);
      public ILDasmType GetByReferenceType(ILDasmType elementType);
      public ILDasmType GetFunctionPointerType(MethodSignature<ILDasmType> signature);
      public ILDasmType GetGenericInstance(ILDasmType genericType, ImmutableArray<ILDasmType> typeArguments);
      public ILDasmType GetGenericMethodParameter(int index);
      public ILDasmType GetGenericTypeParameter(int index);
      public ILDasmType GetModifiedType(ILDasmType unmodifiedType, ImmutableArray<CustomModifier<ILDasmType>> customModifiers);
      public string GetParameterList(MethodSignature<ILDasmType> signature, Nullable<ParameterHandleCollection> parameters=null);
      public ILDasmType GetPinnedType(ILDasmType elementType);
      public ILDasmType GetPointerType(ILDasmType elementType);
      public ILDasmType GetPrimitiveType(PrimitiveTypeCode typeCode);
      public ILDasmType GetSZArrayType(ILDasmType elementType);
      public ILDasmType GetTypeFromDefinition(TypeDefinitionHandle handle);
      public ILDasmType GetTypeFromReference(TypeReferenceHandle handle);
    }
  }
  namespace ILDasmLibrary.Instructions {
    public class ILDasmBranchInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public class ILDasmByteInstruction : ILInstruction {
      protected override string GetBytes();
    }
    public class ILDasmDoubleInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
      protected override string GetBytes();
    }
    public class ILDasmFloatInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
      protected override string GetBytes();
    }
    public class ILDasmInstructionWithNoValue : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public abstract class ILDasmInstructionWithValue<T> : ILInstruction {
      public int Token { get; }
      public T Value { get; }
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public class ILDasmIntInstruction : ILInstruction {
      protected override string GetBytes();
    }
    public class ILDasmLongInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
      protected override string GetBytes();
    }
    public abstract class ILDasmNumericValueInstruction<T> : ILDasmInstructionWithValue<T> {
      public string Bytes { get; }
      public override void Dump(StringBuilder sb, bool showBytes=false);
      protected abstract string GetBytes();
    }
    public class ILDasmShortBranchInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public class ILDasmStringInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public class ILDasmSwitchInstruction : ILInstruction {
      public int[] Jumps { get; }
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public class ILDasmVariableInstruction : ILInstruction {
      public override void Dump(StringBuilder sb, bool showBytes=false);
    }
    public abstract class ILInstruction {
      public OpCode opCode { get; }
      public int Size { get; }
      public abstract void Dump(StringBuilder sb, bool showBytes=false);
      protected void DumpBytes(StringBuilder sb, string bytes);
    }
  }
 }
```
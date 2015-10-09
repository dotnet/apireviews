```diff
---D:\Temp\System.Reflection.Metadata.dll
+++D:\Src\corefx\bin\Windows_NT.AnyCPU.Debug\System.Reflection.Metadata\System.Reflection.Metadata.dll
 namespace System.Reflection.Metadata {
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct BlobReader {
+   public BlobHandle ReadBlobHandle();
+   public object ReadConstant(ConstantTypeCode typeCode);
+   public DateTime ReadDateTime();
    ^ Immo Landwerth: Where is that used?
+   public decimal ReadDecimal();
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomDebugInformation {
    ^ Immo Landwerth: CustomDebugInfo
+   public GuidHandle Kind { get; }
+   public EntityHandle Parent { get; }
+   public BlobHandle Value { get; }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomDebugInformationHandle : IEquatable<CustomDebugInformationHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(CustomDebugInformationHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(CustomDebugInformationHandle left, CustomDebugInformationHandle right);
+   public static explicit operator CustomDebugInformationHandle (EntityHandle handle);
+   public static explicit operator CustomDebugInformationHandle (Handle handle);
+   public static implicit operator Handle (CustomDebugInformationHandle handle);
+   public static implicit operator EntityHandle (CustomDebugInformationHandle handle);
+   public static bool operator !=(CustomDebugInformationHandle left, CustomDebugInformationHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct CustomDebugInformationHandleCollection : IEnumerable, IEnumerable<CustomDebugInformationHandle>, IReadOnlyCollection<CustomDebugInformationHandle> {
+   public int Count { get; }
+   public CustomDebugInformationHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<CustomDebugInformationHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.CustomDebugInformationHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<CustomDebugInformationHandle> {
+     public CustomDebugInformationHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ public sealed class DebugMetadataHeader {
    ^ Immo Landwerth: What does this do?
+   public readonly MethodDefinitionHandle EntryPoint;
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct Document {
    ^ Immo Landwerth: Very generic SourceDocument.
+   public BlobHandle Hash { get; }
+   public GuidHandle HashAlgorithm { get; }
+   public GuidHandle Language { get; }
+   public DocumentNameBlobHandle Name { get; }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct DocumentHandle : IEquatable<DocumentHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(DocumentHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(DocumentHandle left, DocumentHandle right);
+   public static explicit operator DocumentHandle (EntityHandle handle);
+   public static explicit operator DocumentHandle (Handle handle);
+   public static implicit operator Handle (DocumentHandle handle);
+   public static implicit operator EntityHandle (DocumentHandle handle);
+   public static bool operator !=(DocumentHandle left, DocumentHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct DocumentHandleCollection : IEnumerable, IEnumerable<DocumentHandle>, IReadOnlyCollection<DocumentHandle> {
+   public int Count { get; }
+   public DocumentHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<DocumentHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.DocumentHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<DocumentHandle> {
+     public DocumentHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct DocumentNameBlobHandle : IEquatable<DocumentNameBlobHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(DocumentNameBlobHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(DocumentNameBlobHandle left, DocumentNameBlobHandle right);
+   public static explicit operator DocumentNameBlobHandle (BlobHandle handle);
+   public static implicit operator BlobHandle (DocumentNameBlobHandle handle);
+   public static bool operator !=(DocumentNameBlobHandle left, DocumentNameBlobHandle right);
  }
  public enum HandleKind : byte {
+   CustomDebugInformation = (byte)55,
+   Document = (byte)48,
+   ImportScope = (byte)53,
+   LocalConstant = (byte)52,
+   LocalScope = (byte)50,
+   MethodBody = (byte)49,
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ImportDefinition {
+   public BlobHandle Alias { get; }
+   public ImportDefinitionKind Kind { get; }
+   public AssemblyReferenceHandle TargetAssembly { get; }
+   public BlobHandle TargetNamespace { get; }
+   public EntityHandle TargetType { get; }
  }
+ public enum ImportDefinitionKind {
+   AliasAssemblyNamespace = 8,
+   AliasAssemblyReference = 6,
+   AliasNamespace = 7,
+   AliasType = 9,
+   ImportAssemblyNamespace = 2,
+   ImportAssemblyReferenceAlias = 5,
+   ImportNamespace = 1,
+   ImportType = 3,
+   ImportXmlNamespace = 4,
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ImportsBlobReader : IDisposable, IEnumerator, IEnumerator<ImportDefinition> {
+   public unsafe ImportsBlobReader(byte* buffer, int length);
+   public ImportDefinition Current { get; }
+   object System.Collections.IEnumerator.Current { get; }
+   public void Dispose();
+   public bool MoveNext();
+   public void Reset();
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ImportScope {
+   public BlobHandle Imports { get; }
+   public ImportScopeHandle Parent { get; }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ImportScopeCollection : IEnumerable, IEnumerable<ImportScopeHandle>, IReadOnlyCollection<ImportScopeHandle> {
+   public int Count { get; }
+   public ImportScopeCollection.Enumerator GetEnumerator();
+   IEnumerator<ImportScopeHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.ImportScopeHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<ImportScopeHandle> {
+     public ImportScopeHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct ImportScopeHandle : IEquatable<ImportScopeHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(ImportScopeHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(ImportScopeHandle left, ImportScopeHandle right);
+   public static explicit operator ImportScopeHandle (EntityHandle handle);
+   public static explicit operator ImportScopeHandle (Handle handle);
+   public static implicit operator Handle (ImportScopeHandle handle);
+   public static implicit operator EntityHandle (ImportScopeHandle handle);
+   public static bool operator !=(ImportScopeHandle left, ImportScopeHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalConstant {
+   public StringHandle Name { get; }
+   public BlobHandle Signature { get; }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalConstantHandle : IEquatable<LocalConstantHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(LocalConstantHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(LocalConstantHandle left, LocalConstantHandle right);
+   public static explicit operator LocalConstantHandle (EntityHandle handle);
+   public static explicit operator LocalConstantHandle (Handle handle);
+   public static implicit operator Handle (LocalConstantHandle handle);
+   public static implicit operator EntityHandle (LocalConstantHandle handle);
+   public static bool operator !=(LocalConstantHandle left, LocalConstantHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalConstantHandleCollection : IEnumerable, IEnumerable<LocalConstantHandle>, IReadOnlyCollection<LocalConstantHandle> {
+   public int Count { get; }
+   public LocalConstantHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<LocalConstantHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.LocalConstantHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<LocalConstantHandle> {
+     public LocalConstantHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalScope {
+   public int EndOffset { get; }
+   public ImportScopeHandle ImportScope { get; }
+   public int Length { get; }
+   public MethodDefinitionHandle Method { get; }
+   public int StartOffset { get; }
+   public LocalScopeHandleCollection.ChildrenEnumerator GetChildren();
+   public LocalConstantHandleCollection GetLocalConstants();
+   public LocalVariableHandleCollection GetLocalVariables();
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalScopeHandle : IEquatable<LocalScopeHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(LocalScopeHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(LocalScopeHandle left, LocalScopeHandle right);
+   public static explicit operator LocalScopeHandle (EntityHandle handle);
+   public static explicit operator LocalScopeHandle (Handle handle);
+   public static implicit operator Handle (LocalScopeHandle handle);
+   public static implicit operator EntityHandle (LocalScopeHandle handle);
+   public static bool operator !=(LocalScopeHandle left, LocalScopeHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalScopeHandleCollection : IEnumerable, IEnumerable<LocalScopeHandle>, IReadOnlyCollection<LocalScopeHandle> {
+   public int Count { get; }
+   public LocalScopeHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<LocalScopeHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.LocalScopeHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct ChildrenEnumerator : IDisposable, IEnumerator, IEnumerator<LocalScopeHandle> {
+     public LocalScopeHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<LocalScopeHandle> {
+     public LocalScopeHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalVariable {
+   public LocalVariableAttributes Attributes { get; }
+   public int Index { get; }
+   public StringHandle Name { get; }
  }
+ public enum LocalVariableAttributes {
+   DebuggerHidden = 1,
+   None = 0,
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalVariableHandle : IEquatable<LocalVariableHandle> {
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(LocalVariableHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(LocalVariableHandle left, LocalVariableHandle right);
+   public static explicit operator LocalVariableHandle (EntityHandle handle);
+   public static explicit operator LocalVariableHandle (Handle handle);
+   public static implicit operator Handle (LocalVariableHandle handle);
+   public static implicit operator EntityHandle (LocalVariableHandle handle);
+   public static bool operator !=(LocalVariableHandle left, LocalVariableHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct LocalVariableHandleCollection : IEnumerable, IEnumerable<LocalVariableHandle>, IReadOnlyCollection<LocalVariableHandle> {
+   public int Count { get; }
+   public LocalVariableHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<LocalVariableHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.LocalVariableHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<LocalVariableHandle> {
+     public LocalVariableHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
  public sealed class MetadataReader {
+   public CustomDebugInformationHandleCollection CustomDebugInformation { get; }
+   public DebugMetadataHeader DebugMetadataHeader { get; }
+   public DocumentHandleCollection Documents { get; }
+   public ImportScopeCollection ImportScopes { get; }
+   public LocalConstantHandleCollection LocalConstants { get; }
+   public LocalScopeHandleCollection LocalScopes { get; }
+   public LocalVariableHandleCollection LocalVariables { get; }
+   public MethodBodyHandleCollection MethodBodies { get; }
+   public CustomDebugInformation GetCustomDebugInformation(CustomDebugInformationHandle handle);
+   public CustomDebugInformationHandleCollection GetCustomDebugInformation(EntityHandle handle);
    ^ Immo Landwerth: Needs to be plural
+   public Document GetDocument(DocumentHandle handle);
+   public ImportScope GetImportScope(ImportScopeHandle handle);
+   public ImportsBlobReader GetImportsReader(BlobHandle handle);
+   public LocalConstant GetLocalConstant(LocalConstantHandle handle);
+   public LocalScope GetLocalScope(LocalScopeHandle handle);
+   public LocalScopeHandleCollection GetLocalScopes(MethodBodyHandle handle);
+   public LocalScopeHandleCollection GetLocalScopes(MethodDefinitionHandle handle);
+   public LocalVariable GetLocalVariable(LocalVariableHandle handle);
+   public MethodBody GetMethodBody(MethodBodyHandle handle);
+   public MethodBody GetMethodBody(MethodDefinitionHandle handle);
+   public SequencePointBlobReader GetSequencePointsReader(BlobHandle handle);
+   public string GetString(DocumentNameBlobHandle handle);
  }
  [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct MetadataStringComparer {
+   public bool Equals(DocumentNameBlobHandle handle, string value);
+   public bool Equals(DocumentNameBlobHandle handle, string value, bool ignoreCase);
+   public bool Equals(NamespaceDefinitionHandle handle, string value, bool ignoreCase);
+   public bool Equals(StringHandle handle, string value, bool ignoreCase);
+   public bool StartsWith(StringHandle handle, string value, bool ignoreCase);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct MethodBody {
    ^ Immo Landwerth: MethodDebugInfo
+   public StandaloneSignatureHandle LocalSignature { get; }
+   public BlobHandle SequencePoints { get; }
+   public MethodDefinitionHandle GetStateMachineKickoffMethod();
    ^ Immo Landwerth: Seems a bit too specific for a generic MD reader.
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct MethodBodyHandle : IEquatable<MethodBodyHandle> {
    ^ Immo Landwerth: MethodDebugInfoHandle
+   public bool IsNil { get; }
+   public override bool Equals(object obj);
+   public bool Equals(MethodBodyHandle other);
+   public override int GetHashCode();
+   public static bool operator ==(MethodBodyHandle left, MethodBodyHandle right);
+   public static explicit operator MethodBodyHandle (EntityHandle handle);
+   public static explicit operator MethodBodyHandle (Handle handle);
+   public static implicit operator Handle (MethodBodyHandle handle);
+   public static implicit operator EntityHandle (MethodBodyHandle handle);
+   public static bool operator !=(MethodBodyHandle left, MethodBodyHandle right);
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct MethodBodyHandleCollection : IEnumerable, IEnumerable<MethodBodyHandle>, IReadOnlyCollection<MethodBodyHandle> {
    ^ Immo Landwerth: MethodDebugInfoHandleCollection
+   public int Count { get; }
+   public MethodBodyHandleCollection.Enumerator GetEnumerator();
+   IEnumerator<MethodBodyHandle> System.Collections.Generic.IEnumerable<System.Reflection.Metadata.MethodBodyHandle>.GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
    public struct Enumerator : IDisposable, IEnumerator, IEnumerator<MethodBodyHandle> {
+     public MethodBodyHandle Current { get; }
+     object System.Collections.IEnumerator.Current { get; }
+     public bool MoveNext();
+     void System.Collections.IEnumerator.Reset();
+     void System.IDisposable.Dispose();
    }
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct SequencePoint : IEquatable<SequencePoint> {
    ^ Immo Landwerth: The fields should be properties.
+   public readonly int EndLine;
+   public const int HiddenLine = 16707566;
    ^ Immo Landwerth: Should this be public?
+   public readonly int Offset;
+   public readonly int StartLine;
+   public readonly DocumentHandle Document;
+   public readonly ushort EndColumn;
+   public readonly ushort StartColumn;
+   public bool IsHidden { get; }
+   public override bool Equals(object obj);
+   public bool Equals(SequencePoint other);
+   public override int GetHashCode();
  }
+ [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
  public struct SequencePointBlobReader : IDisposable, IEnumerator, IEnumerator<SequencePoint> {
    ^ Immo Landwerth: Should this be SequencePointCollection?
+   public unsafe SequencePointBlobReader(byte* buffer, int length);
+   public SequencePoint Current { get; }
+   object System.Collections.IEnumerator.Current { get; }
+   public void Dispose();
+   public bool MoveNext();
+   public void Reset();
  }
 }
 namespace System.Reflection.Metadata.Ecma335 {
  public static class MetadataTokens {
+   public static CustomDebugInformationHandle CustomDebugInformationHandle(int rowNumber);
+   public static DocumentHandle DocumentHandle(int rowNumber);
+   public static DocumentNameBlobHandle DocumentNameBlobHandle(int offset);
+   public static ImportScopeHandle ImportScopeHandle(int rowNumber);
+   public static LocalConstantHandle LocalConstantHandle(int rowNumber);
+   public static LocalScopeHandle LocalScopeHandle(int rowNumber);
+   public static LocalVariableHandle LocalVariableHandle(int rowNumber);
  }
  public enum TableIndex : byte {
+   CustomDebugInformation = (byte)55,
+   Document = (byte)48,
+   ImportScope = (byte)53,
+   LocalConstant = (byte)52,
+   LocalScope = (byte)50,
+   LocalVariable = (byte)51,
+   MethodBody = (byte)49,
+   StateMachineMethod = (byte)54,
  }
 }
```

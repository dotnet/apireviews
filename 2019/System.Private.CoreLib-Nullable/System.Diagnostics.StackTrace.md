# System.Diagnostics.StackTrace

```diff
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.StackTrace\before\System.Diagnostics.StackTrace.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.StackTrace\after\System.Diagnostics.StackTrace.dll
 namespace System.Diagnostics {
  public class StackFrame {
    public const int OFFSET_UNKNOWN = -1;
    public StackFrame();
    public StackFrame(bool fNeedFileInfo);
    public StackFrame(int skipFrames);
    public StackFrame(int skipFrames, bool fNeedFileInfo);
    public StackFrame(string? fileName, int lineNumber);
    public StackFrame(string? fileName, int lineNumber, int colNumber);
    public virtual int GetFileColumnNumber();
    public virtual int GetFileLineNumber();
    public virtual string? GetFileName();
    public virtual int GetILOffset();
    public virtual MethodBase? GetMethod();
    public virtual int GetNativeOffset();
    public override string ToString();
  }
  public static class StackFrameExtensions {
    public static IntPtr GetNativeImageBase(this StackFrame stackFrame);
    public static IntPtr GetNativeIP(this StackFrame stackFrame);
    public static bool HasILOffset(this StackFrame stackFrame);
    public static bool HasMethod(this StackFrame stackFrame);
    public static bool HasNativeImage(this StackFrame stackFrame);
    public static bool HasSource(this StackFrame stackFrame);
  }
  public class StackTrace {
    public const int METHODS_TO_SKIP = 0;
    public StackTrace();
    public StackTrace(bool fNeedFileInfo);
    public StackTrace(StackFrame frame);
    public StackTrace(Exception e);
    public StackTrace(Exception e, bool fNeedFileInfo);
    public StackTrace(Exception e, int skipFrames);
    public StackTrace(Exception e, int skipFrames, bool fNeedFileInfo);
    public StackTrace(int skipFrames);
    public StackTrace(int skipFrames, bool fNeedFileInfo);
    public virtual int FrameCount { get; }
    public virtual StackFrame? GetFrame(int index);
    public virtual StackFrame?[]? GetFrames();
    public override string ToString();
  }
 }
 namespace System.Diagnostics.SymbolStore {
  public interface ISymbolBinder {
    ISymbolReader? GetReader(int importer, string filename, string searchPath);
  }
  public interface ISymbolBinder1 {
    ISymbolReader? GetReader(IntPtr importer, string filename, string searchPath);
  }
  public interface ISymbolDocument {
    Guid CheckSumAlgorithmId { get; }
    Guid DocumentType { get; }
    bool HasEmbeddedSource { get; }
    Guid Language { get; }
    Guid LanguageVendor { get; }
    int SourceLength { get; }
    string URL { get; }
    int FindClosestLine(int line);
    byte[] GetCheckSum();
    byte[] GetSourceRange(int startLine, int startColumn, int endLine, int endColumn);
  }
  public interface ISymbolDocumentWriter {
    void SetCheckSum(Guid algorithmId, byte[] checkSum);
    void SetSource(byte[] source);
  }
  public interface ISymbolMethod {
    ISymbolScope RootScope { get; }
    int SequencePointCount { get; }
    SymbolToken Token { get; }
    ISymbolNamespace GetNamespace();
    int GetOffset(ISymbolDocument document, int line, int column);
    ISymbolVariable[] GetParameters();
    int[] GetRanges(ISymbolDocument document, int line, int column);
    ISymbolScope GetScope(int offset);
    void GetSequencePoints(int[]? offsets, ISymbolDocument[]? documents, int[]? lines, int[]? columns, int[]? endLines, int[]? endColumns);
    bool GetSourceStartEnd(ISymbolDocument[]? docs, int[]? lines, int[]? columns);
  }
  public interface ISymbolNamespace {
    string Name { get; }
    ISymbolNamespace[] GetNamespaces();
    ISymbolVariable[] GetVariables();
  }
  public interface ISymbolReader {
    SymbolToken UserEntryPoint { get; }
    ISymbolDocument? GetDocument(string url, Guid language, Guid languageVendor, Guid documentType);
    ISymbolDocument[] GetDocuments();
    ISymbolVariable[] GetGlobalVariables();
    ISymbolMethod? GetMethod(SymbolToken method);
    ISymbolMethod? GetMethod(SymbolToken method, int version);
    ISymbolMethod GetMethodFromDocumentPosition(ISymbolDocument document, int line, int column);
    ISymbolNamespace[] GetNamespaces();
    byte[] GetSymAttribute(SymbolToken parent, string name);
    ISymbolVariable[] GetVariables(SymbolToken parent);
  }
  public interface ISymbolScope {
    int EndOffset { get; }
    ISymbolMethod Method { get; }
    ISymbolScope Parent { get; }
    int StartOffset { get; }
    ISymbolScope[] GetChildren();
    ISymbolVariable[] GetLocals();
    ISymbolNamespace[] GetNamespaces();
  }
  public interface ISymbolVariable {
    int AddressField1 { get; }
    int AddressField2 { get; }
    int AddressField3 { get; }
    SymAddressKind AddressKind { get; }
    object Attributes { get; }
    int EndOffset { get; }
    string Name { get; }
    int StartOffset { get; }
    byte[] GetSignature();
  }
  public interface ISymbolWriter {
    void Close();
    void CloseMethod();
    void CloseNamespace();
    void CloseScope(int endOffset);
    ISymbolDocumentWriter DefineDocument(string url, Guid language, Guid languageVendor, Guid documentType);
    void DefineField(SymbolToken parent, string name, FieldAttributes attributes, byte[] signature, SymAddressKind addrKind, int addr1, int addr2, int addr3);
    void DefineGlobalVariable(string name, FieldAttributes attributes, byte[] signature, SymAddressKind addrKind, int addr1, int addr2, int addr3);
    void DefineLocalVariable(string name, FieldAttributes attributes, byte[] signature, SymAddressKind addrKind, int addr1, int addr2, int addr3, int startOffset, int endOffset);
    void DefineParameter(string name, ParameterAttributes attributes, int sequence, SymAddressKind addrKind, int addr1, int addr2, int addr3);
    void DefineSequencePoints(ISymbolDocumentWriter document, int[] offsets, int[] lines, int[] columns, int[] endLines, int[] endColumns);
    void Initialize(IntPtr emitter, string filename, bool fFullBuild);
    void OpenMethod(SymbolToken method);
    void OpenNamespace(string name);
    int OpenScope(int startOffset);
    void SetMethodSourceRange(ISymbolDocumentWriter startDoc, int startLine, int startColumn, ISymbolDocumentWriter endDoc, int endLine, int endColumn);
    void SetScopeRange(int scopeID, int startOffset, int endOffset);
    void SetSymAttribute(SymbolToken parent, string name, byte[] data);
    void SetUnderlyingWriter(IntPtr underlyingWriter);
    void SetUserEntryPoint(SymbolToken entryMethod);
    void UsingNamespace(string fullName);
  }
  public enum SymAddressKind {
    BitField = 9,
    ILOffset = 1,
    NativeOffset = 5,
    NativeRegister = 3,
    NativeRegisterRegister = 6,
    NativeRegisterRelative = 4,
    NativeRegisterStack = 7,
    NativeRVA = 2,
    NativeSectionOffset = 10,
    NativeStackRegister = 8,
  }
  public readonly struct SymbolToken {
    public SymbolToken(int val);
    public bool Equals(SymbolToken obj);
    public override bool Equals(object? obj);
    public override int GetHashCode();
    public int GetToken();
    public static bool operator ==(SymbolToken a, SymbolToken b);
    public static bool operator !=(SymbolToken a, SymbolToken b);
  }
  public class SymDocumentType {
    public static readonly Guid Text;
    public SymDocumentType();
  }
  public class SymLanguageType {
    public static readonly Guid Basic;
    public static readonly Guid C;
    public static readonly Guid Cobol;
    public static readonly Guid CPlusPlus;
    public static readonly Guid CSharp;
    public static readonly Guid ILAssembly;
    public static readonly Guid Java;
    public static readonly Guid JScript;
    public static readonly Guid MCPlusPlus;
    public static readonly Guid Pascal;
    public static readonly Guid SMC;
    public SymLanguageType();
  }
  public class SymLanguageVendor {
    public static readonly Guid Microsoft;
    public SymLanguageVendor();
  }
 }
```
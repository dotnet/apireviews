```diff
---\\cpvsbuild\drops\Roslyn\Roslyn-Master-Signed-Release\20151026.22
   namespace Microsoft.CodeAnalysis.CSharp.Scripting {
  public static class CSharpScript {
    public static Script<object> Create(string code, ScriptOptions options=null, Type globalsType=null, InteractiveAssemblyLoader assemblyLoader=null);
    public static Script<T> Create<T>(string code, ScriptOptions options=null, Type globalsType=null, InteractiveAssemblyLoader assemblyLoader=null);
    public static Task<object> EvaluateAsync(string code, ScriptOptions options=null, object globals=null, Type globalsType=null, CancellationToken cancellationToken=null);
    public static Task<T> EvaluateAsync<T>(string code, ScriptOptions options=null, object globals=null, Type globalsType=null, CancellationToken cancellationToken=null);
    public static Task<ScriptState<object>> RunAsync(string code, ScriptOptions options=null, object globals=null, Type globalsType=null, CancellationToken cancellationToken=null);
    public static Task<ScriptState<T>> RunAsync<T>(string code, ScriptOptions options=null, object globals=null, Type globalsType=null, CancellationToken cancellationToken=null);
  }
 }
 namespace Microsoft.CodeAnalysis.CSharp.Scripting.Hosting {
  public sealed class CSharpObjectFormatter : ObjectFormatter {
    public static CSharpObjectFormatter Instance { get; }
  }
 }
 namespace Microsoft.CodeAnalysis.Scripting {
  public sealed class CompilationErrorException : Exception {
    ^ Immo Landwerth: Should have public .ctors
    public ImmutableArray<Diagnostic> Diagnostics { get; }
  }
  public abstract class Script {
    public string Code { get; }
    public Type GlobalsType { get; }
    public ScriptOptions Options { get; }
    public Script Previous { get; }
    public abstract Type ReturnType { get; }
    public ImmutableArray<Diagnostic> Build(CancellationToken cancellationToken=null);
    ^ Immo Landwerth: Should be called Compile
    public Script<object> ContinueWith(string code, ScriptOptions options=null);
    public Script<TResult> ContinueWith<TResult>(string code, ScriptOptions options=null);
    public Compilation GetCompilation();
    public Task<ScriptState> RunAsync(object globals=null, CancellationToken cancellationToken=null);
    public Script WithCode(string code);
    ^ Immo Landwerth: Remove. Confusion with ContinueWith.
    public Script WithGlobalsType(Type globalsType);
    ^ Immo Landwerth: Remove. Unclear what the chaining semantics are.
    public Script WithOptions(ScriptOptions options);
  }
  public sealed class Script<T> : Script {
    public override Type ReturnType { get; }
    public ScriptRunner<T> CreateDelegate(CancellationToken cancellationToken=null);
    public new Task<ScriptState<T>> RunAsync(object globals=null, CancellationToken cancellationToken=null);
    public new Script<T> WithCode(string code);
    ^ Immo Landwerth: Same
    public new Script<T> WithGlobalsType(Type globalsType);
    ^ Immo Landwerth: Same
    public new Script<T> WithOptions(ScriptOptions options);
  }
  public sealed class ScriptMetadataResolver : MetadataReferenceResolver, IEquatable<ScriptMetadataResolver> {
    public string BaseDirectory { get; }
    public static ScriptMetadataResolver Default { get; }
    public override bool ResolveMissingAssemblies { get; }
    public ImmutableArray<string> SearchPaths { get; }
    public bool Equals(ScriptMetadataResolver other);
    public override bool Equals(object other);
    public override int GetHashCode();
    public override PortableExecutableReference ResolveMissingAssembly(MetadataReference definition, AssemblyIdentity referenceIdentity);
    public override ImmutableArray<PortableExecutableReference> ResolveReference(string reference, string baseFilePath, MetadataReferenceProperties properties);
    public ScriptMetadataResolver WithBaseDirectory(string baseDirectory);
    public ScriptMetadataResolver WithSearchPaths(IEnumerable<string> searchPaths);
    public ScriptMetadataResolver WithSearchPaths(ImmutableArray<string> searchPaths);
    public ScriptMetadataResolver WithSearchPaths(params string[] searchPaths);
  }
  public sealed class ScriptOptions {
    public static readonly ScriptOptions Default;
    ^ Immo Landwerth: Make a property
    public string FilePath { get; }
    public ImmutableArray<string> Imports { get; }
    public ImmutableArray<MetadataReference> MetadataReferences { get; }
    public MetadataReferenceResolver MetadataResolver { get; }
    public SourceReferenceResolver SourceResolver { get; }
    public ScriptOptions AddImports(IEnumerable<string> imports);
    public ScriptOptions AddImports(params string[] imports);
    public ScriptOptions AddReferences(params MetadataReference[] references);
    public ScriptOptions AddReferences(IEnumerable<MetadataReference> references);
    public ScriptOptions AddReferences(IEnumerable<Assembly> references);
    public ScriptOptions AddReferences(IEnumerable<string> references);
    public ScriptOptions AddReferences(params Assembly[] references);
    public ScriptOptions AddReferences(params string[] references);
    public ScriptOptions WithFilePath(string filePath);
    public ScriptOptions WithImports(IEnumerable<string> imports);
    public ScriptOptions WithImports(params string[] imports);
    public ScriptOptions WithMetadataResolver(MetadataReferenceResolver resolver);
    public ScriptOptions WithReferences(params MetadataReference[] references);
    public ScriptOptions WithReferences(IEnumerable<MetadataReference> references);
    public ScriptOptions WithReferences(IEnumerable<Assembly> references);
    public ScriptOptions WithReferences(IEnumerable<string> references);
    public ScriptOptions WithReferences(params Assembly[] references);
    public ScriptOptions WithReferences(params string[] references);
    public ScriptOptions WithSourceResolver(SourceReferenceResolver resolver);
  }
  public delegate Task<T> ScriptRunner<T>(object globals=null, CancellationToken cancellationToken=null);
  public sealed class ScriptSourceResolver : SourceFileResolver, IEquatable<ScriptSourceResolver> {
    ^ Immo Landwerth: You should consider having == because IEquatable<>. Perhaps you want to offload Equals into a separate comparer.
    public static ScriptSourceResolver Default { get; }
    public bool Equals(ScriptSourceResolver other);
    public override bool Equals(object obj);
    public override int GetHashCode();
    public ScriptSourceResolver WithBaseDirectory(string baseDirectory);
    public ScriptSourceResolver WithSearchPaths(IEnumerable<string> searchPaths);
    public ScriptSourceResolver WithSearchPaths(ImmutableArray<string> searchPaths);
    public ScriptSourceResolver WithSearchPaths(params string[] searchPaths);
  }
  public abstract class ScriptState {
    public object ReturnValue { get; }
    public Script Script { get; }
    public ImmutableArray<ScriptVariable> Variables { get; }
    public Task<ScriptState<object>> ContinueWithAsync(string code, ScriptOptions options=null, CancellationToken cancellationToken=null);
    public Task<ScriptState<TResult>> ContinueWithAsync<TResult>(string code, ScriptOptions options=null, CancellationToken cancellationToken=null);
    public ScriptVariable GetVariable(string name);
    ^ Immo Landwerth: LookupVariable()? This conveys language specific lookup rules around hiding. Should throw if name doesn't exist at all. If you don't want to throw TryLookupVariable(name, out ScriptVariable)
  }
  public sealed class ScriptState<T> : ScriptState {
    public new T ReturnValue { get; }
  }
  public sealed class ScriptVariable {
    ^ Immo Landwerth: We should probably expose whether the variable is read-only.
    public string Name { get; }
    public Type Type { get; }
    public object Value { get; set; }
  }
 }
 namespace Microsoft.CodeAnalysis.Scripting.Hosting {
  public class CommandLineScriptGlobals {
    public CommandLineScriptGlobals(TextWriter outputWriter, ObjectFormatter objectFormatter);
    public IList<string> Args { get; }
    public void Print(object value);
  }
  public sealed class InteractiveAssemblyLoader : IDisposable {
    ^ Immo Landwerth: Consider introducing a really simple base class that doesn't leak any policy.
    public InteractiveAssemblyLoader(MetadataShadowCopyProvider shadowCopyProvider=null);
    public void Dispose();
    public void RegisterDependency(AssemblyIdentity dependency, string path);
    public void RegisterDependency(Assembly dependency);
  }
  public class InteractiveScriptGlobals {
    public InteractiveScriptGlobals(TextWriter outputWriter, ObjectFormatter objectFormatter);
    public IList<string> Args { get; }
    public IList<string> ReferencePaths { get; }
    public IList<string> SourcePaths { get; }
    public void Print(object value);
  }
  public sealed class MetadataShadowCopy {
    public ShadowCopy DocumentationFile { get; }
    public Metadata Metadata { get; }
    public ShadowCopy PrimaryModule { get; }
  }
  public sealed class MetadataShadowCopyProvider : IDisposable {
    public MetadataShadowCopyProvider(string directory=null, IEnumerable<string> noShadowCopyDirectories=null, CultureInfo documentationCommentsCulture=null);
    public void Dispose();
    ~MetadataShadowCopyProvider();
    public Metadata GetMetadata(string fullPath, MetadataImageKind kind);
    public MetadataShadowCopy GetMetadataShadowCopy(string fullPath, MetadataImageKind kind);
    public bool IsShadowCopy(string fullPath);
    public bool NeedsShadowCopy(string fullPath);
    public void SuppressShadowCopy(string originalPath);
  }
  public abstract class ObjectFormatter {
    public string FormatObject(object obj);
  }
  public sealed class ShadowCopy {
    public string FullPath { get; }
    public string OriginalPath { get; }
  }
 }
```
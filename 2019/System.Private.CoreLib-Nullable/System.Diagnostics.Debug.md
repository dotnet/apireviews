# System.Diagnostics.Debug

```diff
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Debug\before\System.Diagnostics.Debug.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Debug\after\System.Diagnostics.Debug.dll
 namespace System.Diagnostics {
  public static class Debug {
    public static bool AutoFlush { get; set; }
    public static int IndentLevel { get; set; }
    public static int IndentSize { get; set; }
    public static void Assert(bool condition);
    public static void Assert(bool condition, string? message);
    public static void Assert(bool condition, string? message, string? detailMessage);
    public static void Assert(bool condition, string? message, string detailMessageFormat, params object?[] args);
    public static void Close();
    public static void Fail(string? message);
    public static void Fail(string? message, string? detailMessage);
    public static void Flush();
    public static void Indent();
    public static void Print(string? message);
    public static void Print(string format, params object?[] args);
    public static void Unindent();
    public static void Write(object? value);
    public static void Write(object? value, string? category);
    public static void Write(string? message);
    public static void Write(string? message, string? category);
    public static void WriteIf(bool condition, object? value);
    public static void WriteIf(bool condition, object? value, string? category);
    public static void WriteIf(bool condition, string? message);
    public static void WriteIf(bool condition, string? message, string? category);
    public static void WriteLine(object? value);
    public static void WriteLine(object? value, string? category);
    public static void WriteLine(string? message);
    public static void WriteLine(string format, params object?[] args);
    public static void WriteLine(string? message, string? category);
    public static void WriteLineIf(bool condition, object? value);
    public static void WriteLineIf(bool condition, object? value, string? category);
    public static void WriteLineIf(bool condition, string? message);
    public static void WriteLineIf(bool condition, string? message, string? category);
  }
  public static class Debugger {
    public static readonly string? DefaultCategory;
    public static bool IsAttached { get; }
    public static void Break();
    public static bool IsLogging();
    public static bool Launch();
    public static void Log(int level, string? category, string? message);
    public static void NotifyOfCrossThreadDependency();
  }
  public sealed class DebuggerBrowsableAttribute : Attribute {
    public DebuggerBrowsableAttribute(DebuggerBrowsableState state);
    public DebuggerBrowsableState State { get; }
  }
  public enum DebuggerBrowsableState {
    Collapsed = 2,
    Never = 0,
    RootHidden = 3,
  }
  public sealed class DebuggerDisplayAttribute : Attribute {
    public DebuggerDisplayAttribute(string? value);
    public string? Name { get; set; }
    public Type? Target { get; set; }
    public string? TargetTypeName { get; set; }
    public string? Type { get; set; }
    public string Value { get; }
  }
  public sealed class DebuggerHiddenAttribute : Attribute {
    public DebuggerHiddenAttribute();
  }
  public sealed class DebuggerNonUserCodeAttribute : Attribute {
    public DebuggerNonUserCodeAttribute();
  }
  public sealed class DebuggerStepperBoundaryAttribute : Attribute {
    public DebuggerStepperBoundaryAttribute();
  }
  public sealed class DebuggerStepThroughAttribute : Attribute {
    public DebuggerStepThroughAttribute();
  }
  public sealed class DebuggerTypeProxyAttribute : Attribute {
    public DebuggerTypeProxyAttribute(string typeName);
    public DebuggerTypeProxyAttribute(Type type);
    public string ProxyTypeName { get; }
    public Type? Target { get; set; }
    public string? TargetTypeName { get; set; }
  }
  public sealed class DebuggerVisualizerAttribute : Attribute {
    public DebuggerVisualizerAttribute(string visualizerTypeName);
    public DebuggerVisualizerAttribute(string visualizerTypeName, string? visualizerObjectSourceTypeName);
    public DebuggerVisualizerAttribute(string visualizerTypeName, Type visualizerObjectSource);
    public DebuggerVisualizerAttribute(Type visualizer);
    public DebuggerVisualizerAttribute(Type visualizer, string? visualizerObjectSourceTypeName);
    public DebuggerVisualizerAttribute(Type visualizer, Type visualizerObjectSource);
    public string? Description { get; set; }
    public Type? Target { get; set; }
    public string? TargetTypeName { get; set; }
    public string? VisualizerObjectSourceTypeName { get; }
    public string VisualizerTypeName { get; }
  }
 }
```
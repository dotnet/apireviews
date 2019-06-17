# System.Diagnostics.Tools

```diff
---\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Tools\before\System.Diagnostics.Tools.dll
+++\\scratch2\scratch\safern\RefAssembliesNullable\System.Diagnostics.Tools\after\System.Diagnostics.Tools.dll
 namespace System.CodeDom.Compiler {
  public sealed class GeneratedCodeAttribute : Attribute {
    public GeneratedCodeAttribute(string? tool, string? version);
    public string? Tool { get; }
    public string? Version { get; }
  }
 }
 namespace System.Diagnostics.CodeAnalysis {
  public sealed class ExcludeFromCodeCoverageAttribute : Attribute {
    public ExcludeFromCodeCoverageAttribute();
  }
  public sealed class SuppressMessageAttribute : Attribute {
    public SuppressMessageAttribute(string category, string checkId);
    public string Category { get; }
    public string CheckId { get; }
    public string? Justification { get; set; }
    public string? MessageId { get; set; }
    public string? Scope { get; set; }
    public string? Target { get; set; }
  }
 }
```

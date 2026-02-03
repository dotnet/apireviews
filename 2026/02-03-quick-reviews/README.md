# API Review 02/03/2026

## Function approvals and MCP abstractions

**NeedsWork** | [#extensions/7254](https://github.com/dotnet/extensions/issues/7254#issuecomment-3843547859) | [Video](https://www.youtube.com/watch?v=RcGT6LvMsdo&t=0h0m0s)


* FunctionApprovalResponseContent.Reason should be get-only, and reason moved to the ctor.
* A very long discussion renamed FunctionCallContent.InvocationRequired to InformationalOnly
* FunctionApprovalRequestContent's constructor doesn't seem like it needs a requestId parameter, it should just call base(functionCall.CallId) (or change RequestId to be an abstract property)
* Marking as needs-work because we ran out of time (even after a 40 minute extension)

```csharp
namespace Microsoft.Extensions.AI;

// Contents
[JsonPolymorphic]
[JsonDerivedType(typeof(FunctionApprovalRequestContent), "functionApprovalRequest")]
public class InputRequestContent : AIContent
{
    protected InputRequestContent(string requestId);
    public string RequestId { get; }
}

[JsonPolymorphic]
[JsonDerivedType(typeof(FunctionApprovalResponseContent), "functionApprovalResponse")]
public class InputResponseContent : AIContent
{
    protected InputResponseContent(string requestId);
    public string RequestId { get; }
}

public sealed class FunctionApprovalRequestContent : InputRequestContent
{
    public FunctionApprovalRequestContent(FunctionCallContent functionCall);
    public FunctionCallContent FunctionCall { get; }
    public FunctionApprovalResponseContent CreateResponse(bool approved, string? reason = null);
}

public sealed class FunctionApprovalResponseContent : InputResponseContent
{
    public FunctionApprovalResponseContent(string requestId, bool approved, FunctionCallContent functionCall, string? reason = null);
    public bool Approved { get; }
    public FunctionCallContent FunctionCall { get; }
    public string? Reason { get; }
}

public sealed class McpServerToolCallContent : FunctionCallContent
{
    public McpServerToolCallContent(string callId, string name, string? serverName);
    public string? ServerName { get; }
}

public sealed class McpServerToolResultContent : FunctionResultContent
{
    public McpServerToolResultContent(string callId);
}

[JsonPolymorphic]
[JsonDerivedType(typeof(McpServerToolCallContent), "mcpServerToolCall")]
public class FunctionCallContent : AIContent // Existing type, attributes are new.
{
    [JsonConstructor]
    public FunctionCallContent(string callId, string name, IDictionary<string, object?>? arguments = null);
    public string CallId { get; }
    public string Name { get; }
    public IDictionary<string, object?>? Arguments { get; set; }
    [JsonIgnore]
    public Exception? Exception { get; set; }
    public bool InformationalOnly { get; set; } = true;
    public static FunctionCallContent CreateFromParsedArguments<TEncoding>(
        TEncoding encodedArguments,
        string callId,
        string name,
        Func<TEncoding, IDictionary<string, object?>?> argumentParser);
    [DebuggerBrowsable(DebuggerBrowsableState.Never)]
    private string DebuggerDisplay { get; }
}

[JsonPolymorphic] 
[JsonDerivedType(typeof(McpServerToolResultContent), "mcpServerToolResult")]
public class FunctionResultContent : AIContent // Existing type, attributes are new.
{
    [JsonConstructor]
    public FunctionResultContent(string callId, object? result);
    public string CallId { get; }
    public object? Result { get; set; }
    [JsonIgnore]
    public Exception? Exception { get; set; }
    [DebuggerBrowsable(DebuggerBrowsableState.Never)]
    private string DebuggerDisplay { get; }
}

// Tools.
public sealed class ApprovalRequiredAIFunction : DelegatingAIFunction
{
    public ApprovalRequiredAIFunction(AIFunction innerFunction);
}

public class HostedMcpServerTool : AITool
{
    public HostedMcpServerTool(string serverName, string serverAddress);
    public HostedMcpServerTool(string serverName, string serverAddress, IReadOnlyDictionary<string, object?>? additionalProperties);
    public HostedMcpServerTool(string serverName, Uri serverUrl);
    public HostedMcpServerTool(string serverName, Uri serverUrl, IReadOnlyDictionary<string, object?>? additionalProperties);
    public override IReadOnlyDictionary<string, object?> AdditionalProperties { get; }
    public IList<string>? AllowedTools { get; set; }
    public HostedMcpServerToolApprovalMode? ApprovalMode { get; set; }
    public string? AuthorizationToken { get; set; }
    public IDictionary<string, string> Headers { get; }
    public override string Name { get; }
    public string ServerAddress { get; }
    public string? ServerDescription { get; set; }
    public string ServerName { get; }
}

// HostedMcpServerTool utilities.
[JsonPolymorphic]
[JsonDerivedType(typeof(HostedMcpServerToolAlwaysRequireApprovalMode), "always")]
[JsonDerivedType(typeof(HostedMcpServerToolNeverRequireApprovalMode), "never")]
[JsonDerivedType(typeof(HostedMcpServerToolRequireSpecificApprovalMode), "requireSpecific")]
public class HostedMcpServerToolApprovalMode // Follows the pattern of `ChatToolMode`.
{
    public static HostedMcpServerToolAlwaysRequireApprovalMode AlwaysRequire { get; }
    public static HostedMcpServerToolNeverRequireApprovalMode NeverRequire { get; }
    public static HostedMcpServerToolRequireSpecificApprovalMode RequireSpecific(IList<string>? alwaysRequireApprovalToolNames, IList<string>? neverRequireApprovalToolNames);
}

[DebuggerDisplay("AlwaysRequire")]
public sealed class HostedMcpServerToolAlwaysRequireApprovalMode : HostedMcpServerToolApprovalMode
{
    public HostedMcpServerToolAlwaysRequireApprovalMode();
    public override bool Equals(object? obj);
    public override int GetHashCode();
}

[DebuggerDisplay("NeverRequire")]
public sealed class HostedMcpServerToolNeverRequireApprovalMode : HostedMcpServerToolApprovalMode
{
    public HostedMcpServerToolNeverRequireApprovalMode();
    public override bool Equals(object? obj);
    public override int GetHashCode();
}

public sealed class HostedMcpServerToolRequireSpecificApprovalMode : HostedMcpServerToolApprovalMode
{
    public HostedMcpServerToolRequireSpecificApprovalMode(IList<string>? alwaysRequireApprovalToolNames, IList<string>? neverRequireApprovalToolNames);
    public IList<string>? AlwaysRequireApprovalToolNames { get; set; }
    public IList<string>? NeverRequireApprovalToolNames { get; set; }
    public override bool Equals(object? obj);
    public override int GetHashCode();
}
```

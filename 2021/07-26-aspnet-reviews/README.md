# API Review 07/26/2021

## JS root components for server and webview

**Rejected** | [#aspnetcore/34597](https://github.com/dotnet/aspnetcore/pull/34597#issuecomment-886507576)

Updated and hopefully very done now. CC @aspnet-blazor-eng for review.
## Added option to enable delayed socket operations

**Approved** | [#aspnetcore/34639](https://github.com/dotnet/aspnetcore/pull/34639#issuecomment-886897977)

API approved with a rename to the property name: `DelaySocketOptions` -> `DelaySocketOperations`


## Expose SNI hostname via a feature

**Approved** | [#aspnetcore/34525](https://github.com/dotnet/aspnetcore/issues/34525#issuecomment-886900757)

We think it's better to add an additional feature interface to Connection.Abstractions rather than adding this to `ITlsConnectionFeature`:

```diff
namespace Microsoft.AspNetCore.Connection.Abstractions
{
+    public interface ITlsServerNameFeature
+    {
+        string? ServerName { get; }
+    }
}
```
## Add static method to create ConnectionContext from Socket

**Approved** | [#aspnetcore/34522](https://github.com/dotnet/aspnetcore/issues/34522#issuecomment-886918623)

```csharp
namespace Microsoft.AspNetCore.Server.Kestrel.Transport.Sockets
{
    // The implementation will also likely be disposable.
    public interface ISocketConnectionContextFactory
    {
        ConnectionContext Create(Socket socket, SocketConnectionOptions options);
    }

    // sealed? I could see extending this for a custom ISocketConnectionContextFactory.
    public sealed class SocketConnectionOptions
    {
        public PipeOptions InputOptions { get; init; } = new PipeOptions();
        public PipeOptions OutputOptions { get; init; } = new PipeOptions();
        
        // A mouthful, but consistent with SocketTransportOptions.
        public bool WaitForDataBeforeAllocatingBuffer { get; init; } = true;
        public bool DelaySocketOperations { get; init; } = false;
    }
```

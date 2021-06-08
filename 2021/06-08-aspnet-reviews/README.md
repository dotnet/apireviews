# API Review 06/08/2021

## Add HTTP/0.9 to Microsoft.AspNetCore.Http.HttpProtocol

**Approved** | [#aspnetcore/33280](https://github.com/dotnet/aspnetcore/pull/33280)

## Close connection 

**Approved** | [#aspnetcore/32096](https://github.com/dotnet/aspnetcore/pull/32096#issuecomment-856902967)

Let's make the default implementation non-public. Outside of that, API looks :shipit: 
## Use request culture to deserialize JSON body 

**Approved** | [#aspnetcore/31331](https://github.com/dotnet/aspnetcore/pull/31331)

## GRPC :scheme pseudo-header passed from proxy/loadbalancer causes ConnectionAbortedException

**Approved** | [#aspnetcore/30532](https://github.com/dotnet/aspnetcore/issues/30532#issuecomment-856917570)

We discussed putting this on the root Kestrel options at which point it would apply to HTTP/2 and HTTP/3. Most limits are currently numeric, so this seems like a slight outlier. On the other hand, it crowds the top-level option.

API feedback: Put this on the top level Kestrel option.

Additional discussion: Need to talk to @blowdart about enabling this option at all.

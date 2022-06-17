# API Review 06/17/2022

## Allow endpoint filter factories to manipulate endpoint metadata

**Approved** | [#aspnetcore/41722](https://github.com/dotnet/aspnetcore/issues/41722)

API review notes from @captainsafia :

[this proposal] makes sense given the other changes made around EndpointMetadata in the route groups PR.

## EndpointBuilder.ServiceProvider should be non-nullabe and renamed to ApplicationServices

**Approved** | [#aspnetcore/42137](https://github.com/dotnet/aspnetcore/issues/42137)

API review notes from @captainsafia:

[this proposal] is a sensible change that reflects our efforts to make use of IServiceProvider​ in contexts more consistent.


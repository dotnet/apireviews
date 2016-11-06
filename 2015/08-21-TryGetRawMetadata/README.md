# API Review 2015-08-21

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cdld1f77acl7eqj1nfkemdr3tqg).

### #2768 CoreCLR: TryGetRawMetadata

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/2768) |
[Video](https://plus.google.com/events/cdld1f77acl7eqj1nfkemdr3tqg)

* Which namespace?
    - `System.Reflection.Metadata` seems like a good candidate
* Which assembly?
    - Folks feel it logically belong to `System.Runtime.Loader`
    - We should lump it together with the requests for getting metadata tokens
      from reflection objects (e.g. `MethodInfo`)
    - Could imagine similar APIs, such as `TryGetPEImage`
* Roslyn will ship a preview of scripting for November
    - As long as we unblock them, they are fine with us moving the API around

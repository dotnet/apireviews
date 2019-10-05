# Quick Reviews 8/28/2018

## Add MemoryExtensions.Contains

**Approved** | [#corefx/27526](https://github.com/dotnet/corefx/issues/27526) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h0m0s)

## Trim/TrimStart/TrimEnd methods for Memory and Span with trim element

**Approved** | [#corefx/31011](https://github.com/dotnet/corefx/issues/31011#issuecomment-416678327) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h26m42s)

We can probably cut the APIs that don't specify a element (because `default(T)` doesn't seem reasonable) but it's still 6 APIs. We feel like we need a bit more justification to add these APIs.

@schungx what are the scenarios where you needed this API?
## Add span-based overloads of String.GetHashCode

**Approved** | [#corefx/31302](https://github.com/dotnet/corefx/issues/31302) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h38m48s)

## Need one step API for creating files on Windows with security

**Needs Work** | [#corefx/31488](https://github.com/dotnet/corefx/issues/31488#issuecomment-416680473) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h40m37s)

@JeremyKuhne 

What would it look like if were to add them back where they were? How bad would the dependency be? These APIs show up a lot in porting.
## File.Move(string,string, bool) overload

**Approved** | [#corefx/31511](https://github.com/dotnet/corefx/issues/31511#issuecomment-416681677) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h44m38s)

Makes sense. A few points:

* We should probably add corresponding APIs to `FileInfo` and `DirectoryInfo`.
* Can this be implemented on Mac/Linux/Unix?
## CustomAttributeData's AttributeType property should be virtual.

**Approved** | [#corefx/31614](https://github.com/dotnet/corefx/issues/31614) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h48m23s)

## a new GC API for large array allocation

**Approved** | [#corefx/31787](https://github.com/dotnet/corefx/issues/31787#issuecomment-416688698) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=0h50m56s)

* Should it just be `AllocateArray`? In the end, the developer controls the size.
* What happens if the developer specifies gen-0 but wants to create a 50 MB array? Will the API fail or will it silently promote the object to, say, gen-1?
* No clearing the memory is fine, but we want to make sure it shows up visibly on the call side (a plain `false` isn't good enough). We'd like this to be an overload, such a `AllocateLargeArrayUninitialized`? The other benefit of having an overload is that this could be constrained to only allow `T`s with no references (unmanaged constraint).
* Is LOH the same as MaxGeneration? If not, how can a developer explicitly allocate on the LOH?
## Expose the ability to create signature generic instance types.

**Approved** | [#corefx/31798](https://github.com/dotnet/corefx/issues/31798) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=1h9m36s)

## Expose additional MidpointRounding modes

**Approved** | [#corefx/31902](https://github.com/dotnet/corefx/issues/31902) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=1h15m58s)

## Expose some additional Math and MathF operations

**Approved** | [#corefx/31903](https://github.com/dotnet/corefx/issues/31903#issuecomment-416696207) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=1h19m36s)

Some minor feedback:

* NextValue -> BitIncrement
* PreviousValue -> BitDecrement
* Copysign -> CopySign
## Add string keyed dictionary ReadOnlySpan<char> lookup support

**Needs Work** | [#corefx/31942](https://github.com/dotnet/corefx/issues/31942#issuecomment-416704108) | [Video](https://www.youtube.com/watch?v=hLbCfJSPIaI&t=1h32m20s)

On the surface, the extension approach looks more appealing because it would allow the lookups to work against any instance of a dictionary (at least in principle, the current design only works if the dictionary was instantiated with a comparer that implements `IStringEqualityComparer`).

However, this also complicates the implementation and might have performance implications if we have to expose more methods that only make sense when `TKey` happens to be `string`. Also, one could argue that in the hot paths where people would be inclined to lookup with spans that they do in fact control their dictionary and whether they need to pass in the correct comparer or use a derived type of `Dictionary<,>` doesn't matter.

I totally buy the scenario, but adding a new collection type (even if derived from `Dictionary<,>`) feels heavy handed, but maybe I'm overthinking this. Maybe just putting it in `S.C.Specialized` is good enough.

What do others think?

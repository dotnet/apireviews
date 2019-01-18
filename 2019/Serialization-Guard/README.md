# Serialization Guard

Status: **Approved with feedback** | 
[API Ref](https://github.com/dotnet/corefx/issues/34555) |
[Video](https://www.youtube.com/watch?v=OGuxohgcZZU)

## Notes

* Do we need a public API at all? It seems:
    1. The majority of the usages would be from the framework itself; very few
       external customers would use it.
    2. Before we make it a public API, we might want to keep it private to
       validate the APIs with internal usage first.
    3. The API won't be in .NET Framework 4.8 (implementation might), so we have
       more time anyways.
* We should rename the exception type and derive from `SerializationException`
    - Code using binary serialization and data contract serialization is likely
      already handling this exception.
    - `DeserializationBlockedException` : `SerializationException`
* Should `StartDeserialization()` be blocked too?
    - The attack vector is deliberately calling and leaking the token, thus
      effectively disabling many types
    - We need to support recursive deserialization, so we can't prevent calling
      this API in general
    - Ideally we'd prevent direct calls via reflection, but we don't have that
      mechanism in .NET Core.
* `ThrowOnDangerousDeserialization` should be renamed to
  `ThrowIfDeserializationInProgress`
* We need to figure out how individual gadgets/targets would get granular
  checks. Presumably passing a `string`-based switch to
  `ThrowIfDeserializationInProgress` would be good enough.
* If `DeserializationToken` needs to be class, we should hide the type and just
  return `IDisposable`. In other words, the type would be useful to make this a
  struct.
* `DeserializationBlockedException` should probably follow the default patterns
  for constructors. The construction of the message with the switch name can be
  handled by the corresponding throw helper on `SerializationInfo`.

## Remaining work

* Provide throw helper with switch
* Agree on default policy (on/off)
* Agree on whether or not we make these APIs public in .NET Core 3.0
* We should distill a list with 3rd party serializers what would be good
  customers (i.e. are baking type information into the payload).
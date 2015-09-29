# API Review 2015-09-29

This API review was also recorded and is available on [Google+](https://plus.google.com/events/cp5uek0sfdpbp04m9usnvker2lk).

## Overview

In this API review we reviewed:

* [The backlog](#backlog-review)
* [Serialization in .NET Core](#serialization)

## Notes

### Backlog Review

Status: **Proposal Pending**

* We'll probably change our design review process as our current approach
  clearly doesn't scale. @terrajobst is on the hook to make a proposal.

### Serialization

Status: **Proposal Approved**

### General stance on serialization in .NET Core

* Serialization team asked whether we should add `ISerializable` into
  `System.Runtime`.
* Our stance on this topic is that we don't want to make serialization a
  technology that the core has to know about. In particular, we don't want to
  add types that require core framework types, such as exceptions and
  collections, having to implement serialization logic.
* The reason being that serialization technologies have a tendency to come and
  go and we think that tying the platform core to volatile technologies isn't a
  recipe for success.
* Of course, we want the .NET platform to be able to provide the right building
  blocks for serialization, which includes reflection and design time code
  generation.
* We believe the right model for serialization is make it so that consumers can
  register handlers for serialization/deserialization of types. Of course, it
  should come with pre-hooked handlers for common types such as primitives
  and collections, but it should be extensible for consumers too. This way,
  anybody can add code that makes any type serializable while it decouples
  the types and serialization technologies. This model is referred to
  as *surrogates* and it's already supported by some serializers, such as data
  contract serializer.

### Support for IDataContractSurrogate in .NET

* The current way to hook up surrogates for data contract serialization is via
  [IDataContractSurrogate](https://msdn.microsoft.com/en-us/library/system.runtime.serialization.idatacontractsurrogate(v=vs.110).aspx)
* Unfortunately, `IDataContractSurrogate` depends on CodeDOM which we consider
  legacy and is replaced by Roslyn
* The proposal is to add a new type, `ISerializationSurrogateProvider` that
  doesn't have the CodeDOM dependency but supports the scenario:

```C#
public interface ISerializationSurrogateProvider
{
    Type GetSurrogateType(Type type);
    object GetDeserializedObject(object obj, Type targetType);
    object GetObjectToSerialize(object obj, Type targetType);
}
```

**NOTE:** It would be great if we could make this type a generalized surrogate
provider across all serialization technologies, but it's not a requirement.
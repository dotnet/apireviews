# JSON Reference Handling

Status: **Needs work** | 
[Issue with API](https://github.com/dotnet/corefx/issues/41002)
[Spec](https://github.com/dotnet/runtime/blob/cc1ac66d0a0df6e9139431a7fa42636050ce9a6d/src/libraries/System.Text.Json/docs/ReferenceHandling_spec.md)
[Video](https://youtu.be/H9zrbztep4M?t=168)

## Notes

* We "cooked up" the `$id`, `$ref`, `$values` naming
    - It's borrowed from JSON.NET, so we can read what JSON.NET serialized and
      vice versa
    - But there is no official spec for how object graphs should be represented
      in JSON
* `Ignore` will only ignore cycles; if the objects are peers, i.e. appear in
  disjoint branches, they get serialized multiple times.
    - It seems `Ignore` is a problematic policy because it's hard to reason
      about.
    - We recommend not to support this policy. Customers can apply
      `[JsonIgnore]` to properties representing back-pointers/cycles or custom
      converters if they can't control the object.
* There is no way to combine serialized results from multiple serializations
  because it will result in conflicting keys.
    - So far, it seems nobody has any issues with that.
* Right now, there is no way in a custom converter to get access to the internal
  mapping from `$id` to object.
    - We recommend to wait until someone is asking for it.
* In JSON.NET, customers have requested being able to change the strings `$id` et. al.
    - Seems rare
    - We recommend to wait until someone is asking for it.
* We have to be careful with dictionaries where the keys contain `$id`
    - We should escape the `$`.
    - This ensures we can read everything that we can write and it imposes no
      restrictions on the keys customers can have.
    - Same applies to `JsonPropertyNameAttribute`
* We shouldn't emit `$id` properties when serializing structs (you're just
  wasting bandwidth). We shouldn't throw if we deserialize a struct with a `$id`
  property, but we should throw if we see `$ref` pointing to a struct.
* The ground rules make sense.
* We discussed the shape of the `ReferenceHandling` API
    - It seems an enum would be closer match to the current API
    - However, if the JSON team has high confidence that the API needs to be
      evolved into a sealed class in an upcoming release, a class is also
      acceptable.

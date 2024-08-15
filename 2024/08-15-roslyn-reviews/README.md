# API Review 08/15/2024

## RegisterHostOutputs

**Approved** | [#roslyn/74753](https://github.com/dotnet/roslyn/issues/74753#issuecomment-2292218955)

### API Review

* What if we just used the existing named step tracking infrastructure?
    * This is a generator testing component, not a strong guarantee of a contract.
    * Better to have an API that intentionally defines an output component, rather than exposing internal generator details
* Would this be related to supplemental file generation?
    * We don't think so. Those would be tied to semantics of being a file, and this should not be.
* By switching to `object`, we are saying that we won't be serializing anything here.
    * Yup. People want to use host outputs in VS are going to have to come talk to us
* Arguments to using a `ImmutableDictionary<string, object>`:
    * Making it appear to be key/value without it actually being a dictionary is a bit odd.
    * What would the ordering be if we did `ImmutableArray<(string, object)>`?
    * Making it a dictionary will match behavior around file hint names

**Conclusion**: API is approved. We will use an `ImmutableDictionary<string, object>` for the host outputs, rather than an array.

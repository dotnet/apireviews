# Quick Reviews 4/17/2018

## Expose `System.Runtime.CompilerServices.SkipLocalsInitAttribute`

**Approved** | [#corefx/29026](https://github.com/dotnet/corefx/issues/29026#issuecomment-382076991) | [Video](https://www.youtube.com/watch?v=8m4ijdz0CXw&t=0h0m0s)

Questions:

* Will there be a keyword in the language too or will developers use the attribute?
* Presumably people will be able to define the attribute in their own code and get the same behavior?
* Does it make sense to be applied to fields? Presumably no, because what matters is the constructor in case initializers are present?
* Do you plan to support override behavior, i.e. apply to assembly/module/type to turn on/off and then override on the a per member basis?

```csharp
namespace System.Runtime.CompilerServices
{
    [AttributeUsage(AttributeUsage.All, Inherited = false)]
    public sealed class SkipLocalsInitAttribute : Attribute
    {
        public SkipLocalsInitAttribute(bool isEnabled);
        public bool IsEnabled { get; }
    }
}
```
## Add a GetEncodings method to System.Text.EncodingProvider to support enumerating available character encodings

**Needs Work** | [#corefx/28944](https://github.com/dotnet/corefx/issues/28944#issuecomment-382081100) | [Video](https://www.youtube.com/watch?v=8m4ijdz0CXw&t=0h9m53s)

* `EncodingProvider.GetEncodings()` should be `IEnumerable<EncodingInfo>`
* `EncodingProvider.GetEncodings()` should be virtual and return an empty list (instead of throwing). While this isn't correct it means that one provider that isn't updated doesn't spoil the enumeration for everyone else.
* `Encoding.GetEncodings()` should return all registered encodings across all providers and de-dupe them if necessary.

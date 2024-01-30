# API Review 01/30/2024

## JsonSerializer: Allow out-of-order reading of metadata properties.

**Approved** | [#runtime/72604](https://github.com/dotnet/runtime/issues/72604#issuecomment-1917730728) | [Video](https://www.youtube.com/watch?v=O-aPOKb-1TM&t=0h0m0s)

Looks good as proposed.  The default may need to change from false to true in the future; but we'll let that be based on data/feedback.

```c#
namespace System.Text.Json
{
    public partial class JsonSerializerOptions
    {
        public bool AllowOutOfOrderMetadataProperties { get; set; } = false;
    }
}

namespace System.Text.Json.Serialization
{
    public partial class JsonSourceGenerationOptionsAttribute
    {
        public bool AllowOutOfOrderMetadataProperties { get; set; } = false;
    }
}
```
## Attribute model for feature APIs

**NeedsWork** | [#runtime/96859](https://github.com/dotnet/runtime/issues/96859#issuecomment-1917828539) | [Video](https://www.youtube.com/watch?v=O-aPOKb-1TM&t=0h16m40s)

* FeatureSwitchDefinition: FeatureName => SwitchName, to align with the terminology from AppContext
* Consider if FeatureCheck can be eliminated in terms of FeatureGuard (if FeatureCheck is intended for sourcegen then it has unique value).
* FeatureGuardAttribute.FeatureAttributeType => FeatureType
* We're concerned that the names are over-generalized but that we haven't proven out that it can really be used generally.  If it can't be applied to RequiresDynamicCode, the Intrinsics/ISA types, and some third technology, we need to revisit these names.

```c#
namespace System.Diagnostics.CodeAnalysis;

[AttributeUsage(AttributeTargets.Property, Inherited = false)]
public sealed class FeatureCheckAttribute : Attribute
{
    public Type FeatureType { get; }
    public FeatureCheckAttribute(Type featureType)
}

[AttributeUsage(AttributeTargets.Property, Inherited = false, AllowMultiple = true)]
public sealed class FeatureGuardAttribute : Attribute
{
    public Type FeatureType { get; }
    public FeatureGuardAttribute(Type featureType)
}

[AttributeUsage(AttributeTargets.Class, Inherited = false)]
public sealed class FeatureSwitchDefinitionAttribute : Attribute
{
    public string SwitchName { get; }
    public FeatureSwitchDefinitionAttribute(string switchName)
}
```

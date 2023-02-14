# API Review 02/14/2023

## Expand data validation attributes

**Approved** | [#runtime/77402](https://github.com/dotnet/runtime/issues/77402#issuecomment-1430306200) | [Video](https://www.youtube.com/watch?v=HtomCP23-TE&t=0h0m0s)

* `RangeAttribute` calls the current properties `Minimum` and `Maximum`, so those terms should be preferred over `UpperBound` and `LowerBound`
  * And we moved the "Is" to later in the name so that Minimum/MinimumIsExclusive are adjacent in IntelliSense and docs.
* `RequiredAttribute.DisallowDefaultValues` yielded a very long discussion, and the conclusion was to insert "All": `DisallowAllDefaultValues`.
* `Base64StringAttribute` looks good as proposed
* For `LengthAttribute` we removed the defaulted paramters, both are required (if you want just one or the other use the existing attributes)
  * We changed MinLength/MaxLength to be based on `long` instead of `int`
  * And renamed MinLength/MaxLength to MinimumLength/MaximumLength
* For `AllowedValuesAttribute` and `DeniedValuesAttribute` we renamed their properties to just be `Values`
* It was asked and answered that these types are intentionally not sealed, consistency with the other data validation attributes (and the notion that these work less like attributes and more like general runtime objects)

```C#
namespace System.ComponentModel.Annotations;

public partial class RangeAttribute : ValidationAttribute
{
    public bool MinimumIsExclusive { get; set; } = false;
    public bool MaximumIsExclusive { get; set; } = false;
}

public partial class RequiredAttribute : ValidationAttribute
{
     // Fail validation for structs that equal the default value for the type
     public bool DisallowAllDefaultValues { get; set; } = false;
}

// Validates that the specified string uses Base64 encoding
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter, AllowMultiple = false)]
public class Base64StringAttribute : ValidationAttribute
{
}

// Specifies length ranges for string/IEnumerable
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter, AllowMultiple = false)]
public class LengthAttribute : ValidationAttribute
{
    public LengthAttribute(long minimumLength, long maximumLength);

    public long MinimumLength { get; }
    public long MaximumLength { get; }
}

// Validation using allow and deny lists
[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter, AllowMultiple = false)]
public class AllowedValuesAttribute : ValidationAttribute
{
    public AllowedValuesAttribute(params object[] values);
    public object[] Values { get; }
}

[AttributeUsage(AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Parameter, AllowMultiple = false)]
public class DeniedValuesAttribute : ValidationAttribute
{
    public DeniedValuesAttribute(params object[] values);
    public object[] Values { get; }
}
```

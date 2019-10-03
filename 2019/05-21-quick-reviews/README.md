# Quick Reviews 5/21/2019

## Attributes for nullable annotations

**Approved** | [#corefx/37826](https://github.com/dotnet/corefx/issues/37826#issuecomment-494505569) | [Video](https://www.youtube.com/watch?v=zAhOxraBsi0&t=-10h-23m-4s)

Summary:

* We didn't want to put them in compiler services, because most APIs aren't meant to be used by developers.
* `NullableAttribute` (emitted by the compiler for question marks) will still go to compiler services.
* We decided to go with the existing `System.Diagnostics.CodeAnalysis` namespace
* Since the new namespace is low in terms of types, we decided that we don't need a base type

Here is the API shape we agreed on:

```C#
namespace System.Diagnostics.CodeAnalysis 
{
    [AttributeUsage(AttributeTargets.Field |
                    AttributeTargets.Parameter |
                    AttributeTargets.Property)]
    public sealed class AllowNullAttribute : Attribute
    {    	
    }

    [AttributeUsage(AttributeTargets.Field |
                    AttributeTargets.Parameter |
                    AttributeTargets.Property)]
    public sealed class DisallowNullAttribute : Attribute
    {
    }

    [AttributeUsage(AttributeTargets.Field |
                    AttributeTargets.Parameter |
                    AttributeTargets.Property |
                    AttributeTargets.ReturnValue)]
    public sealed class MaybeNullAttribute : Attribute
    {
    }

    [AttributeUsage(AttributeTargets.Field |
                    AttributeTargets.Parameter |
                    AttributeTargets.Property |
                    AttributeTargets.ReturnValue)]
    public sealed class NotNullAttribute : Attribute
    {    	
    }

    [AttributeUsage(AttributeTargets.Parameter)]
    public sealed class MaybeNullWhenAttribute : Attribute
    {
        public MaybeNullWhenAttribute(bool returnValue);
        public bool ReturnValue { get; }
    }

    [AttributeUsage(AttributeTargets.Parameter)]
    public sealed class NotNullWhenAttribute : Attribute
    {
        public NotNullWhenAttribute(bool returnValue);
        public bool ReturnValue { get; }
    }

    [AttributeUsage(AttributeTargets.Parameter |
                    AttributeTargets.Property |
                    AttributeTargets.ReturnValue, AllowMultiple = true)]
    public sealed class NotNullIfNotNullAttribute : Attribute
    {
        public NotNullIfNotNullAttribute(string parameterName);
        public string ParameterName { get; }
    }

    [AttributeUsage(AttributeTargets.Method | AttributeTargets.Constructor)]
    public sealed class DoesNotReturnAttribute : Attribute
    {
    }

    [AttributeUsage(AttributeTargets.Parameter)]
    public sealed class DoesNotReturnIfAttribute : Attribute
    {
        public DoesNotReturnIfAttribute(bool parameterValue);
        public bool ParameterValue { get; }
    }
}
```

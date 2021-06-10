# API Review 06/10/2021

## Minor cleanups for System.Runtime.Numerics

**Approved** | [#runtime/53984](https://github.com/dotnet/runtime/pull/53984#issuecomment-858849057) | [Video](https://www.youtube.com/watch?v=YRcGjsFbx4Y&t=0h0m0s)

Thanks for the cleanup here!
## Add ability to detect if a service is registered in the DI container

**Approved** | [#runtime/53919](https://github.com/dotnet/runtime/issues/53919#issuecomment-858861117) | [Video](https://www.youtube.com/watch?v=YRcGjsFbx4Y&t=0h55m13s)


* The existing ISupportRequiredService is used with an as-cast from IServiceProvider, so this shouldn't start with ISupport if it's used in a different way.
  * Either a different naming pattern, or change the usage to be as-based instead of GetService-based.
  * Last minute proposal was "IServiceProvider[PrimaryMethodName]" for this new kind of thing.

```C#
namespace Microsoft.Extensions.DependencyInjection
{
     public interface IServiceProviderIsService
     {
         bool IsService(Type serviceType);
     }
}
```
## Expose top-level nullability information from reflection

**Approved** | [#runtime/29723](https://github.com/dotnet/runtime/issues/29723#issuecomment-858930248) | [Video](https://www.youtube.com/watch?v=YRcGjsFbx4Y&t=1h7m7s)


* [MaybeNullWhen] should count as a "maybe null" return answer.
* We changed NullabilityInfo.GenericTypeArguments to a non-nullable array to match standard reflection API practices.
  * We discussed the caching strategy, and since NullabilityInfo objects are expected to be freshly returned each time there's no parallel-caller mutation concern.
* We renamed the MaybeNull member to Nullable
* Once we renamed the member the type name for the enum seemed wrong, so we renamed that to NullabilityState.

```C#
namespace System.Reflection
{
    public sealed class NullabilityInfoContext
    {
        public NullabilityInfo Create(ParameterInfo parameterInfo);
        public NullabilityInfo Create(PropertyInfo propertyInfo);
        public NullabilityInfo Create(EventInfo eventInfo);
        public NullabilityInfo Create(FieldInfo parameterInfo);
    }

    public sealed class NullabilityInfo
    {
        public Type Type { get; }
        public NullableState ReadState { get; }
        public NullableState WriteState { get; }
        public NullabilityInfo? ElementType { get; }
        public NullabilityInfo[] GenericTypeArguments { get; }
    }

    public enum NullabilityState
    {
        Unknown,
        NotNull,
        Nullable
    }
}
```

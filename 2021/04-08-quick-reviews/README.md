# Quick Reviews 04/08/2021

## Compile-time source generation for System.Text.Json

**Approved** | [#runtime/45448](https://github.com/dotnet/runtime/issues/45448#issuecomment-816280665) | [Video](https://www.youtube.com/watch?v=YnMtcUwzFq4&t=0h0m0s)

* Consider changing JsonSerializerOptions.SetContext to JsonSerializerOptions.AddContext
  * Even if it would throw on a second call now, it allows either a stack or list-based implementation in the future, to combine contexts across assemblies.
* Consider adding JsonSerializerContext.CreateTypeInfoViaReflection to enable dynamic scenarios in the context-preferred world.
```C#
partial class JsonSerializerContext
{
    public static JsonTypeInfo<T> CreateTypeInfoViaReflection<T>(JsonSerializerOptions options);
    public static JsonTypeInfo CreateTypeInfoViaReflection(Type type, JsonSerializerOptions options);
}
```
* JsonSerializerContext.GetTypeInfo and JsonSerializerContext.GetTypeInfo<T> should both be abstract
* Remove the default ctor on JsonSerializerContext and hide the null options state
```C#
partial class JsonSerializerContext
{
    // This can use lazy initialization, or other techniques, to make AddContext work without creating an options.
    public JsonSerializerOptions Options { get; }

    protected JsonSerializerContext(JsonSerializerOptions? options = null) { }
}
```
* Make JsonTypeInfo.Converter a non-public property, if possible
* It feels like JsonTypeInfo should be abstract if JsonTypeInfo<T> is.  But it's not important since both types have no public ctors.
* JsonSerializableAttribute.TypeInfoPropertyName should be declared nullable
* Move JsonSerializableAttribute to System.Text.Json.Serialization
* Collapse System.Text.Json.Serialization.Metadata.Internal into System.Text.Json.Serialization.Metadata
* Rename MetadataServices to JsonMetadataServices
* We discussed if it would make sense to replace the JsonNumberHandling parameter to InitializeObject<T> be something to represent all possible type-specific attributes, but decided that could be deferred until we had a second thing
* We discussed if the JsonNumberHandling parameter to InitializeObject<T> should be nullable, and decided it didn't need to because the generator can already distinguish between using an attribute-specified value or options.DefaultNumberHandling
* Rename CreatePropertyInfo's clrPropertyName to propertyName, declaredPropertyName, runtimePropertyName, or memberName.
* Move all the members on ConverterProvider to JsonMetadataServices, adding a "Converter" suffix to each property.
* Are we missing scenarios/overloads for the System.Net.Http.Json members that don't currently accept a JsonSerializerOptions?
* We're missing IList/IDictionary versions of JsonMetadataServices.Create* members.

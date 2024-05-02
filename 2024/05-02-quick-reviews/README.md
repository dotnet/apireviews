# API Review 05/02/2024

## Extend BlobBuilder so consumers can better control allocations

**Approved** | [#runtime/100418](https://github.com/dotnet/runtime/issues/100418#issuecomment-2091243298) | [Video](https://www.youtube.com/watch?v=oQERLVomgvE&t=0h0m0s)


* `WriteBytes` should have been public (typo in the proposal)
* There was a question as to whether `MaxChunkSize` should be a ctor-set field. It got moved to a ctor parameter only for the byte[]-accepting protected ctor
* ManagedPEBuilder.CreateBlobBulder changed from `int?=null` to `int=0` as zero has no alternative meaning
* ManagedPEBuilder's new ctor should have all the parameters defaulted, and should "un-default" and hide the previous 4 argument ctor.
* `BeforeSwap` => `OnLinking`

```c#
public partial class BlobBuilder
{
     protected byte[] Buffer { get; set; }
     public int Capacity { get; set; }

     protected BlobBuilder(byte[] buffer, int maxChunkSize = 0);

     protected virtual void OnLinking(BlobBuilder other);
     protected virtual void SetCapacity(int capacity);

     public void WriteBytes(ReadOnlySpan<byte> buffer);
}

public partial class DebugDirectoryBuilder
{
     public DebugDirectoryBuilder(BlobBuilder blobBuilder);
}

public partial class ManagedPEBuilder
{
     protected virtual BlobBuilder CreateBlobBuilder(int minimumSize = 0);
}
```

```diff
public partial class MetadataBuilder
{
+    [EditorBrowsable(EditorBrowsableState.Never)]
     public MetadataBuilder(
-        int userStringHeapStartOffset = 0,
+        int userStringHeapStartOffset,
-        int stringHeapStartOffset = 0,
+        int stringHeapStartOffset,
-        int blobHeapStartOffset = 0,
+        int blobHeapStartOffset,
-        int guidHeapStartOffset = 0);
+        int guidHeapStartOffset);

+    public MetadataBuilder(
+        int userStringHeapStartOffset = 0,
+        int stringHeapStartOffset = 0,
+        int blobHeapStartOffset = 0,
+        int guidHeapStartOffset = 0,
+        Func<int, BlobBuilder>? createBlobBuilderFunc = null);
}

## TypeDescriptor-related trimming support

**Approved** | [#runtime/101202](https://github.com/dotnet/runtime/issues/101202#issuecomment-2091404734) | [Video](https://www.youtube.com/watch?v=oQERLVomgvE&t=1h24m56s)


* We discussed the implications of using DIMs in ICustomTypeDescriptor, and it seems OK.
* TypeDescriptor.GetPropertiesFromRegisteredType (and others) has other overloads, they're intentionally omitted.
* The DynamicallyAccessedMemberTypes value on RegisterType is going to be something less than All, will be corrected during implementation.
* `TypeDescriptorProvider.RegisterType<T>` doesn't seem to need the DynamicallyAccessedMembers attribute, the use is covered by the static wrapper.

```c#
using System.ComponentModel;

public sealed partial class TypeDescriptor
{
    // The main registration API that will cause the linker\trimmer to preserve metadata\members for reflection.
    // Instead of .All, we may use more narrow values (public properties, fields, events, members, constructors)
    public static void RegisterType<[DynamicallyAccessedMembers(DynamicallyAccessedMemberTypes.All)] T>();

    public static AttributeCollection GetAttributesFromRegisteredType(Type componentType);
    public static TypeConverter? GetConverterFromRegisteredType(object component);
    public static TypeConverter? GetConverterFromRegisteredType(Type type);
    public static EventDescriptorCollection GetEventsFromRegisteredType(Type componentType);
    public static PropertyDescriptorCollection GetPropertiesFromRegisteredType(Type componentType);
    public static PropertyDescriptorCollection GetPropertiesFromRegisteredType(object component);
}

// Uses Default Interface Methods to add new members.
// Alternative approach is to add 'ICustomTypeDescriptorForRegisteredType : ICustomTypeDescriptor'
public partial interface ICustomTypeDescriptor
{
    AttributeCollection GetAttributesFromRegisteredType() => DIM;
    TypeConverter? GetConverterFromRegisteredType() => DIM;
    EventDescriptorCollection GetEventsFromRegisteredType() => DIM;
    PropertyDescriptorCollection GetPropertiesFromRegisteredType() => DIM;

    // Defaults to 'null' which then the framework defers to the new feature switch as to validate or not.
    public virtual bool? RequireRegisteredTypes => null;

    // The overloads that take attributes were purposely omitted for now.
}

public abstract class CustomTypeDescriptor : ICustomTypeDescriptor
{
    public virtual AttributeCollection GetAttributesFromRegisteredType();
    public virtual TypeConverter? GetConverterFromRegisteredType();
    public virtual EventDescriptorCollection GetEventsFromRegisteredType();
    public virtual PropertyDescriptorCollection GetPropertiesFromRegisteredType();

    // Defaults to 'null' which then the framework defers to possible new feature switch as to validate or not.
    public virtual bool? RequireRegisteredTypes => null;
}

public abstract class PropertyDescriptor : MemberDescriptor
{
    public virtual TypeConverter ConverterFromRegisteredType { get; }
}

public abstract class TypeDescriptionProvider
{
    public virtual void RegisterType<T>();
    public virtual ICustomTypeDescriptor GetExtendedTypeDescriptorFromRegisteredType(object instance);

    public ICustomTypeDescriptor? GetTypeDescriptorFromRegisteredType(object instance);

    public virtual bool IsRegisteredType(System.Type type);
    public virtual bool? RequireRegisteredTypes => null;
}

public class ComponentResourceManager
{
    public virtual void ApplyResourceToRegisteredType(object value, string objectName, CultureInfo? culture);
}
```

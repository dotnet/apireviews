# API Review 05/24/2022

## System.Runtime.CompilerServices.MetadataUpdateOriginalTypeAttribute

**Approved** | [#runtime/66222](https://github.com/dotnet/runtime/issues/66222#issuecomment-1136248335) | [Video](https://www.youtube.com/watch?v=tupBJ957KfI&t=0h0m0s)

* Should we add other types (interface, delegate?)
* We should add a property for consistency
* We should use `System.Type` instead of `System.String`
    - This might be desirable if we reference previous versions or only the root. We believe we'd only reference the root. 
    - This also gets rid of all the ambiguities on the actual string format
    - Should be renamed to `MetadataUpdateOriginalTypeAttribute` then

```C#
namespace System.Runtime.CompilerServices;

[AttributeUsage(System.AttributeTargets.Class | System.AttributeTargets.Struct,
                AllowMultiple=false, Inherited=false)]
public class MetadataUpdateOriginalTypeAttribute : Attribute
{
    public MetadataUpdateOriginalTypeAttribute(Type originalType);
    public Type OriginalType { get; }
}
```
## Developers can use System.Text.Json to serialize type hierarchies securely

**Approved** | [#runtime/63747](https://github.com/dotnet/runtime/issues/63747#issuecomment-1136302184) | [Video](https://www.youtube.com/watch?v=tupBJ957KfI&t=0h31m47s)

* Let's drop the `Id` suffix of `TypeDiscriminator`

```C#
namespace System.Text.Json.Serialization;

[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Interface,
                AllowMultiple = false,
                Inherited = false)]
public sealed class JsonPolymorphicAttribute : JsonAttribute
{
    public string? TypeDiscriminatorPropertyName { get; set; }
    public bool IgnoreUnrecognizedTypeDiscriminators { get; set; }
    public JsonUnknownDerivedTypeHandling UnknownDerivedTypeHandling { get; set; }
}

[AttributeUsage(AttributeTargets.Class |
                AttributeTargets.Interface,
                AllowMultiple = true,
                Inherited = false)]
public class JsonDerivedTypeAttribute : JsonAttribute
{
    public JsonDerivedTypeAttribute(Type derivedType);
    public JsonDerivedTypeAttribute(Type derivedType, string typeDiscriminator);
    public JsonDerivedTypeAttribute(Type derivedType, int typeDiscriminator);

    public Type DerivedType { get; }
    public object? TypeDiscriminator { get; }
}

public enum JsonUnknownDerivedTypeHandling
{
    FailSerialization = 0,
    FallBackToBaseType = 1,
    FallBackToNearestAncestor = 2
}
```

```C#
namespace System.Text.Json.Metadata;

public partial class JsonTypeInfo
{
    public JsonPolymorphismOptions? PolymorphismOptions { get; set; }
}

public class JsonPolymorphismOptions
{
    public JsonPolymorphismOptions();

    public ICollection<JsonDerivedType> DerivedTypes { get; }

    public bool IgnoreUnrecognizedTypeDiscriminators { get; set; }
    public JsonUnknownDerivedTypeHandling UnknownDerivedTypeHandling { get; set; }
    public string TypeDiscriminatorPropertyName { get; set; }
}

public struct JsonDerivedType
{
    public JsonDerivedType(Type derivedType);
    public JsonDerivedType(Type derivedType, int typeDiscriminator);
    public JsonDerivedType(Type derivedType, string typeDiscriminator);

    public Type DerivedType { get; }
    public object? TypeDiscriminator { get; }
}
```

## APIs for working with unix file mode

**Approved** | [#runtime/67837](https://github.com/dotnet/runtime/issues/67837#issuecomment-1136368812) | [Video](https://www.youtube.com/watch?v=tupBJ957KfI&t=1h29m56s)

* Let's remove `Directory.OpenHandle`
    - We don't have an `OpenHandle` on Directory today
* `File`
    - Let's add overloads for `string path`
* Let's also remove `File.OpenHandle`
    - We don't have an overload today that allows setting Windows ACLs, it's a low level API
    - This would also have implications for if we use the 
* None of the APIs will be marked as Linux only as it seems we can and should make them work on Windows by setting WSL related metadata in NTFS
    - See https://docs.microsoft.com/en-us/windows/wsl/file-permissions
* `UnixFileMode`
    - Let's remove the mask because it messes with `ToString()`

```C#
namespace System.IO;

[System.FlagsAttribute]
public enum UnixFileMode
{
    None = 0,
    OtherExecute = 1,
    OtherWrite = 2,
    OtherRead = 4,
    GroupExecute = 8,
    GroupWrite = 16,
    GroupRead = 32,
    UserExecute = 64,
    UserWrite = 128,
    UserRead = 256,
    StickyBit = 512,
    SetGroup = 1024,
    SetUser = 2048
}
public partial class Directory
{
    public static DirectoryInfo CreateDirectory(string path, UnixFileMode unixCreateMode);
}
public partial class File
{
    public static UnixFileMode GetUnixFileMode(string path);
    public static UnixFileMode GetUnixFileMode(SafeFileHandle fileHandle);
    public static void SetUnixFileMode(string path, UnixFileMode mode);
    public static void SetUnixFileMode(SafeFileHandle fileHandle, UnixFileMode mode);
}
public partial class FileSystemInfo
{
    public UnixFileMode UnixFileMode { get; set; }
}
public partial class FileStreamOptions
{
    public UnixFileMode? UnixCreateMode { get; set; }
}
```

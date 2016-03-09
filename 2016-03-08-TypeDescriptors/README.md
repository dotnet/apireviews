# API Review 2016-03-09

This API review was also recorded and is available on [Google Hangouts](https://plus.google.com/events/cap2r7lt8a51l5juutmet82er58).

## Add TypeDescriptor to .NET Core

Status: **Approved with feedback** |
[API Ref](System.ComponentModel.TypeConverter.md) |
[Issue](https://github.com/dotnet/corefx/issues/6573) |
[Video](https://plus.google.com/events/cap2r7lt8a51l5juutmet82er58)

* In the meeting, we looked at a minimal version and a long term version of
  `System.ComponentModel.TypeConverter`. We decided that it's best to not split
  that work. Piece meal additions are expensive and not worth it; we should
  simply start with the long term version, remove APIs we certainly don't want
  and only further trim if implementation is costly or pulls in more dependencies
  we don't want.
* [API Ref](System.ComponentModel.TypeConverter.md) represents the longer term
  version. Suggested tweaks are below.
* Although `CustomTypeDescriptor.GetEditor(Type)` implies UI we should add it
  because we can't remove `ICustomTypeDescriptor.GetEditor(Type)`. The implementation
  should match desktop which basically says: `return _parent?.GetEditor(type)`
* **Do** Expose
    - All `static readonly fields` that have pre-constructed attributes -- the designer relies on reference equality
    - `PropertyDescriptor.SerializationVisibility`
    - `TypeDescriptor.AddEditorTable`
    - `TypeDescriptor.GetEditor`
    - `BrowsableAttribute`
    - `CategoryAttribute`
    - `DesignerSerializationVisibilityAttribute`
* Move all attributes that are independent of type converters/descriptors to `System.ComponentModel.Primitives`
    - `Default(Event/Property)Attribute` should be in `System.ComponentModel.TypeConverter`
* **Do not** expose
    -  `IComNativeDescriptonHandler` / `TypeDescriptor.ComObjectType`
    -  `TypeDescriptor.CreateDesigner`
    -  `DataObjectFieldAttribute`
    -  `INestedSite`
    -  `Inheritance*`
    -  `IRaiseItemChangeEventArgs`
    -  `ListChanged*`
    -  `ListSort*`
    -  `WarningException`

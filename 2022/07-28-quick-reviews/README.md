# API Review 07/28/2022

## DataContract API's to enable external Schema Import/Export package

**Approved** | [#runtime/72242](https://github.com/dotnet/runtime/issues/72242#issuecomment-1198498213) | [Video](https://www.youtube.com/watch?v=l1ippVkiRxM&t=0h0m0s)

* Since these types aren't expected to be used by callers, other than tooling, let's avoid a popular namespace.
  * Moved to System.Runtime.Serialization.Schema, except the ISerializationSurrogateProvider2 interface.
* DataContractSet.ImportSchemaSet: Be `List<T>` instead of `IList<T>` if the implementation always returns that concrete type.
  * Or `ReadOnlyCollection<T>` if the caller isn't supposed to mutate the data.
* DataContractSet.ImportSchemaSet's `elements` parameter should be `IEnumerable<T>` if possible.
* DataContractSet's "surrogate" ctor should take the inputs as `IEnumerable<T>` instead of `ICollection<T>` if possible.
* DataContract.KnownDataContracts should not be settable, if possible
* DataContract.StableName doesn't feel like an obvious name (or a name consistent with existing tech).  `XmlName` seems to be a better fit (or `XmlQualifiedName` if you prefer)
  * DataContract.GetStableName should match.
* DataContract.EncodeLocalName doesn't feel like it lives on the right type.  Consider removing.
* DataContract.IsKeyValue => DataContract.IsKeyedCollection or DataContract.IsDictionary or DataContract.IsDictionaryLike
  * Check the nullable annotations on this member, e.g. can all three gain a [NotNullWhen]?
* Is `Collection<Type>` the right type for ISerializationExtendedSurrogateProvider.GetKnownCustomDataTypes?
  * ICollection, IEnumerable, etc.
* ISerializationExtendedSurrogateProvider.GetCustomDataToExport's `clrType` should be `runtimeType` instead.
* ISerializationExtendedSurrogateProvider => ISerializationSurrogateProvider2
* DataContractJsonSerializerExtensions: These members should go on DataContractJsonSerializer directly
* If you want to copy (NOT move) the DataContractSerializerExtension extension methods to DataContractSerializer, go ahead.

```C#
namespace System.Runtime.Serialization.Schema
{
    public sealed class DataContractSet
    {
        [RequiresUnreferencedCode("")]
        public DataContractSet(DataContractSet dataContractSet);

        public DataContractSet(
            ISerializationSurrogateProvider? dataContractSurrogate,
            ICollection<Type>? referencedTypes,
            ICollection<Type>? referencedCollectionTypes);

        public Dictionary<XmlQualifiedName, DataContract> Contracts { get; }
        public Dictionary<XmlQualifiedName, DataContract>? KnownTypesForObject { get; }
        public Dictionary<DataContract, object> ProcessedContracts { get; }
        public Hashtable SurrogateData { get; }

        [RequiresUnreferencedCode("")]
        public void Add(Type type);
        [RequiresUnreferencedCode("")]
        public void ExportSchemaSet(XmlSchemaSet schemaSet);
        [RequiresUnreferencedCode("")]
        public DataContract GetDataContract(Type type);
        [RequiresUnreferencedCode("")]
        public DataContract? GetDataContract(XmlQualifiedName key);
        [RequiresUnreferencedCode("")]
        public Type? GetReferencedType(
            XmlQualifiedName stableName,
            DataContract dataContract,
            out DataContract? referencedContract,
            out object[]? genericParameters,
            bool? supportGenericTypes = null);
        [RequiresUnreferencedCode("")]
        public void ImportSchemaSet(
            XmlSchemaSet schemaSet,
            ICollection<XmlQualifiedName>? typeNames,
            bool importXmlDataType);
        [RequiresUnreferencedCode("")]
        public IList<XmlQualifiedName> ImportSchemaSet(
            XmlSchemaSet schemaSet,
            ICollection<XmlSchemaElement> elements,
            bool importXmlDataType);
    }

    public abstract class DataContract
    {
        public virtual bool IsBuiltInDataContract { get; }
        [DynamicallyAccessedMembers(*)]
        public virtual Type UnderlyingType { get; }
        public virtual XmlQualifiedName XmlName { get; }
        public virtual Type OriginalUnderlyingType { get; }
        public virtual List<DataMember>? Members { get; }
        public virtual Dictionary<XmlQualifiedName, DataContract>? KnownDataContracts { get; set; }
        public virtual bool IsValueType { get; }
        public virtual bool IsReference { get; }
        public virtual bool IsISerializable { get; }
        public virtual XmlDictionaryString? TopLevelElementName { get; }
        public virtual XmlDictionaryString? TopLevelElementNamespace { get; }
        public virtual DataContract? BaseContract { get; }
        public virtual string? ContractType { get; }

        public static string EncodeLocalName(string localName);
        [RequiresUnreferencedCode("")]
        public static DataContract? GetBuiltInDataContract(string name, string ns);
        [RequiresUnreferencedCode("")]
        public static DataContract GetDataContract(Type type);
        [RequiresUnreferencedCode("")]
        public static XmlQualifiedName GetXmlName(Type type);
        [RequiresUnreferencedCode("")]
        public static Type GetSurrogateType(ISerializationSurrogateProvider surrogateProvider, Type type);
        [RequiresUnreferencedCode("")]
        public static bool IsTypeSerializable(Type type);
        [RequiresUnreferencedCode("")]
        public virtual XmlQualifiedName GetArrayTypeName(bool isNullable);
        public virtual bool IsDictionaryLike(out string? keyName, out string? valueName, out string? itemName);
    }

    public sealed class XmlDataContract : DataContract
    {
        public bool HasRoot { get; }
        public bool IsAnonymous { get; }
        public bool IsTopLevelElementNullable { get; }
        public bool IsTypeDefinedOnImport { get; set; }
        public bool IsValueType { get; set; }
        public XmlSchemaType? XsdType { get; }
    }

    public sealed class DataMember
    {
        public bool EmitDefaultValue { get; }
        public bool IsNullable { get; }
        public bool IsRequired { get; }
        public DataContract MemberTypeContract { get; }
        public string Name { get; }
        public long Order { get; }
    }
}

namespace System.Runtime.Serialization
{
    public interface ISerializationSurrogateProvider2 : ISerializationSurrogateProvider
    {
        object? GetCustomDataToExport(MemberInfo memberInfo, Type dataContractType);
        object? GetCustomDataToExport(Type runtimeType, Type dataContractType);
        void GetKnownCustomDataTypes(Collection<Type> customDataTypes);
        Type? GetReferencedTypeOnImport(string typeName, string typeNamespace, object? customData);
    }
}

namespace System.Runtime.Serialization.Json
{
    public partial class DataContractJsonSerializer
    {
        public System.Runtime.Serialization.ISerializationSurrogateProvider? GetSerializationSurrogateProvider();
        public void SetSerializationSurrogateProvider(System.Runtime.Serialization.ISerializationSurrogateProvider? provider);
    }
}
```
## XSD DCS Schema API's from 4.8

**Approved** | [#runtime/72243](https://github.com/dotnet/runtime/issues/72243#issuecomment-1198527093) | [Video](https://www.youtube.com/watch?v=l1ippVkiRxM&t=1h26m18s)

* ImportOptions.ProcessImportedType feels like it needs to be an ISerializationCodeDomSurrogateProvider to provide parity with the object flow from .NET Framework
* ExportOptions shouldn't be redefined in System.Runtime.Serialization.Schema, it just needs to merge with the one already present in System.Runtime.Serialization.
  * Similarly for the concrete exporter types

```C#
namespace System.Runtime.Serialization
{
    public partial class ExportOptions
    {
        public ISerializationSurrogateProvider? SurrogateProvider { get; set; }
    }

    // This ImportOptions API is almost exactly the same as the ImportOptions API in 4.8, with the exception of the surrogate
    // type of course - since that changed between NetFx and Core - and one extra Func that used to be part of the surrogate
    // provider interface in 4.8, but was pulled out into this options class because it uses CodeDom types.
    // https://docs.microsoft.com/en-us/dotnet/api/system.runtime.serialization.importoptions?view=netframework-4.8
    public class ImportOptions
    {
        public System.CodeDom.Compiler.CodeDomProvider? CodeProvider { get; set; }
        public bool EnableDataBinding { get; set; }
        public bool GenerateInternal { get; set; }
        public bool GenerateSerializable { get; set; }
        public bool ImportXmlType { get; set; }
        public IDictionary<string, string> Namespaces { get; }
        public ICollection<Type> ReferencedCollectionTypes { get; }
        public ICollection<Type> ReferencedTypes { get; }
        public ISerializationSurrogateProvider? DataContractSurrogate { get; set; }
    }

    public class XsdDataContractImporter
    {
        public XsdDataContractImporter() { }
        public XsdDataContractImporter(System.CodeDom.CodeCompileUnit codeCompileUnit) { }

        public System.CodeDom.CodeCompileUnit CodeCompileUnit { get; }
        public ImportOptions? Options { get; set; }

        [RequiresUnreferencedCodeAttribute()]
        public bool CanImport(System.Xml.Schema.XmlSchemaSet schemas) { }
        [RequiresUnreferencedCodeAttribute()]
        public bool CanImport(System.Xml.Schema.XmlSchemaSet schemas, ICollection<System.Xml.XmlQualifiedName> typeNames) { }
        [RequiresUnreferencedCodeAttribute()]
        public bool CanImport(System.Xml.Schema.XmlSchemaSet schemas, System.Xml.XmlQualifiedName typeName) { }
        [RequiresUnreferencedCodeAttribute()]
        public bool CanImport(System.Xml.Schema.XmlSchemaSet schemas, System.Xml.Schema.XmlSchemaElement element) { }

        [RequiresUnreferencedCodeAttribute()]
        public void Import(System.Xml.Schema.XmlSchemaSet schemas) { }
        [RequiresUnreferencedCodeAttribute()]
        public void Import(System.Xml.Schema.XmlSchemaSet schemas, ICollection<System.Xml.XmlQualifiedName> typeNames) { 
        [RequiresUnreferencedCodeAttribute()]
        public void Import(System.Xml.Schema.XmlSchemaSet schemas, System.Xml.XmlQualifiedName typeName) { }
        [RequiresUnreferencedCodeAttribute()]
        public System.Xml.XmlQualifiedName? Import(System.Xml.Schema.XmlSchemaSet schemas, System.Xml.Schema.XmlSchemaElement element) { }

        [RequiresUnreferencedCodeAttribute()]
        public System.CodeDom.CodeTypeReference GetCodeTypeReference(System.Xml.XmlQualifiedName typeName) { }
        [RequiresUnreferencedCodeAttribute()]
        public System.CodeDom.CodeTypeReference GetCodeTypeReference(System.Xml.XmlQualifiedName typeName, System.Xml.Schema.XmlSchemaElement element) { }
        [RequiresUnreferencedCodeAttribute()]
        public ICollection<System.CodeDom.CodeTypeReference>? GetKnownTypeReferences(System.Xml.XmlQualifiedName typeName) { }
    }

    public interface ISerializationCodeDomSurrogateProvider
    {
        CodeTypeDeclaration ProcessImportedType(CodeTypeDeclaration typeDeclaration, CodeCompileUnit compileUnit);
    }
}
```

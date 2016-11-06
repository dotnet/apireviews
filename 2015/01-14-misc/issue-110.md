```diff
---P:\oss\corefx\bin\_System.Xml.XDocument.old\System.Xml.XDocument.dll
+++P:\oss\corefx\bin\_System.Xml.XDocument.new\System.Xml.XDocument.dll
 namespace System.Xml.Linq {
  public class XDocument : XContainer {
    public XDocument();
    public XDocument(params object[] content);
    public XDocument(XDeclaration declaration, params object[] content);
    public XDocument(XDocument other);
    public XDeclaration Declaration { get; set; }
    public XDocumentType DocumentType { get; }
    public override XmlNodeType NodeType { get; }
    public XElement Root { get; }
    public static XDocument Load(Stream stream);
    public static XDocument Load(Stream stream, LoadOptions options);
    public static XDocument Load(TextReader textReader);
    public static XDocument Load(TextReader textReader, LoadOptions options);
    public static XDocument Load(string uri);
    public static XDocument Load(string uri, LoadOptions options);
    public static XDocument Load(XmlReader reader);
    public static XDocument Load(XmlReader reader, LoadOptions options);
+   public static Task<XDocument> LoadAsync(Stream stream, LoadOptions options, CancellationToken token);
    ^ Immo Landwerth: We'd either like to have overloads without them or make them optional. Right now, we
    | Immo Landwerth: don't have a good versioning plan for optionals, but it seems there might one coming.
    | Immo Landwerth: For now, don't add overloads nor mark them optional.
+   public static Task<XDocument> LoadAsync(TextReader textReader, LoadOptions options, CancellationToken token);
+   public static Task<XDocument> LoadAsync(string uri, LoadOptions options, CancellationToken token);
    ^ Immo Landwerth: We don't like Load(Uri) due to behavioral inconsistencies. If we plan on obsoleting it,
    | Immo Landwerth: we shouldn't add LoadAsync(Uri)
+   public static Task<XDocument> LoadAsync(XmlReader reader, LoadOptions options, CancellationToken token);
    public static XDocument Parse(string text);
    public static XDocument Parse(string text, LoadOptions options);
    public void Save(Stream stream);
    ^ Immo Landwerth: We should add SaveAsync overloads as well.
    public void Save(Stream stream, SaveOptions options);
    public void Save(TextWriter textWriter);
    public void Save(TextWriter textWriter, SaveOptions options);
    public void Save(XmlWriter writer);
    public override void WriteTo(XmlWriter writer);
  }
  public class XElement : XContainer {
    public XElement(XElement other);
    public XElement(XName name);
    public XElement(XName name, object content);
    public XElement(XName name, params object[] content);
    public XElement(XStreamingElement other);
    public static IEnumerable<XElement> EmptySequence { get; }
    public XAttribute FirstAttribute { get; }
    public bool HasAttributes { get; }
    public bool HasElements { get; }
    public bool IsEmpty { get; }
    public XAttribute LastAttribute { get; }
    public XName Name { get; set; }
    public override XmlNodeType NodeType { get; }
    public string Value { get; set; }
    public IEnumerable<XElement> AncestorsAndSelf();
    public IEnumerable<XElement> AncestorsAndSelf(XName name);
    public XAttribute Attribute(XName name);
    public IEnumerable<XAttribute> Attributes();
    public IEnumerable<XAttribute> Attributes(XName name);
    public IEnumerable<XNode> DescendantNodesAndSelf();
    public IEnumerable<XElement> DescendantsAndSelf();
    public IEnumerable<XElement> DescendantsAndSelf(XName name);
    public XNamespace GetDefaultNamespace();
    public XNamespace GetNamespaceOfPrefix(string prefix);
    public string GetPrefixOfNamespace(XNamespace ns);
    public static XElement Load(Stream stream);
    public static XElement Load(Stream stream, LoadOptions options);
    public static XElement Load(TextReader textReader);
    public static XElement Load(TextReader textReader, LoadOptions options);
    public static XElement Load(string uri);
    public static XElement Load(string uri, LoadOptions options);
    public static XElement Load(XmlReader reader);
    public static XElement Load(XmlReader reader, LoadOptions options);
+   public static Task<XElement> LoadAsync(Stream stream, LoadOptions options, CancellationToken token);
    ^ Immo Landwerth: See coments on XDocument.
+   public static Task<XElement> LoadAsync(TextReader textReader, LoadOptions options, CancellationToken token);
+   public static Task<XElement> LoadAsync(string uri, LoadOptions options, CancellationToken token);
+   public static Task<XElement> LoadAsync(XmlReader reader, LoadOptions options, CancellationToken token);
    public static explicit operator string (XElement element);
    public static explicit operator bool (XElement element);
    public static explicit operator Nullable<bool> (XElement element);
    public static explicit operator int (XElement element);
    public static explicit operator Nullable<int> (XElement element);
    public static explicit operator uint (XElement element);
    public static explicit operator Nullable<uint> (XElement element);
    public static explicit operator long (XElement element);
    public static explicit operator Nullable<long> (XElement element);
    public static explicit operator ulong (XElement element);
    public static explicit operator Nullable<ulong> (XElement element);
    public static explicit operator float (XElement element);
    public static explicit operator Nullable<float> (XElement element);
    public static explicit operator double (XElement element);
    public static explicit operator Nullable<double> (XElement element);
    public static explicit operator decimal (XElement element);
    public static explicit operator Nullable<decimal> (XElement element);
    public static explicit operator DateTime (XElement element);
    public static explicit operator Nullable<DateTime> (XElement element);
    public static explicit operator DateTimeOffset (XElement element);
    public static explicit operator Nullable<DateTimeOffset> (XElement element);
    public static explicit operator TimeSpan (XElement element);
    public static explicit operator Nullable<TimeSpan> (XElement element);
    public static explicit operator Guid (XElement element);
    public static explicit operator Nullable<Guid> (XElement element);
    public static XElement Parse(string text);
    public static XElement Parse(string text, LoadOptions options);
    public void RemoveAll();
    public void RemoveAttributes();
    public void ReplaceAll(object content);
    public void ReplaceAll(params object[] content);
    public void ReplaceAttributes(object content);
    public void ReplaceAttributes(params object[] content);
    public void Save(Stream stream);
    public void Save(Stream stream, SaveOptions options);
    public void Save(TextWriter textWriter);
    public void Save(TextWriter textWriter, SaveOptions options);
    public void Save(XmlWriter writer);
    public void SetAttributeValue(XName name, object value);
    public void SetElementValue(XName name, object value);
    public void SetValue(object value);
    public override void WriteTo(XmlWriter writer);
  }
 }
```

```diff
---System.ComponentModel.TypeConverter.dll (shipped)
+++System.ComponentModel.TypeConverter.dll (proposed)
 assembly System.ComponentModel.TypeConverter {
  namespace System.ComponentModel {
    public class ArrayConverter : CollectionConverter {
      public ArrayConverter();
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value, Attribute[] attributes);
+     public override bool GetPropertiesSupported(ITypeDescriptorContext context);
    }
+   public class AttributeCollection : ICollection, IEnumerable {
+     public static readonly AttributeCollection Empty;
+     protected AttributeCollection();
+     public AttributeCollection(params Attribute[] attributes);
+     protected virtual Attribute[] Attributes { get; }
+     public int Count { get; }
+     bool System.Collections.ICollection.IsSynchronized { get; }
+     object System.Collections.ICollection.SyncRoot { get; }
+     public virtual Attribute this[int index] { get; }
+     public virtual Attribute this[Type attributeType] { get; }
+     public bool Contains(Attribute attribute);
+     public bool Contains(Attribute[] attributes);
+     public void CopyTo(Array array, int index);
+     public static AttributeCollection FromExisting(AttributeCollection existing, params Attribute[] newAttributes);
+     protected Attribute GetDefaultAttribute(Type attributeType);
+     public IEnumerator GetEnumerator();
+     public bool Matches(Attribute attribute);
+     public bool Matches(Attribute[] attributes);
    }
+   public class AttributeProviderAttribute : Attribute {
+     public AttributeProviderAttribute(string typeName);
+     public AttributeProviderAttribute(string typeName, string propertyName);
+     public AttributeProviderAttribute(Type type);
+     public string PropertyName { get; }
+     public string TypeName { get; }
    }
    public abstract class BaseNumberConverter : TypeConverter {
      protected BaseNumberConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationTypet);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
    public class BooleanConverter : TypeConverter {
      public BooleanConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
    }
    public class ByteConverter : BaseNumberConverter {
      public ByteConverter();
    }
+   public delegate void CancelEventHandler(object sender, CancelEventArgs e);
    public class CharConverter : TypeConverter {
      public CharConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
+   public enum CollectionChangeAction {
+     Add = 1,
+     Refresh = 3,
+     Remove = 2,
    }
+   public class CollectionChangeEventArgs : EventArgs {
+     public CollectionChangeEventArgs(CollectionChangeAction action, object element);
+     public virtual CollectionChangeAction Action { get; }
+     public virtual object Element { get; }
    }
+   public delegate void CollectionChangeEventHandler(object sender, CollectionChangeEventArgs e);
    public class CollectionConverter : TypeConverter {
      public CollectionConverter();
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value, Attribute[] attributes);
+     public override bool GetPropertiesSupported(ITypeDescriptorContext context);
    }
+   public class CultureInfoConverter : TypeConverter {
+     public CultureInfoConverter();
+     public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
+     public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     protected virtual string GetCultureName(CultureInfo culture);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
    }
+   public abstract class CustomTypeDescriptor : ICustomTypeDescriptor {
+     protected CustomTypeDescriptor();
+     protected CustomTypeDescriptor(ICustomTypeDescriptor parent);
+     public virtual AttributeCollection GetAttributes();
+     public virtual string GetClassName();
+     public virtual string GetComponentName();
+     public virtual TypeConverter GetConverter();
+     public virtual EventDescriptor GetDefaultEvent();
+     public virtual PropertyDescriptor GetDefaultProperty();
+     public virtual EventDescriptorCollection GetEvents();
+     public virtual EventDescriptorCollection GetEvents(Attribute[] attributes);
+     public virtual PropertyDescriptorCollection GetProperties();
+     public virtual PropertyDescriptorCollection GetProperties(Attribute[] attributes);
+     public virtual object GetPropertyOwner(PropertyDescriptor pd);
    }
+   public sealed class DataObjectFieldAttribute : Attribute {
+     public DataObjectFieldAttribute(bool primaryKey);
+     public DataObjectFieldAttribute(bool primaryKey, bool isIdentity);
+     public DataObjectFieldAttribute(bool primaryKey, bool isIdentity, bool isNullable);
+     public DataObjectFieldAttribute(bool primaryKey, bool isIdentity, bool isNullable, int length);
+     public bool IsIdentity { get; }
+     public bool IsNullable { get; }
+     public int Length { get; }
+     public bool PrimaryKey { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public sealed class DataObjectMethodAttribute : Attribute {
+     public DataObjectMethodAttribute(DataObjectMethodType methodType);
+     public DataObjectMethodAttribute(DataObjectMethodType methodType, bool isDefault);
+     public bool IsDefault { get; }
+     public DataObjectMethodType MethodType { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public enum DataObjectMethodType {
+     Delete = 4,
+     Fill = 0,
+     Insert = 3,
+     Select = 1,
+     Update = 2,
    }
    public class DateTimeConverter : TypeConverter {
      public DateTimeConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
    public class DateTimeOffsetConverter : TypeConverter {
      public DateTimeOffsetConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
    public class DecimalConverter : BaseNumberConverter {
      public DecimalConverter();
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
+     public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
+   public sealed class DefaultEventAttribute : Attribute {
+     public static readonly DefaultEventAttribute Default;
+     public DefaultEventAttribute(string name);
+     public string Name { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public sealed class DefaultPropertyAttribute : Attribute {
+     public static readonly DefaultPropertyAttribute Default;
+     public DefaultPropertyAttribute(string name);
+     public string Name { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public class DefaultValueAttribute : Attribute {
+     public DefaultValueAttribute(bool value);
+     public DefaultValueAttribute(byte value);
+     public DefaultValueAttribute(char value);
+     public DefaultValueAttribute(double value);
+     public DefaultValueAttribute(short value);
+     public DefaultValueAttribute(int value);
+     public DefaultValueAttribute(long value);
+     public DefaultValueAttribute(object value);
+     public DefaultValueAttribute(float value);
+     public DefaultValueAttribute(string value);
+     public DefaultValueAttribute(Type type, string value);
+     public virtual object Value { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
+     protected void SetValue(object value);
    }
+   public class DescriptionAttribute : Attribute {
+     public static readonly DescriptionAttribute Default;
+     public DescriptionAttribute();
+     public DescriptionAttribute(string description);
+     public virtual string Description { get; }
+     protected string DescriptionValue { get; set; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public class DisplayNameAttribute : Attribute {
      ^ Immo Landwerth: We should move this down, e.g. S.CM.Primitives
+     public static readonly DisplayNameAttribute Default;
+     public DisplayNameAttribute();
+     public DisplayNameAttribute(string displayName);
+     public virtual string DisplayName { get; }
+     protected string DisplayNameValue { get; set; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
    public class DoubleConverter : BaseNumberConverter {
      public DoubleConverter();
    }
    public class EnumConverter : TypeConverter {
      public EnumConverter(Type type);
+     protected virtual IComparer Comparer { get; }
      protected Type EnumType { get; }
+     protected TypeConverter.StandardValuesCollection Values { get; set; }
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
+     public override bool IsValid(ITypeDescriptorContext context, object value);
    }
+   public abstract class EventDescriptor : MemberDescriptor {
+     protected EventDescriptor(MemberDescriptor descr);
+     protected EventDescriptor(MemberDescriptor descr, Attribute[] attrs);
+     protected EventDescriptor(string name, Attribute[] attrs);
+     public abstract Type ComponentType { get; }
+     public abstract Type EventType { get; }
+     public abstract bool IsMulticast { get; }
+     public abstract void AddEventHandler(object component, Delegate value);
+     public abstract void RemoveEventHandler(object component, Delegate value);
    }
+   public class EventDescriptorCollection : ICollection, IEnumerable, IList {
+     public static readonly EventDescriptorCollection Empty;
+     public EventDescriptorCollection(EventDescriptor[] events);
+     public EventDescriptorCollection(EventDescriptor[] events, bool readOnly);
+     public int Count { get; }
+     bool System.Collections.ICollection.IsSynchronized { get; }
+     object System.Collections.ICollection.SyncRoot { get; }
+     bool System.Collections.IList.IsFixedSize { get; }
+     bool System.Collections.IList.IsReadOnly { get; }
+     object System.Collections.IList.this[int index] { get; set; }
+     public virtual EventDescriptor this[int index] { get; }
+     public virtual EventDescriptor this[string name] { get; }
+     public int Add(EventDescriptor value);
+     public void Clear();
+     public bool Contains(EventDescriptor value);
+     public virtual EventDescriptor Find(string name, bool ignoreCase);
+     public IEnumerator GetEnumerator();
+     public int IndexOf(EventDescriptor value);
+     public void Insert(int index, EventDescriptor value);
+     protected void InternalSort(IComparer sorter);
+     protected void InternalSort(string[] names);
+     public void Remove(EventDescriptor value);
+     public void RemoveAt(int index);
+     public virtual EventDescriptorCollection Sort();
+     public virtual EventDescriptorCollection Sort(IComparer comparer);
+     public virtual EventDescriptorCollection Sort(string[] names);
+     public virtual EventDescriptorCollection Sort(string[] names, IComparer comparer);
+     void System.Collections.ICollection.CopyTo(Array array, int index);
+     int System.Collections.IList.Add(object value);
+     bool System.Collections.IList.Contains(object value);
+     int System.Collections.IList.IndexOf(object value);
+     void System.Collections.IList.Insert(int index, object value);
+     void System.Collections.IList.Remove(object value);
    }
+   public sealed class EventHandlerList : IDisposable {
+     public EventHandlerList();
+     public Delegate this[object key] { get; set; }
+     public void AddHandler(object key, Delegate value);
+     public void AddHandlers(EventHandlerList listToAddFrom);
+     public void Dispose();
+     public void RemoveHandler(object key, Delegate value);
    }
+   public sealed class ExtenderProvidedPropertyAttribute : Attribute {
+     public ExtenderProvidedPropertyAttribute();
+     public PropertyDescriptor ExtenderProperty { get; }
+     public IExtenderProvider Provider { get; }
+     public Type ReceiverType { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
    public class GuidConverter : TypeConverter {
      public GuidConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
+   public class HandledEventArgs : EventArgs {
+     public HandledEventArgs();
+     public HandledEventArgs(bool defaultHandledValue);
+     public bool Handled { get; set; }
    }
+   public delegate void HandledEventHandler(object sender, HandledEventArgs e);
+   public interface ICustomTypeDescriptor {
+     AttributeCollection GetAttributes();
+     string GetClassName();
+     string GetComponentName();
+     TypeConverter GetConverter();
+     EventDescriptor GetDefaultEvent();
+     PropertyDescriptor GetDefaultProperty();
+     EventDescriptorCollection GetEvents();
+     EventDescriptorCollection GetEvents(Attribute[] attributes);
+     PropertyDescriptorCollection GetProperties();
+     PropertyDescriptorCollection GetProperties(Attribute[] attributes);
+     object GetPropertyOwner(PropertyDescriptor pd);
    }
+   public interface IExtenderProvider {
+     bool CanExtend(object extendee);
    }
+   public interface IListSource {
+     bool ContainsListCollection { get; }
+     IList GetList();
    }
+   public sealed class ImmutableObjectAttribute : Attribute {
+     public static readonly ImmutableObjectAttribute Default;
+     public static readonly ImmutableObjectAttribute No;
+     public static readonly ImmutableObjectAttribute Yes;
+     public ImmutableObjectAttribute(bool immutable);
+     public bool Immutable { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public interface INestedSite : IServiceProvider, ISite {
+     string FullName { get; }
    }
+   public sealed class InheritanceAttribute : Attribute {
+     public static readonly InheritanceAttribute Default;
+     public static readonly InheritanceAttribute Inherited;
+     public static readonly InheritanceAttribute InheritedReadOnly;
+     public static readonly InheritanceAttribute NotInherited;
+     public InheritanceAttribute();
+     public InheritanceAttribute(InheritanceLevel inheritanceLevel);
+     public InheritanceLevel InheritanceLevel { get; }
+     public override bool Equals(object value);
+     public override int GetHashCode();
+     public override string ToString();
    }
+   public enum InheritanceLevel {
+     Inherited = 1,
+     InheritedReadOnly = 2,
+     NotInherited = 3,
    }
+   public sealed class InitializationEventAttribute : Attribute {
+     public InitializationEventAttribute(string eventName);
+     public string EventName { get; }
    }
    public class Int16Converter : BaseNumberConverter {
      public Int16Converter();
    }
    public class Int32Converter : BaseNumberConverter {
      public Int32Converter();
    }
    public class Int64Converter : BaseNumberConverter {
      public Int64Converter();
    }
+   public class InvalidAsynchronousStateException : ArgumentException {
+     public InvalidAsynchronousStateException();
+     public InvalidAsynchronousStateException(string message);
+     public InvalidAsynchronousStateException(string message, Exception innerException);
    }
+   public interface IRaiseItemChangedEvents {
+     bool RaisesItemChangedEvents { get; }
    }
    public interface ITypeDescriptorContext : IServiceProvider {
      IContainer Container { get; }
      object Instance { get; }
      PropertyDescriptor PropertyDescriptor { get; }
      void OnComponentChanged();
      bool OnComponentChanging();
    }
+   public interface ITypedList {
+     PropertyDescriptorCollection GetItemProperties(PropertyDescriptor[] listAccessors);
+     string GetListName(PropertyDescriptor[] listAccessors);
    }
+   public class ListChangedEventArgs : EventArgs {
+     public ListChangedEventArgs(ListChangedType listChangedType, PropertyDescriptor propDesc);
+     public ListChangedEventArgs(ListChangedType listChangedType, int newIndex);
+     public ListChangedEventArgs(ListChangedType listChangedType, int newIndex, PropertyDescriptor propDesc);
+     public ListChangedEventArgs(ListChangedType listChangedType, int newIndex, int oldIndex);
+     public ListChangedType ListChangedType { get; }
+     public int NewIndex { get; }
+     public int OldIndex { get; }
+     public PropertyDescriptor PropertyDescriptor { get; }
    }
+   public delegate void ListChangedEventHandler(object sender, ListChangedEventArgs e);
+   public enum ListChangedType {
+     ItemAdded = 1,
+     ItemChanged = 4,
+     ItemDeleted = 2,
+     ItemMoved = 3,
+     PropertyDescriptorAdded = 5,
+     PropertyDescriptorChanged = 7,
+     PropertyDescriptorDeleted = 6,
+     Reset = 0,
    }
+   public class ListSortDescription {
+     public ListSortDescription(PropertyDescriptor property, ListSortDirection direction);
+     public PropertyDescriptor PropertyDescriptor { get; set; }
+     public ListSortDirection SortDirection { get; set; }
    }
+   public class ListSortDescriptionCollection : ICollection, IEnumerable, IList {
+     public ListSortDescriptionCollection();
+     public ListSortDescriptionCollection(ListSortDescription[] sorts);
+     public int Count { get; }
+     bool System.Collections.ICollection.IsSynchronized { get; }
+     object System.Collections.ICollection.SyncRoot { get; }
+     bool System.Collections.IList.IsFixedSize { get; }
+     bool System.Collections.IList.IsReadOnly { get; }
+     object System.Collections.IList.this[int index] { get; set; }
+     public ListSortDescription this[int index] { get; set; }
+     public bool Contains(object value);
+     public void CopyTo(Array array, int index);
+     public int IndexOf(object value);
+     IEnumerator System.Collections.IEnumerable.GetEnumerator();
+     int System.Collections.IList.Add(object value);
+     void System.Collections.IList.Clear();
+     void System.Collections.IList.Insert(int index, object value);
+     void System.Collections.IList.Remove(object value);
+     void System.Collections.IList.RemoveAt(int index);
    }
+   public enum ListSortDirection {
+     Ascending = 0,
+     Descending = 1,
    }
+   public sealed class LocalizableAttribute : Attribute {
+     public static readonly LocalizableAttribute Default;
+     public static readonly LocalizableAttribute No;
+     public static readonly LocalizableAttribute Yes;
+     public LocalizableAttribute(bool isLocalizable);
+     public bool IsLocalizable { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public abstract class MemberDescriptor {
+     protected MemberDescriptor(MemberDescriptor descr);
+     protected MemberDescriptor(MemberDescriptor oldMemberDescriptor, Attribute[] newAttributes);
+     protected MemberDescriptor(string name);
+     protected MemberDescriptor(string name, Attribute[] attributes);
+     protected virtual Attribute[] AttributeArray { get; set; }
+     public virtual AttributeCollection Attributes { get; }
+     public virtual string Category { get; }
+     public virtual string Description { get; }
+     public virtual bool DesignTimeOnly { get; }
+     public virtual string DisplayName { get; }
+     public virtual bool IsBrowsable { get; }
+     public virtual string Name { get; }
+     protected virtual int NameHashCode { get; }
+     protected virtual AttributeCollection CreateAttributeCollection();
+     public override bool Equals(object obj);
+     protected virtual void FillAttributes(IList attributeList);
+     protected static MethodInfo FindMethod(Type componentClass, string name, Type[] args, Type returnType);
+     protected static MethodInfo FindMethod(Type componentClass, string name, Type[] args, Type returnType, bool publicOnly);
+     public override int GetHashCode();
+     protected virtual object GetInvocationTarget(Type type, object instance);
+     protected static object GetInvokee(Type componentClass, object component);
+     protected static ISite GetSite(object component);
    }
+   public sealed class MergablePropertyAttribute : Attribute {
+     public static readonly MergablePropertyAttribute Default;
+     public static readonly MergablePropertyAttribute No;
+     public static readonly MergablePropertyAttribute Yes;
+     public MergablePropertyAttribute(bool allowMerge);
+     public bool AllowMerge { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
    public class MultilineStringConverter : TypeConverter {
      public MultilineStringConverter();
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value, Attribute[] attributes);
+     public override bool GetPropertiesSupported(ITypeDescriptorContext context);
    }
+   public sealed class NotifyParentPropertyAttribute : Attribute {
+     public static readonly NotifyParentPropertyAttribute Default;
+     public static readonly NotifyParentPropertyAttribute No;
+     public static readonly NotifyParentPropertyAttribute Yes;
+     public NotifyParentPropertyAttribute(bool notifyParent);
+     public bool NotifyParent { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
    public class NullableConverter : TypeConverter {
      public NullableConverter(Type type);
      public Type NullableType { get; }
      public Type UnderlyingType { get; }
      public TypeConverter UnderlyingTypeConverter { get; }
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override object CreateInstance(ITypeDescriptorContext context, IDictionary propertyValues);
+     public override bool GetCreateInstanceSupported(ITypeDescriptorContext context);
+     public override PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value, Attribute[] attributes);
+     public override bool GetPropertiesSupported(ITypeDescriptorContext context);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
+     public override bool IsValid(ITypeDescriptorContext context, object value);
    }
+   public sealed class ParenthesizePropertyNameAttribute : Attribute {
+     public static readonly ParenthesizePropertyNameAttribute Default;
+     public ParenthesizePropertyNameAttribute();
+     public ParenthesizePropertyNameAttribute(bool needParenthesis);
+     public bool NeedParenthesis { get; }
+     public override bool Equals(object o);
+     public override int GetHashCode();
    }
    public abstract class PropertyDescriptor : MemberDescriptor {
+     protected PropertyDescriptor(MemberDescriptor descr);
+     protected PropertyDescriptor(MemberDescriptor descr, Attribute[] attrs);
+     protected PropertyDescriptor(string name, Attribute[] attrs);
+     public abstract Type ComponentType { get; }
+     public virtual TypeConverter Converter { get; }
+     public virtual bool IsLocalizable { get; }
+     public abstract bool IsReadOnly { get; }
+     public abstract Type PropertyType { get; }
+     public virtual bool SupportsChangeEvents { get; }
+     public virtual void AddValueChanged(object component, EventHandler handler);
+     public abstract bool CanResetValue(object component);
+     protected object CreateInstance(Type type);
+     public override bool Equals(object obj);
+     protected override void FillAttributes(IList attributeList);
+     public PropertyDescriptorCollection GetChildProperties();
+     public PropertyDescriptorCollection GetChildProperties(Attribute[] filter);
+     public PropertyDescriptorCollection GetChildProperties(object instance);
+     public virtual PropertyDescriptorCollection GetChildProperties(object instance, Attribute[] filter);
+     public override int GetHashCode();
+     protected override object GetInvocationTarget(Type type, object instance);
+     protected Type GetTypeFromName(string typeName);
+     public abstract object GetValue(object component);
+     protected internal EventHandler GetValueChangedHandler(object component);
+     protected virtual void OnValueChanged(object component, EventArgs e);
+     public virtual void RemoveValueChanged(object component, EventHandler handler);
+     public abstract void ResetValue(object component);
+     public abstract void SetValue(object component, object value);
+     public abstract bool ShouldSerializeValue(object component);
    }
+   public class PropertyDescriptorCollection : ICollection, IDictionary, IEnumerable, IList {
+     public static readonly PropertyDescriptorCollection Empty;
+     public PropertyDescriptorCollection(PropertyDescriptor[] properties);
+     public PropertyDescriptorCollection(PropertyDescriptor[] properties, bool readOnly);
+     public int Count { get; }
+     bool System.Collections.ICollection.IsSynchronized { get; }
+     object System.Collections.ICollection.SyncRoot { get; }
+     bool System.Collections.IDictionary.IsFixedSize { get; }
+     bool System.Collections.IDictionary.IsReadOnly { get; }
+     object System.Collections.IDictionary.this[object key] { get; set; }
+     ICollection System.Collections.IDictionary.Keys { get; }
+     ICollection System.Collections.IDictionary.Values { get; }
+     bool System.Collections.IList.IsFixedSize { get; }
+     bool System.Collections.IList.IsReadOnly { get; }
+     object System.Collections.IList.this[int index] { get; set; }
+     public virtual PropertyDescriptor this[int index] { get; }
+     public virtual PropertyDescriptor this[string name] { get; }
+     public int Add(PropertyDescriptor value);
+     public void Clear();
+     public bool Contains(PropertyDescriptor value);
+     public void CopyTo(Array array, int index);
+     public virtual PropertyDescriptor Find(string name, bool ignoreCase);
+     public virtual IEnumerator GetEnumerator();
+     public int IndexOf(PropertyDescriptor value);
+     public void Insert(int index, PropertyDescriptor value);
+     protected void InternalSort(IComparer sorter);
+     protected void InternalSort(string[] names);
+     public void Remove(PropertyDescriptor value);
+     public void RemoveAt(int index);
+     public virtual PropertyDescriptorCollection Sort();
+     public virtual PropertyDescriptorCollection Sort(IComparer comparer);
+     public virtual PropertyDescriptorCollection Sort(string[] names);
+     public virtual PropertyDescriptorCollection Sort(string[] names, IComparer comparer);
+     void System.Collections.IDictionary.Add(object key, object value);
+     bool System.Collections.IDictionary.Contains(object key);
+     IDictionaryEnumerator System.Collections.IDictionary.GetEnumerator();
+     void System.Collections.IDictionary.Remove(object key);
+     int System.Collections.IList.Add(object value);
+     bool System.Collections.IList.Contains(object value);
+     int System.Collections.IList.IndexOf(object value);
+     void System.Collections.IList.Insert(int index, object value);
+     void System.Collections.IList.Remove(object value);
    }
+   public sealed class ProvidePropertyAttribute : Attribute {
+     public ProvidePropertyAttribute(string propertyName, string receiverTypeName);
+     public ProvidePropertyAttribute(string propertyName, Type receiverType);
+     public string PropertyName { get; }
+     public string ReceiverTypeName { get; }
+     public override bool Equals(object obj);
+     public override int GetHashCode();
    }
+   public sealed class ReadOnlyAttribute : Attribute {
+     public static readonly ReadOnlyAttribute Default;
+     public static readonly ReadOnlyAttribute No;
+     public static readonly ReadOnlyAttribute Yes;
+     public ReadOnlyAttribute(bool isReadOnly);
+     public bool IsReadOnly { get; }
+     public override bool Equals(object value);
+     public override int GetHashCode();
    }
+   public class ReferenceConverter : TypeConverter {
+     public ReferenceConverter(Type type);
+     public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
+     protected virtual bool IsValueAllowed(ITypeDescriptorContext context, object value);
    }
+   public class RefreshEventArgs : EventArgs {
+     public RefreshEventArgs(object componentChanged);
+     public RefreshEventArgs(Type typeChanged);
+     public object ComponentChanged { get; }
+     public Type TypeChanged { get; }
    }
+   public delegate void RefreshEventHandler(RefreshEventArgs e);
+   public enum RefreshProperties {
+     All = 1,
+     None = 0,
+     Repaint = 2,
    }
+   public sealed class RefreshPropertiesAttribute : Attribute {
+     public static readonly RefreshPropertiesAttribute All;
+     public static readonly RefreshPropertiesAttribute Default;
+     public static readonly RefreshPropertiesAttribute Repaint;
+     public RefreshPropertiesAttribute(RefreshProperties refresh);
+     public RefreshProperties RefreshProperties { get; }
+     public override bool Equals(object value);
+     public override int GetHashCode();
    }
    public class SByteConverter : BaseNumberConverter {
      public SByteConverter();
    }
    public class SingleConverter : BaseNumberConverter {
      public SingleConverter();
    }
    public class StringConverter : TypeConverter {
      public StringConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
    }
    public class TimeSpanConverter : TypeConverter {
      public TimeSpanConverter();
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
    }
    public class TypeConverter {
      public TypeConverter();
      public virtual bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
      public bool CanConvertFrom(Type sourceType);
      public virtual bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public bool CanConvertTo(Type destinationType);
      public virtual object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public object ConvertFrom(object value);
+     public object ConvertFromInvariantString(ITypeDescriptorContext context, string text);
      public object ConvertFromInvariantString(string text);
      public object ConvertFromString(ITypeDescriptorContext context, CultureInfo culture, string text);
+     public object ConvertFromString(ITypeDescriptorContext context, string text);
      public object ConvertFromString(string text);
      public virtual object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
      public object ConvertTo(object value, Type destinationType);
+     public string ConvertToInvariantString(ITypeDescriptorContext context, object value);
      public string ConvertToInvariantString(object value);
      public string ConvertToString(ITypeDescriptorContext context, CultureInfo culture, object value);
+     public string ConvertToString(ITypeDescriptorContext context, object value);
      public string ConvertToString(object value);
+     public object CreateInstance(IDictionary propertyValues);
+     public virtual object CreateInstance(ITypeDescriptorContext context, IDictionary propertyValues);
      protected Exception GetConvertFromException(object value);
      protected Exception GetConvertToException(object value, Type destinationType);
+     public bool GetCreateInstanceSupported();
+     public virtual bool GetCreateInstanceSupported(ITypeDescriptorContext context);
+     public PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value);
+     public virtual PropertyDescriptorCollection GetProperties(ITypeDescriptorContext context, object value, Attribute[] attributes);
+     public PropertyDescriptorCollection GetProperties(object value);
+     public bool GetPropertiesSupported();
+     public virtual bool GetPropertiesSupported(ITypeDescriptorContext context);
+     public ICollection GetStandardValues();
+     public virtual TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public bool GetStandardValuesExclusive();
+     public virtual bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public bool GetStandardValuesSupported();
+     public virtual bool GetStandardValuesSupported(ITypeDescriptorContext context);
+     public virtual bool IsValid(ITypeDescriptorContext context, object value);
+     public bool IsValid(object value);
+     protected PropertyDescriptorCollection SortProperties(PropertyDescriptorCollection props, string[] names);
+     protected abstract class SimplePropertyDescriptor : PropertyDescriptor {
+       protected SimplePropertyDescriptor(Type componentType, string name, Type propertyType);
+       protected SimplePropertyDescriptor(Type componentType, string name, Type propertyType, Attribute[] attributes);
+       public override Type ComponentType { get; }
+       public override bool IsReadOnly { get; }
+       public override Type PropertyType { get; }
+       public override bool CanResetValue(object component);
+       public override void ResetValue(object component);
+       public override bool ShouldSerializeValue(object component);
      }
+     public class StandardValuesCollection : ICollection, IEnumerable {
+       public StandardValuesCollection(ICollection values);
+       public int Count { get; }
+       int System.Collections.ICollection.Count { get; }
+       bool System.Collections.ICollection.IsSynchronized { get; }
+       object System.Collections.ICollection.SyncRoot { get; }
+       public object this[int index] { get; }
+       public void CopyTo(Array array, int index);
+       public IEnumerator GetEnumerator();
+       void System.Collections.ICollection.CopyTo(Array array, int index);
+       IEnumerator System.Collections.IEnumerable.GetEnumerator();
      }
    }
    public sealed class TypeConverterAttribute : Attribute {
+     public static readonly TypeConverterAttribute Default;
+     public TypeConverterAttribute();
      public TypeConverterAttribute(string typeName);
      public TypeConverterAttribute(Type type);
      public string ConverterTypeName { get; }
      public override bool Equals(object obj);
      public override int GetHashCode();
    }
+   public abstract class TypeDescriptionProvider {
+     protected TypeDescriptionProvider();
+     protected TypeDescriptionProvider(TypeDescriptionProvider parent);
+     public virtual object CreateInstance(IServiceProvider provider, Type objectType, Type[] argTypes, object[] args);
+     public virtual IDictionary GetCache(object instance);
+     public virtual ICustomTypeDescriptor GetExtendedTypeDescriptor(object instance);
+     protected internal virtual IExtenderProvider[] GetExtenderProviders(object instance);
+     public virtual string GetFullComponentName(object component);
+     public Type GetReflectionType(object instance);
+     public Type GetReflectionType(Type objectType);
+     public virtual Type GetReflectionType(Type objectType, object instance);
+     public virtual Type GetRuntimeType(Type reflectionType);
+     public ICustomTypeDescriptor GetTypeDescriptor(object instance);
+     public ICustomTypeDescriptor GetTypeDescriptor(Type objectType);
+     public virtual ICustomTypeDescriptor GetTypeDescriptor(Type objectType, object instance);
+     public virtual bool IsSupportedType(Type type);
    }
+   public sealed class TypeDescriptionProviderAttribute : Attribute {
+     public TypeDescriptionProviderAttribute(string typeName);
+     public TypeDescriptionProviderAttribute(Type type);
+     public string TypeName { get; }
    }
    public sealed class TypeDescriptor {
+     public TypeDescriptor();
+     public static Type InterfaceType { get; }
+     public static event RefreshEventHandler Refreshed;
+     public static TypeDescriptionProvider AddAttributes(object instance, params Attribute[] attributes);
+     public static TypeDescriptionProvider AddAttributes(Type type, params Attribute[] attributes);
+     public static void AddProvider(TypeDescriptionProvider provider, object instance);
+     public static void AddProvider(TypeDescriptionProvider provider, Type type);
+     public static void AddProviderTransparent(TypeDescriptionProvider provider, object instance);
+     public static void AddProviderTransparent(TypeDescriptionProvider provider, Type type);
+     public static void CreateAssociation(object primary, object secondary);
+     public static EventDescriptor CreateEvent(Type componentType, EventDescriptor oldEventDescriptor, params Attribute[] attributes);
+     public static EventDescriptor CreateEvent(Type componentType, string name, Type type, params Attribute[] attributes);
+     public static object CreateInstance(IServiceProvider provider, Type objectType, Type[] argTypes, object[] args);
+     public static PropertyDescriptor CreateProperty(Type componentType, PropertyDescriptor oldPropertyDescriptor, params Attribute[] attributes);
+     public static PropertyDescriptor CreateProperty(Type componentType, string name, Type type, params Attribute[] attributes);
+     public static object GetAssociation(Type type, object primary);
+     public static AttributeCollection GetAttributes(object component);
+     public static AttributeCollection GetAttributes(object component, bool noCustomTypeDesc);
+     public static AttributeCollection GetAttributes(Type componentType);
+     public static string GetClassName(object component);
+     public static string GetClassName(object component, bool noCustomTypeDesc);
+     public static string GetClassName(Type componentType);
+     public static string GetComponentName(object component);
+     public static string GetComponentName(object component, bool noCustomTypeDesc);
+     public static TypeConverter GetConverter(object component);
+     public static TypeConverter GetConverter(object component, bool noCustomTypeDesc);
      public static TypeConverter GetConverter(Type type);
+     public static EventDescriptor GetDefaultEvent(object component);
+     public static EventDescriptor GetDefaultEvent(object component, bool noCustomTypeDesc);
+     public static EventDescriptor GetDefaultEvent(Type componentType);
+     public static PropertyDescriptor GetDefaultProperty(object component);
+     public static PropertyDescriptor GetDefaultProperty(object component, bool noCustomTypeDesc);
+     public static PropertyDescriptor GetDefaultProperty(Type componentType);
+     public static EventDescriptorCollection GetEvents(object component);
+     public static EventDescriptorCollection GetEvents(object component, Attribute[] attributes);
+     public static EventDescriptorCollection GetEvents(object component, Attribute[] attributes, bool noCustomTypeDesc);
+     public static EventDescriptorCollection GetEvents(object component, bool noCustomTypeDesc);
+     public static EventDescriptorCollection GetEvents(Type componentType);
+     public static EventDescriptorCollection GetEvents(Type componentType, Attribute[] attributes);
+     public static string GetFullComponentName(object component);
+     public static PropertyDescriptorCollection GetProperties(object component);
+     public static PropertyDescriptorCollection GetProperties(object component, Attribute[] attributes);
+     public static PropertyDescriptorCollection GetProperties(object component, Attribute[] attributes, bool noCustomTypeDesc);
+     public static PropertyDescriptorCollection GetProperties(object component, bool noCustomTypeDesc);
+     public static PropertyDescriptorCollection GetProperties(Type componentType);
+     public static PropertyDescriptorCollection GetProperties(Type componentType, Attribute[] attributes);
+     public static TypeDescriptionProvider GetProvider(object instance);
+     public static TypeDescriptionProvider GetProvider(Type type);
+     public static Type GetReflectionType(object instance);
+     public static Type GetReflectionType(Type type);
+     public static void Refresh(object component);
+     public static void Refresh(Assembly assembly);
+     public static void Refresh(Module module);
+     public static void Refresh(Type type);
+     public static void RemoveAssociation(object primary, object secondary);
+     public static void RemoveAssociations(object primary);
+     public static void RemoveProvider(TypeDescriptionProvider provider, object instance);
+     public static void RemoveProvider(TypeDescriptionProvider provider, Type type);
+     public static void RemoveProviderTransparent(TypeDescriptionProvider provider, object instance);
+     public static void RemoveProviderTransparent(TypeDescriptionProvider provider, Type type);
+     public static void SortDescriptorArray(IList infos);
    }
    public abstract class TypeListConverter : TypeConverter {
      protected TypeListConverter(Type[] types);
      public override bool CanConvertFrom(ITypeDescriptorContext context, Type sourceType);
+     public override bool CanConvertTo(ITypeDescriptorContext context, Type destinationType);
      public override object ConvertFrom(ITypeDescriptorContext context, CultureInfo culture, object value);
      public override object ConvertTo(ITypeDescriptorContext context, CultureInfo culture, object value, Type destinationType);
+     public override TypeConverter.StandardValuesCollection GetStandardValues(ITypeDescriptorContext context);
+     public override bool GetStandardValuesExclusive(ITypeDescriptorContext context);
+     public override bool GetStandardValuesSupported(ITypeDescriptorContext context);
    }
    public class UInt16Converter : BaseNumberConverter {
      public UInt16Converter();
    }
    public class UInt32Converter : BaseNumberConverter {
      public UInt32Converter();
    }
    public class UInt64Converter : BaseNumberConverter {
      public UInt64Converter();
    }
+   public class WarningException : Exception {
+     public WarningException();
+     public WarningException(string message);
+     public WarningException(string message, Exception innerException);
+     public WarningException(string message, string helpUrl);
+     public WarningException(string message, string helpUrl, string helpTopic);
+     public string HelpTopic { get; }
+     public string HelpUrl { get; }
    }
  }
 }
```

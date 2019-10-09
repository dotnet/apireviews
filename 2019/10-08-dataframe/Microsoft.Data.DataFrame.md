```C#
namespace Microsoft.Data {
    public enum DropNullOptions {
        Any = 0,
        All = 1,
    }
    [DefaultMember("Item")]
    public class ArrowStringColumn : BaseColumn, IEnumerable<string>, IEnumerable {
        public ArrowStringColumn(string name) {}
        public ArrowStringColumn(string name, ReadOnlyMemory<byte> values, ReadOnlyMemory<byte> offsets, ReadOnlyMemory<byte> nullBits, int length, int nullCount) {}
        public override long NullCount { get; }
        public string this[long rowIndex] { get; set; }
        public List<string> this[long startIndex, int length] { get; }
        public override BaseColumn Clone(BaseColumn mapIndices = null, bool invertMapIndices = false, long numberOfNullsToAppend = 0) {}
        public override BaseColumn FillNulls(object value, bool inPlace = false) {}
        [IteratorStateMachine()]
        public IEnumerator<string> GetEnumerator() {}
        [IteratorStateMachine()]
        public IEnumerable<ReadOnlyMemory<byte>> GetReadOnlyDataBuffers() {}
        [IteratorStateMachine()]
        public IEnumerable<ReadOnlyMemory<byte>> GetReadOnlyNullBitMapBuffers() {}
        [IteratorStateMachine()]
        public IEnumerable<ReadOnlyMemory<int>> GetReadOnlyOffsetsBuffers() {}
        public bool GetValidityBit(long index) {}
        public override GroupBy GroupBy(int columnIndex, DataFrame parent) {}
        public override Dictionary<TKey, ICollection<long>> GroupColumnValues<TKey>() {}
        public bool IsValid(long index) {}
        public override BaseColumn Sort(bool ascending = true) {}
        public override DataFrame ValueCounts() {}
        protected internal override void AddDataViewColumn(Builder builder) {}
        protected internal override Array AsArrowArray(long startIndex, int numberOfRows) {}
        protected internal override Field Field() {}
        protected internal override Delegate GetDataViewGetter(DataViewRowCursor cursor) {}
        protected override IEnumerator GetEnumeratorCore() {}
        protected override object GetValue(long rowIndex) {}
        protected override object GetValue(long startIndex, int length) {}
        protected internal override int MaxRecordBatchLength(long startIndex) {}
        protected override void SetValue(long rowIndex, object value) {}
    }
    [DefaultMember("Item")]
    public abstract class BaseColumn : IEnumerable {
        public BaseColumn(string name, long length, Type type) {}
        public Type DataType { get; }
        public long Length { get; protected set; }
        public string Name { get; }
        public abstract long NullCount { get; }
        public object this[long rowIndex] { get; set; }
        public object this[long startIndex, int length] { get; }
        public static BaseColumn operator +(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator +(BaseColumn column, bool value) {}
        public static BaseColumn operator +(BaseColumn column, byte value) {}
        public static BaseColumn operator +(BaseColumn column, char value) {}
        public static BaseColumn operator +(BaseColumn column, decimal value) {}
        public static BaseColumn operator +(BaseColumn column, double value) {}
        public static BaseColumn operator +(BaseColumn column, float value) {}
        public static BaseColumn operator +(BaseColumn column, int value) {}
        public static BaseColumn operator +(BaseColumn column, long value) {}
        public static BaseColumn operator +(BaseColumn column, sbyte value) {}
        public static BaseColumn operator +(BaseColumn column, short value) {}
        public static BaseColumn operator +(BaseColumn column, uint value) {}
        public static BaseColumn operator +(BaseColumn column, ulong value) {}
        public static BaseColumn operator +(BaseColumn column, ushort value) {}
        public static BaseColumn operator &(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator &(BaseColumn column, bool value) {}
        public static BaseColumn operator &(BaseColumn column, byte value) {}
        public static BaseColumn operator &(BaseColumn column, char value) {}
        public static BaseColumn operator &(BaseColumn column, decimal value) {}
        public static BaseColumn operator &(BaseColumn column, double value) {}
        public static BaseColumn operator &(BaseColumn column, float value) {}
        public static BaseColumn operator &(BaseColumn column, int value) {}
        public static BaseColumn operator &(BaseColumn column, long value) {}
        public static BaseColumn operator &(BaseColumn column, sbyte value) {}
        public static BaseColumn operator &(BaseColumn column, short value) {}
        public static BaseColumn operator &(BaseColumn column, uint value) {}
        public static BaseColumn operator &(BaseColumn column, ulong value) {}
        public static BaseColumn operator &(BaseColumn column, ushort value) {}
        public static BaseColumn operator |(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator |(BaseColumn column, bool value) {}
        public static BaseColumn operator |(BaseColumn column, byte value) {}
        public static BaseColumn operator |(BaseColumn column, char value) {}
        public static BaseColumn operator |(BaseColumn column, decimal value) {}
        public static BaseColumn operator |(BaseColumn column, double value) {}
        public static BaseColumn operator |(BaseColumn column, float value) {}
        public static BaseColumn operator |(BaseColumn column, int value) {}
        public static BaseColumn operator |(BaseColumn column, long value) {}
        public static BaseColumn operator |(BaseColumn column, sbyte value) {}
        public static BaseColumn operator |(BaseColumn column, short value) {}
        public static BaseColumn operator |(BaseColumn column, uint value) {}
        public static BaseColumn operator |(BaseColumn column, ulong value) {}
        public static BaseColumn operator |(BaseColumn column, ushort value) {}
        public static BaseColumn operator /(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator /(BaseColumn column, bool value) {}
        public static BaseColumn operator /(BaseColumn column, byte value) {}
        public static BaseColumn operator /(BaseColumn column, char value) {}
        public static BaseColumn operator /(BaseColumn column, decimal value) {}
        public static BaseColumn operator /(BaseColumn column, double value) {}
        public static BaseColumn operator /(BaseColumn column, float value) {}
        public static BaseColumn operator /(BaseColumn column, int value) {}
        public static BaseColumn operator /(BaseColumn column, long value) {}
        public static BaseColumn operator /(BaseColumn column, sbyte value) {}
        public static BaseColumn operator /(BaseColumn column, short value) {}
        public static BaseColumn operator /(BaseColumn column, uint value) {}
        public static BaseColumn operator /(BaseColumn column, ulong value) {}
        public static BaseColumn operator /(BaseColumn column, ushort value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator ==(BaseColumn column, ushort value) {}
        public static BaseColumn operator ^(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator ^(BaseColumn column, bool value) {}
        public static BaseColumn operator ^(BaseColumn column, byte value) {}
        public static BaseColumn operator ^(BaseColumn column, char value) {}
        public static BaseColumn operator ^(BaseColumn column, decimal value) {}
        public static BaseColumn operator ^(BaseColumn column, double value) {}
        public static BaseColumn operator ^(BaseColumn column, float value) {}
        public static BaseColumn operator ^(BaseColumn column, int value) {}
        public static BaseColumn operator ^(BaseColumn column, long value) {}
        public static BaseColumn operator ^(BaseColumn column, sbyte value) {}
        public static BaseColumn operator ^(BaseColumn column, short value) {}
        public static BaseColumn operator ^(BaseColumn column, uint value) {}
        public static BaseColumn operator ^(BaseColumn column, ulong value) {}
        public static BaseColumn operator ^(BaseColumn column, ushort value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator >(BaseColumn column, ushort value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator >=(BaseColumn column, ushort value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator !=(BaseColumn column, ushort value) {}
        public static BaseColumn operator <<(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator <(BaseColumn column, ushort value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn left, BaseColumn right) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, bool value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, byte value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, char value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, decimal value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, double value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, float value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, int value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, long value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, sbyte value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, short value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, uint value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, ulong value) {}
        public static PrimitiveColumn<bool> operator <=(BaseColumn column, ushort value) {}
        public static BaseColumn operator %(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator %(BaseColumn column, bool value) {}
        public static BaseColumn operator %(BaseColumn column, byte value) {}
        public static BaseColumn operator %(BaseColumn column, char value) {}
        public static BaseColumn operator %(BaseColumn column, decimal value) {}
        public static BaseColumn operator %(BaseColumn column, double value) {}
        public static BaseColumn operator %(BaseColumn column, float value) {}
        public static BaseColumn operator %(BaseColumn column, int value) {}
        public static BaseColumn operator %(BaseColumn column, long value) {}
        public static BaseColumn operator %(BaseColumn column, sbyte value) {}
        public static BaseColumn operator %(BaseColumn column, short value) {}
        public static BaseColumn operator %(BaseColumn column, uint value) {}
        public static BaseColumn operator %(BaseColumn column, ulong value) {}
        public static BaseColumn operator %(BaseColumn column, ushort value) {}
        public static BaseColumn operator *(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator *(BaseColumn column, bool value) {}
        public static BaseColumn operator *(BaseColumn column, byte value) {}
        public static BaseColumn operator *(BaseColumn column, char value) {}
        public static BaseColumn operator *(BaseColumn column, decimal value) {}
        public static BaseColumn operator *(BaseColumn column, double value) {}
        public static BaseColumn operator *(BaseColumn column, float value) {}
        public static BaseColumn operator *(BaseColumn column, int value) {}
        public static BaseColumn operator *(BaseColumn column, long value) {}
        public static BaseColumn operator *(BaseColumn column, sbyte value) {}
        public static BaseColumn operator *(BaseColumn column, short value) {}
        public static BaseColumn operator *(BaseColumn column, uint value) {}
        public static BaseColumn operator *(BaseColumn column, ulong value) {}
        public static BaseColumn operator *(BaseColumn column, ushort value) {}
        public static BaseColumn operator >>(BaseColumn column, int value) {}
        public static BaseColumn operator -(BaseColumn left, BaseColumn right) {}
        public static BaseColumn operator -(BaseColumn column, bool value) {}
        public static BaseColumn operator -(BaseColumn column, byte value) {}
        public static BaseColumn operator -(BaseColumn column, char value) {}
        public static BaseColumn operator -(BaseColumn column, decimal value) {}
        public static BaseColumn operator -(BaseColumn column, double value) {}
        public static BaseColumn operator -(BaseColumn column, float value) {}
        public static BaseColumn operator -(BaseColumn column, int value) {}
        public static BaseColumn operator -(BaseColumn column, long value) {}
        public static BaseColumn operator -(BaseColumn column, sbyte value) {}
        public static BaseColumn operator -(BaseColumn column, short value) {}
        public static BaseColumn operator -(BaseColumn column, uint value) {}
        public static BaseColumn operator -(BaseColumn column, ulong value) {}
        public static BaseColumn operator -(BaseColumn column, ushort value) {}
        public virtual BaseColumn Abs(bool inPlace = false) {}
        public virtual BaseColumn Add(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Add<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual bool All() {}
        public virtual BaseColumn And(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn And<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual bool Any() {}
        public virtual BaseColumn Clip<U>(U lower, U upper) {}
        public virtual BaseColumn Clone(BaseColumn mapIndices = null, bool invertMapIndices = false, long numberOfNullsToAppend = 0) {}
        public virtual BaseColumn CumulativeMax(bool inPlace = false) {}
        public virtual BaseColumn CumulativeMax(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public virtual BaseColumn CumulativeMin(bool inPlace = false) {}
        public virtual BaseColumn CumulativeMin(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public virtual BaseColumn CumulativeProduct(bool inPlace = false) {}
        public virtual BaseColumn CumulativeProduct(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public virtual BaseColumn CumulativeSum(bool inPlace = false) {}
        public virtual BaseColumn CumulativeSum(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public virtual DataFrame Description() {}
        public virtual BaseColumn Divide(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Divide<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual PrimitiveColumn<bool> Equals(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> Equals<T>(T value) where T : unmanaged {}
        public virtual BaseColumn FillNulls(object value, bool inPlace = false) {}
        public virtual BaseColumn Filter<U>(U lower, U upper) {}
        public virtual PrimitiveColumn<bool> GreaterThan(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> GreaterThan<T>(T value) where T : unmanaged {}
        public virtual PrimitiveColumn<bool> GreaterThanOrEqual(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> GreaterThanOrEqual<T>(T value) where T : unmanaged {}
        public virtual GroupBy GroupBy(int columnIndex, DataFrame parent) {}
        public virtual Dictionary<TKey, ICollection<long>> GroupColumnValues<TKey>() {}
        public virtual bool HasDescription() {}
        public virtual bool IsNumericColumn() {}
        public virtual BaseColumn LeftShift(int value, bool inPlace = false) {}
        public virtual PrimitiveColumn<bool> LessThan(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> LessThan<T>(T value) where T : unmanaged {}
        public virtual PrimitiveColumn<bool> LessThanOrEqual(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> LessThanOrEqual<T>(T value) where T : unmanaged {}
        public virtual object Max() {}
        public virtual object Max(IEnumerable<long> rowIndices) {}
        public virtual double Mean() {}
        public virtual double Median() {}
        public virtual object Min() {}
        public virtual object Min(IEnumerable<long> rowIndices) {}
        public virtual BaseColumn Modulo(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Modulo<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual BaseColumn Multiply(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Multiply<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual PrimitiveColumn<bool> NotEquals(BaseColumn column) {}
        public virtual PrimitiveColumn<bool> NotEquals<T>(T value) where T : unmanaged {}
        public virtual BaseColumn Or(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Or<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual object Product() {}
        public virtual object Product(IEnumerable<long> rowIndices) {}
        public virtual void Resize(long length) {}
        public virtual BaseColumn RightShift(int value, bool inPlace = false) {}
        public virtual BaseColumn Round(bool inPlace = false) {}
        public void SetName(string newName, DataFrame dataFrame = null) {}
        public virtual BaseColumn Sort(bool ascending = true) {}
        public virtual BaseColumn Subtract(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Subtract<T>(T value, bool inPlace = false) where T : unmanaged {}
        public virtual object Sum() {}
        public virtual object Sum(IEnumerable<long> rowIndices) {}
        public virtual DataFrame ValueCounts() {}
        public virtual BaseColumn Xor(BaseColumn column, bool inPlace = false) {}
        public virtual BaseColumn Xor<T>(T value, bool inPlace = false) where T : unmanaged {}
        protected internal virtual void AddDataViewColumn(Builder builder) {}
        protected internal virtual Array AsArrowArray(long startIndex, int numberOfRows) {}
        protected internal virtual Field Field() {}
        protected internal virtual Delegate GetDataViewGetter(DataViewRowCursor cursor) {}
        protected abstract IEnumerator GetEnumeratorCore();
        protected abstract object GetValue(long rowIndex);
        protected abstract object GetValue(long startIndex, int length);
        protected internal virtual int MaxRecordBatchLength(long startIndex) {}
        protected abstract void SetValue(long rowIndex, object value);
    }
    [DefaultMember("Item")]
    public class DataFrame : IDataView {
        public DataFrame() {}
        public DataFrame(IList<BaseColumn> columns) {}
        public DataFrame(RecordBatch recordBatch) {}
        public int ColumnCount { get; }
        public IList<string> Columns { get; }
        public long RowCount { get; }
        public object this[long rowIndex, int columnIndex] { get; set; }
        public IList<object> this[long rowIndex] { get; }
        public DataFrame this[BaseColumn filterColumn] { get; }
        public DataFrame this[IEnumerable<int> filter] { get; }
        public BaseColumn this[string columnName] { get; set; }
        public static DataFrame operator +(DataFrame df, bool value) {}
        public static DataFrame operator +(DataFrame df, byte value) {}
        public static DataFrame operator +(DataFrame df, char value) {}
        public static DataFrame operator +(DataFrame df, decimal value) {}
        public static DataFrame operator +(DataFrame df, double value) {}
        public static DataFrame operator +(DataFrame df, float value) {}
        public static DataFrame operator +(DataFrame df, int value) {}
        public static DataFrame operator +(DataFrame df, long value) {}
        public static DataFrame operator +(DataFrame df, sbyte value) {}
        public static DataFrame operator +(DataFrame df, short value) {}
        public static DataFrame operator +(DataFrame df, uint value) {}
        public static DataFrame operator +(DataFrame df, ulong value) {}
        public static DataFrame operator +(DataFrame df, ushort value) {}
        public static DataFrame operator &(DataFrame df, bool value) {}
        public static DataFrame operator &(DataFrame df, byte value) {}
        public static DataFrame operator &(DataFrame df, char value) {}
        public static DataFrame operator &(DataFrame df, decimal value) {}
        public static DataFrame operator &(DataFrame df, double value) {}
        public static DataFrame operator &(DataFrame df, float value) {}
        public static DataFrame operator &(DataFrame df, int value) {}
        public static DataFrame operator &(DataFrame df, long value) {}
        public static DataFrame operator &(DataFrame df, sbyte value) {}
        public static DataFrame operator &(DataFrame df, short value) {}
        public static DataFrame operator &(DataFrame df, uint value) {}
        public static DataFrame operator &(DataFrame df, ulong value) {}
        public static DataFrame operator &(DataFrame df, ushort value) {}
        public static DataFrame operator |(DataFrame df, bool value) {}
        public static DataFrame operator |(DataFrame df, byte value) {}
        public static DataFrame operator |(DataFrame df, char value) {}
        public static DataFrame operator |(DataFrame df, decimal value) {}
        public static DataFrame operator |(DataFrame df, double value) {}
        public static DataFrame operator |(DataFrame df, float value) {}
        public static DataFrame operator |(DataFrame df, int value) {}
        public static DataFrame operator |(DataFrame df, long value) {}
        public static DataFrame operator |(DataFrame df, sbyte value) {}
        public static DataFrame operator |(DataFrame df, short value) {}
        public static DataFrame operator |(DataFrame df, uint value) {}
        public static DataFrame operator |(DataFrame df, ulong value) {}
        public static DataFrame operator |(DataFrame df, ushort value) {}
        public static DataFrame operator /(DataFrame df, bool value) {}
        public static DataFrame operator /(DataFrame df, byte value) {}
        public static DataFrame operator /(DataFrame df, char value) {}
        public static DataFrame operator /(DataFrame df, decimal value) {}
        public static DataFrame operator /(DataFrame df, double value) {}
        public static DataFrame operator /(DataFrame df, float value) {}
        public static DataFrame operator /(DataFrame df, int value) {}
        public static DataFrame operator /(DataFrame df, long value) {}
        public static DataFrame operator /(DataFrame df, sbyte value) {}
        public static DataFrame operator /(DataFrame df, short value) {}
        public static DataFrame operator /(DataFrame df, uint value) {}
        public static DataFrame operator /(DataFrame df, ulong value) {}
        public static DataFrame operator /(DataFrame df, ushort value) {}
        public static DataFrame operator ==(DataFrame df, bool value) {}
        public static DataFrame operator ==(DataFrame df, byte value) {}
        public static DataFrame operator ==(DataFrame df, char value) {}
        public static DataFrame operator ==(DataFrame df, decimal value) {}
        public static DataFrame operator ==(DataFrame df, double value) {}
        public static DataFrame operator ==(DataFrame df, float value) {}
        public static DataFrame operator ==(DataFrame df, int value) {}
        public static DataFrame operator ==(DataFrame df, long value) {}
        public static DataFrame operator ==(DataFrame df, sbyte value) {}
        public static DataFrame operator ==(DataFrame df, short value) {}
        public static DataFrame operator ==(DataFrame df, uint value) {}
        public static DataFrame operator ==(DataFrame df, ulong value) {}
        public static DataFrame operator ==(DataFrame df, ushort value) {}
        public static DataFrame operator ^(DataFrame df, bool value) {}
        public static DataFrame operator ^(DataFrame df, byte value) {}
        public static DataFrame operator ^(DataFrame df, char value) {}
        public static DataFrame operator ^(DataFrame df, decimal value) {}
        public static DataFrame operator ^(DataFrame df, double value) {}
        public static DataFrame operator ^(DataFrame df, float value) {}
        public static DataFrame operator ^(DataFrame df, int value) {}
        public static DataFrame operator ^(DataFrame df, long value) {}
        public static DataFrame operator ^(DataFrame df, sbyte value) {}
        public static DataFrame operator ^(DataFrame df, short value) {}
        public static DataFrame operator ^(DataFrame df, uint value) {}
        public static DataFrame operator ^(DataFrame df, ulong value) {}
        public static DataFrame operator ^(DataFrame df, ushort value) {}
        public static DataFrame operator >(DataFrame df, bool value) {}
        public static DataFrame operator >(DataFrame df, byte value) {}
        public static DataFrame operator >(DataFrame df, char value) {}
        public static DataFrame operator >(DataFrame df, decimal value) {}
        public static DataFrame operator >(DataFrame df, double value) {}
        public static DataFrame operator >(DataFrame df, float value) {}
        public static DataFrame operator >(DataFrame df, int value) {}
        public static DataFrame operator >(DataFrame df, long value) {}
        public static DataFrame operator >(DataFrame df, sbyte value) {}
        public static DataFrame operator >(DataFrame df, short value) {}
        public static DataFrame operator >(DataFrame df, uint value) {}
        public static DataFrame operator >(DataFrame df, ulong value) {}
        public static DataFrame operator >(DataFrame df, ushort value) {}
        public static DataFrame operator >=(DataFrame df, bool value) {}
        public static DataFrame operator >=(DataFrame df, byte value) {}
        public static DataFrame operator >=(DataFrame df, char value) {}
        public static DataFrame operator >=(DataFrame df, decimal value) {}
        public static DataFrame operator >=(DataFrame df, double value) {}
        public static DataFrame operator >=(DataFrame df, float value) {}
        public static DataFrame operator >=(DataFrame df, int value) {}
        public static DataFrame operator >=(DataFrame df, long value) {}
        public static DataFrame operator >=(DataFrame df, sbyte value) {}
        public static DataFrame operator >=(DataFrame df, short value) {}
        public static DataFrame operator >=(DataFrame df, uint value) {}
        public static DataFrame operator >=(DataFrame df, ulong value) {}
        public static DataFrame operator >=(DataFrame df, ushort value) {}
        public static DataFrame operator !=(DataFrame df, bool value) {}
        public static DataFrame operator !=(DataFrame df, byte value) {}
        public static DataFrame operator !=(DataFrame df, char value) {}
        public static DataFrame operator !=(DataFrame df, decimal value) {}
        public static DataFrame operator !=(DataFrame df, double value) {}
        public static DataFrame operator !=(DataFrame df, float value) {}
        public static DataFrame operator !=(DataFrame df, int value) {}
        public static DataFrame operator !=(DataFrame df, long value) {}
        public static DataFrame operator !=(DataFrame df, sbyte value) {}
        public static DataFrame operator !=(DataFrame df, short value) {}
        public static DataFrame operator !=(DataFrame df, uint value) {}
        public static DataFrame operator !=(DataFrame df, ulong value) {}
        public static DataFrame operator !=(DataFrame df, ushort value) {}
        public static DataFrame operator <<(DataFrame df, int value) {}
        public static DataFrame operator <(DataFrame df, bool value) {}
        public static DataFrame operator <(DataFrame df, byte value) {}
        public static DataFrame operator <(DataFrame df, char value) {}
        public static DataFrame operator <(DataFrame df, decimal value) {}
        public static DataFrame operator <(DataFrame df, double value) {}
        public static DataFrame operator <(DataFrame df, float value) {}
        public static DataFrame operator <(DataFrame df, int value) {}
        public static DataFrame operator <(DataFrame df, long value) {}
        public static DataFrame operator <(DataFrame df, sbyte value) {}
        public static DataFrame operator <(DataFrame df, short value) {}
        public static DataFrame operator <(DataFrame df, uint value) {}
        public static DataFrame operator <(DataFrame df, ulong value) {}
        public static DataFrame operator <(DataFrame df, ushort value) {}
        public static DataFrame operator <=(DataFrame df, bool value) {}
        public static DataFrame operator <=(DataFrame df, byte value) {}
        public static DataFrame operator <=(DataFrame df, char value) {}
        public static DataFrame operator <=(DataFrame df, decimal value) {}
        public static DataFrame operator <=(DataFrame df, double value) {}
        public static DataFrame operator <=(DataFrame df, float value) {}
        public static DataFrame operator <=(DataFrame df, int value) {}
        public static DataFrame operator <=(DataFrame df, long value) {}
        public static DataFrame operator <=(DataFrame df, sbyte value) {}
        public static DataFrame operator <=(DataFrame df, short value) {}
        public static DataFrame operator <=(DataFrame df, uint value) {}
        public static DataFrame operator <=(DataFrame df, ulong value) {}
        public static DataFrame operator <=(DataFrame df, ushort value) {}
        public static DataFrame operator %(DataFrame df, bool value) {}
        public static DataFrame operator %(DataFrame df, byte value) {}
        public static DataFrame operator %(DataFrame df, char value) {}
        public static DataFrame operator %(DataFrame df, decimal value) {}
        public static DataFrame operator %(DataFrame df, double value) {}
        public static DataFrame operator %(DataFrame df, float value) {}
        public static DataFrame operator %(DataFrame df, int value) {}
        public static DataFrame operator %(DataFrame df, long value) {}
        public static DataFrame operator %(DataFrame df, sbyte value) {}
        public static DataFrame operator %(DataFrame df, short value) {}
        public static DataFrame operator %(DataFrame df, uint value) {}
        public static DataFrame operator %(DataFrame df, ulong value) {}
        public static DataFrame operator %(DataFrame df, ushort value) {}
        public static DataFrame operator *(DataFrame df, bool value) {}
        public static DataFrame operator *(DataFrame df, byte value) {}
        public static DataFrame operator *(DataFrame df, char value) {}
        public static DataFrame operator *(DataFrame df, decimal value) {}
        public static DataFrame operator *(DataFrame df, double value) {}
        public static DataFrame operator *(DataFrame df, float value) {}
        public static DataFrame operator *(DataFrame df, int value) {}
        public static DataFrame operator *(DataFrame df, long value) {}
        public static DataFrame operator *(DataFrame df, sbyte value) {}
        public static DataFrame operator *(DataFrame df, short value) {}
        public static DataFrame operator *(DataFrame df, uint value) {}
        public static DataFrame operator *(DataFrame df, ulong value) {}
        public static DataFrame operator *(DataFrame df, ushort value) {}
        public static DataFrame operator >>(DataFrame df, int value) {}
        public static DataFrame operator -(DataFrame df, bool value) {}
        public static DataFrame operator -(DataFrame df, byte value) {}
        public static DataFrame operator -(DataFrame df, char value) {}
        public static DataFrame operator -(DataFrame df, decimal value) {}
        public static DataFrame operator -(DataFrame df, double value) {}
        public static DataFrame operator -(DataFrame df, float value) {}
        public static DataFrame operator -(DataFrame df, int value) {}
        public static DataFrame operator -(DataFrame df, long value) {}
        public static DataFrame operator -(DataFrame df, sbyte value) {}
        public static DataFrame operator -(DataFrame df, short value) {}
        public static DataFrame operator -(DataFrame df, uint value) {}
        public static DataFrame operator -(DataFrame df, ulong value) {}
        public static DataFrame operator -(DataFrame df, ushort value) {}
        public static DataFrame ReadCsv(string filename, char separator = ',', bool header = true, string[] columnNames = null, Type[] dataTypes = null, int numRows = -1, int guessRows = 10, bool addIndexColumn = false) {}
        public static DataFrame ReadStream(Func<StreamReader> createStream, char separator = ',', bool header = true, string[] columnNames = null, Type[] dataTypes = null, long numberOfRowsToRead = -1, int guessRows = 10, bool addIndexColumn = false) {}
        public DataFrame Add<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Add<T>(T value) where T : unmanaged {}
        public DataFrame AddPrefix(string prefix) {}
        public DataFrame AddSuffix(string suffix) {}
        public DataFrame And<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame And<T>(T value) where T : unmanaged {}
        [IteratorStateMachine()]
        public IEnumerable<RecordBatch> AsArrowRecordBatches() {}
        public DataFrame Clip<U>(U lower, U upper) {}
        public BaseColumn Column(int index) {}
        public DataFrame Description() {}
        public DataFrame Divide<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Divide<T>(T value) where T : unmanaged {}
        public DataFrame DropNulls(DropNullOptions options = Any) {}
        public DataFrame Equals<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Equals<T>(T value) where T : unmanaged {}
        public DataFrame FillNulls(object value) {}
        public DataFrame FillNulls(IList<object> values) {}
        public DataFrame GreaterThan<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame GreaterThan<T>(T value) where T : unmanaged {}
        public DataFrame GreaterThanOrEqual<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame GreaterThanOrEqual<T>(T value) where T : unmanaged {}
        public GroupBy GroupBy(string columnName) {}
        public IList<IList<object>> Head(int numberOfRows) {}
        public void InsertColumn(int columnIndex, BaseColumn column) {}
        public DataFrame Join(DataFrame other, string leftSuffix = "_left", string rightSuffix = "_right", JoinAlgorithm joinAlgorithm = Left) {}
        public DataFrame LeftShift(int value) {}
        public DataFrame LessThan<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame LessThan<T>(T value) where T : unmanaged {}
        public DataFrame LessThanOrEqual<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame LessThanOrEqual<T>(T value) where T : unmanaged {}
        public DataFrame Merge<TKey>(DataFrame other, string leftJoinColumn, string rightJoinColumn, string leftSuffix = "_left", string rightSuffix = "_right", JoinAlgorithm joinAlgorithm = Left) {}
        public DataFrame Modulo<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Modulo<T>(T value) where T : unmanaged {}
        public DataFrame Multiply<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Multiply<T>(T value) where T : unmanaged {}
        public DataFrame NotEquals<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame NotEquals<T>(T value) where T : unmanaged {}
        public DataFrame Or<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Or<T>(T value) where T : unmanaged {}
        public void RemoveColumn(int columnIndex) {}
        public void RemoveColumn(string columnName) {}
        public DataFrame RightShift(int value) {}
        public DataFrame Sample(int numberOfRows) {}
        public void SetColumn(int columnIndex, BaseColumn column) {}
        public void SetColumnName(BaseColumn column, string newName) {}
        public DataFrame Sort(string columnName, bool ascending = true) {}
        public DataFrame Subtract<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Subtract<T>(T value) where T : unmanaged {}
        public IList<IList<object>> Tail(int numberOfRows) {}
        public DataFrame Xor<T>(IReadOnlyList<T> values) where T : unmanaged {}
        public DataFrame Xor<T>(T value) where T : unmanaged {}
        public override string ToString() {}
    }
    public abstract class GroupBy {
        protected GroupBy() {}
        public abstract DataFrame Count(params string[] columnNames);
        public abstract DataFrame First(params string[] columnNames);
        public abstract DataFrame Head(int numberOfRows);
        public abstract DataFrame Max(params string[] columnNames);
        public abstract DataFrame Mean(params string[] columnNames);
        public abstract DataFrame Min(params string[] columnNames);
        public abstract DataFrame Product(params string[] columnNames);
        public abstract DataFrame Sum(params string[] columnNames);
        public abstract DataFrame Tail(int numberOfRows);
    }
    public class GroupBy<TKey> : GroupBy {
        public GroupBy(DataFrame dataFrame, int groupByColumnIndex, Dictionary<TKey, ICollection<long>> keyToRowIndices) {}
        public override DataFrame Count(params string[] columnNames) {}
        public override DataFrame First(params string[] columnNames) {}
        public override DataFrame Head(int numberOfRows) {}
        public override DataFrame Max(params string[] columnNames) {}
        public override DataFrame Mean(params string[] columnNames) {}
        public override DataFrame Min(params string[] columnNames) {}
        public override DataFrame Product(params string[] columnNames) {}
        public override DataFrame Sum(params string[] columnNames) {}
        public override DataFrame Tail(int numberOfRows) {}
    }
    [DefaultMember("Item")]
    public class PrimitiveColumn<T> where T : unmanaged : BaseColumn, IEnumerable<T?>, IEnumerable {
        public PrimitiveColumn(string name, IEnumerable<T?> values) {}
        public PrimitiveColumn(string name, IEnumerable<T> values) {}
        public PrimitiveColumn(string name, long length = 0) {}
        public PrimitiveColumn(string name, ReadOnlyMemory<byte> buffer, ReadOnlyMemory<byte> nullBitMap, int length = 0, int nullCount = 0) {}
        public override long NullCount { get; }
        public IList<T?> this[long startIndex, int length] { get; }
        public T? this[long rowIndex] { get; set; }
        public override BaseColumn Abs(bool inPlace = false) {}
        public override BaseColumn Add(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Add<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override bool All() {}
        public override BaseColumn And(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn And<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override bool Any() {}
        public void Append(T? value) {}
        public void AppendMany(T? value, long count) {}
        public void ApplyElementwise(Func<T?, long, T?> func) {}
        public override BaseColumn Clip<U>(U lower, U upper) {}
        public override BaseColumn Clone(BaseColumn mapIndices = null, bool invertMapIndices = false, long numberOfNullsToAppend = 0) {}
        public PrimitiveColumn<T> Clone(PrimitiveColumn<long> mapIndices = null, bool invertMapIndices = false) {}
        public PrimitiveColumn<T> Clone(PrimitiveColumn<int> mapIndices, bool invertMapIndices = false) {}
        public PrimitiveColumn<T> Clone(IEnumerable<long> mapIndices) {}
        public override BaseColumn CumulativeMax(bool inPlace = false) {}
        public override BaseColumn CumulativeMax(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public override BaseColumn CumulativeMin(bool inPlace = false) {}
        public override BaseColumn CumulativeMin(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public override BaseColumn CumulativeProduct(bool inPlace = false) {}
        public override BaseColumn CumulativeProduct(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public override BaseColumn CumulativeSum(bool inPlace = false) {}
        public override BaseColumn CumulativeSum(IEnumerable<long> rowIndices, bool inPlace = false) {}
        public override DataFrame Description() {}
        public override BaseColumn Divide(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Divide<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override PrimitiveColumn<bool> Equals(BaseColumn column) {}
        public override PrimitiveColumn<bool> Equals<U>(U value) where U : unmanaged {}
        public override BaseColumn FillNulls(object value, bool inPlace = false) {}
        public override BaseColumn Filter<U>(U lower, U upper) {}
        public IEnumerator<T?> GetEnumerator() {}
        [IteratorStateMachine()]
        public IEnumerable<ReadOnlyMemory<T>> GetReadOnlyDataBuffers() {}
        [IteratorStateMachine()]
        public IEnumerable<ReadOnlyMemory<byte>> GetReadOnlyNullBitMapBuffers() {}
        public override PrimitiveColumn<bool> GreaterThan(BaseColumn column) {}
        public override PrimitiveColumn<bool> GreaterThan<U>(U value) where U : unmanaged {}
        public override PrimitiveColumn<bool> GreaterThanOrEqual(BaseColumn column) {}
        public override PrimitiveColumn<bool> GreaterThanOrEqual<U>(U value) where U : unmanaged {}
        public override GroupBy GroupBy(int columnIndex, DataFrame parent) {}
        public override Dictionary<TKey, ICollection<long>> GroupColumnValues<TKey>() {}
        public override bool HasDescription() {}
        public override bool IsNumericColumn() {}
        public bool IsValid(long index) {}
        public override BaseColumn LeftShift(int value, bool inPlace = false) {}
        public override PrimitiveColumn<bool> LessThan(BaseColumn column) {}
        public override PrimitiveColumn<bool> LessThan<U>(U value) where U : unmanaged {}
        public override PrimitiveColumn<bool> LessThanOrEqual(BaseColumn column) {}
        public override PrimitiveColumn<bool> LessThanOrEqual<U>(U value) where U : unmanaged {}
        public override object Max() {}
        public override object Max(IEnumerable<long> rowIndices) {}
        public override double Mean() {}
        public override double Median() {}
        public override object Min() {}
        public override object Min(IEnumerable<long> rowIndices) {}
        public override BaseColumn Modulo(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Modulo<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override BaseColumn Multiply(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Multiply<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override PrimitiveColumn<bool> NotEquals(BaseColumn column) {}
        public override PrimitiveColumn<bool> NotEquals<U>(U value) where U : unmanaged {}
        public override BaseColumn Or(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Or<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override object Product() {}
        public override object Product(IEnumerable<long> rowIndices) {}
        public override void Resize(long length) {}
        public override BaseColumn RightShift(int value, bool inPlace = false) {}
        public override BaseColumn Round(bool inPlace = false) {}
        public override BaseColumn Sort(bool ascending = true) {}
        public override BaseColumn Subtract(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Subtract<U>(U value, bool inPlace = false) where U : unmanaged {}
        public override object Sum() {}
        public override object Sum(IEnumerable<long> rowIndices) {}
        public override DataFrame ValueCounts() {}
        public override BaseColumn Xor(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Xor<U>(U value, bool inPlace = false) where U : unmanaged {}
        protected internal override void AddDataViewColumn(Builder builder) {}
        protected internal override Array AsArrowArray(long startIndex, int numberOfRows) {}
        protected internal override Field Field() {}
        protected internal override Delegate GetDataViewGetter(DataViewRowCursor cursor) {}
        protected override IEnumerator GetEnumeratorCore() {}
        protected override object GetValue(long startIndex, int length) {}
        protected override object GetValue(long rowIndex) {}
        protected internal override int MaxRecordBatchLength(long startIndex) {}
        protected override void SetValue(long rowIndex, object value) {}
        public override string ToString() {}
    }
    [DefaultMember("Item")]
    public class StringColumn : BaseColumn, IEnumerable<string>, IEnumerable {
        public StringColumn(string name, long length) {}
        public StringColumn(string name, IEnumerable<string> values) {}
        public override long NullCount { get; }
        public string this[long rowIndex] { get; set; }
        public List<string> this[long startIndex, int length] { get; }
        public override BaseColumn Add(BaseColumn column, bool inPlace = false) {}
        public override BaseColumn Add<T>(T value, bool inPlace = false) where T : unmanaged {}
        public void Append(string value) {}
        public override BaseColumn Clip<U>(U lower, U upper) {}
        public override BaseColumn Clone(BaseColumn mapIndices = null, bool invertMapIndices = false, long numberOfNullsToAppend = 0) {}
        public override PrimitiveColumn<bool> Equals(BaseColumn column) {}
        public override PrimitiveColumn<bool> Equals<T>(T value) where T : unmanaged {}
        public override BaseColumn FillNulls(object value, bool inPlace = false) {}
        [IteratorStateMachine()]
        public IEnumerator<string> GetEnumerator() {}
        public override GroupBy GroupBy(int columnIndex, DataFrame parent) {}
        public override Dictionary<TKey, ICollection<long>> GroupColumnValues<TKey>() {}
        public override PrimitiveColumn<bool> NotEquals(BaseColumn column) {}
        public override PrimitiveColumn<bool> NotEquals<T>(T value) where T : unmanaged {}
        public override void Resize(long length) {}
        public override BaseColumn Sort(bool ascending = true) {}
        public override DataFrame ValueCounts() {}
        protected internal override void AddDataViewColumn(Builder builder) {}
        protected internal override Delegate GetDataViewGetter(DataViewRowCursor cursor) {}
        protected override IEnumerator GetEnumeratorCore() {}
        protected override object GetValue(long rowIndex) {}
        protected override object GetValue(long startIndex, int length) {}
        protected override void SetValue(long rowIndex, object value) {}
    }
    [GeneratedCode("System.Resources.Tools.StronglyTypedResourceBuilder", "16.0.0.0")]
    [DebuggerNonUserCode]
    [CompilerGenerated]
    public class Strings {
        public static string CannotResizeDown { get; }
        public static string ColumnIndexOutOfRange { get; }
        public static CultureInfo Culture { get; set; }
        public static string EmptyFile { get; }
        public static string ImmutableColumn { get; }
        public static string InvalidColumnName { get; }
        public static string LessColumnsThatExpected { get; }
        public static string MapIndicesExceedsColumnLenth { get; }
        public static string MismatchedColumnLengths { get; }
        public static string MismatchedRowCount { get; }
        public static string MismatchedValueType { get; }
        public static string MultipleMismatchedValueType { get; }
        public static string NumericColumnType { get; }
        public static ResourceManager ResourceManager { get; }
        public static string SpansMultipleBuffers { get; }
    }
    public enum JoinAlgorithm {
        Left = 0,
        Right = 1,
        FullOuter = 2,
        Inner = 3,
    }
}
```
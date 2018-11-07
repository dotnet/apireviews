# System.Text.Json.JsonReader

### Background

[JSON RFC](https://tools.ietf.org/html/rfc8259)

* We have several different input sources that we could receive JSON data from:
    - Pipe (async)
    - Stream (sync/async)
    - ReadOnlySequence (sync)
    - ReadOnlySpan (sync)
* A low-level ref struct type that caches the span being processed is
  necessarily required to meet our performance goals. Unfortunately, ref structs
  are unable to support async reads since they can't be boxed (which is
  inevitable across async/await boundaries).   
* We considered providing a low level span-based JsonReader and build
  ReadOnlySequence support on top in a separate type. However, a span-only
  reader can only be about 5% faster than one that supports both Span and
  Sequence and hence it is not worth adding separate types. This leads to a
  single type having some fields/properties that are not relevant for both
  inputs but provides a single layer for the bottom of the JSON stack.
* It is possible to build **synchronous** stream-based readers on top of the low
  level "span and sequence" JsonReader.
* It is possible, though not trivial, to build **asynchronous** stream and pipe
  support on top of the low level "span and sequence" JsonReader. It will
  require new internal static methods and careful construction of the state
  structs. Creating a new instance of the ref struct on every Read/ReadAsync is
  not a viable solution since it is too slow (2-3x slower). Stream would use the
  span-based ctor, while Pipe would use the sequence-based ctor.
* We plan to provide stream support by providing a higher-level convenience type
  that uses the `Utf8JsonReader` as the work horse to make stitching left over
  data easier for the caller.
* There are plans to ship a Pipe <-> Stream adapter to help with the
  interoperability with these types in general: https://github.com/dotnet/corefx/issues/27246

### Questions
1) What namespace and assembly should these types live in? Can it be a new
   assembly (it would have to be if we want to support older netstandard, say
   1.3, and provide a portable library).
   - YSharp already has a package named [System.Text.Json](https://www.nuget.org/packages/System.Text.Json/) published on nuget.
     We would need to replace it.
2) Should we pick the current API design or go with the alternative (B, below)?
3) Is the type name `Utf8JsonReader` acceptable?
4) ~Should the name of "Value" `ReadOnlySpan<byte>` property be different since
   `value` has a different meaning in terms of JSON semantics. RawValue?~
   Changed to `ValueSpan` and `ValueSequence`.
5) Position for tracking error is in bytes, not characters/UTF-8 code points.
   The performance overhead of tracking is too high for our target scenarios.
6) Should we provide a `GetValueAsNumber` API which boxes the value and returns
   an object containing the appropriately sized number? This is mainly for
   callers who don't know what number to expect. Any alternatives?
7) ~We do not currently support single token sizes larger than 2 GB and use an
   `ArrayPool<byte>` to stitch data that straddles segments. Another alternative
   is to expose the `ReadOnlySequence<byte>` slice as a property to the user and
   only allocate if they materialize it into a .NET type.~
   No longer relevant since we removed `Dispose` and use of `ArrayPool` and
   instead return `ReadOnlySequence<byte>`.
8) We assume the input data is valid UTF-8 and only check for invalid UTF-8 when
   the user converts to a .NET type (like string). We reject whatever input that
   is invalid according to the JSON RFC. Is that acceptable behaviour?
9) Should we expose `Consumed`/`SequencePosition` from the `Utf8JsonReader` or
   only from `JsonReaderState` or both (the caller uses these for slicing the
   input data to continue)?

### (B) Alternative Design - Class with nested ref struct type

```C#
namespace System.Text.Json {

    public class Utf8Json {
        public Utf8Json();
        public Utf8Json(JsonReaderState initialState);
        public JsonReaderState CurrentState { get; }
        public int MaxDepth { get; set; }
        public JsonReaderOptions Options { get; set; }
        public Utf8Json.Reader GetReader(ReadOnlySequence<byte> jsonData, bool isFinalBlock = true);
        public Utf8Json.Reader GetReader(ReadOnlySpan<byte> jsonData, bool isFinalBlock = true);
        public ref struct Reader {
            public long Consumed { get; }
            public int CurrentDepth { get; }
            public JsonTokenType TokenType { get; }
            public bool IsValueMultiSegment { get; }   // new
            public ReadOnlySpan<byte> ValueSpan { get; }
            public ReadOnlySequence<byte> ValueSequence { get; } // new
            public decimal GetValueAsDecimal();
            public double GetValueAsDouble();
            public int GetValueAsInt32();
            public long GetValueAsInt64();
            public object GetValueAsNumber();
            public float GetValueAsSingle();
            public string GetValueAsString();
            public bool Read();
            public void Skip();
        }
    }

}
```

#### Advantages
- The user doesn't have to deal with state.
- The `JsonReaderState` can be made internal, as long as we expose
  `SequencePosition` somehow.

#### Disadvantages
- Always allocates an object even if the user has the whole JSON payload and
  doesn't need state.
- If `JsonReaderState` is made internal, the user loses flexibility to start
  from any arbitrary point.
- The user has to deal with a nested type (how do we name these types?). The
  usage pattern starts to differ more from other libraries like Json.NET.
- Higher level types may want to explicitly manage state.
- ~10-25% slower for really small payloads, like HelloWorld (due to higher
  constructor cost/allocation). No difference beyond 1KB (~20+ fields).

### Sample Usage

#### Basic Reader Loop

```C#
// Input could be byte[], ReadOnlySpan<byte>, ReadOnlySequence<byte>
public static void JsonReaderLoop(byte[] data)
{
    var json = new Utf8JsonReader(data)
    {
        MaxDepth = 32,
        Options = JsonReaderOptions.SkipComments
    };

    while (json.Read())
    {
        JsonTokenType tokenType = json.TokenType;
        ReadOnlySpan<byte> value = json.IsValueMultiSegment ? json.ValueSequence.ToArray() : json.ValueSpan;
        switch (tokenType)
        {
            case JsonTokenType.StartObject:
            case JsonTokenType.EndObject:
            case JsonTokenType.StartArray:
            case JsonTokenType.EndArray:
                // Used for recursion, if building a dictionary/push or pop on stack/etc.
                break;
            case JsonTokenType.PropertyName:
                string key = json.GetValueAsString();
                break;
            case JsonTokenType.String:
                string value = json.GetValueAsString();
                break;
            case JsonTokenType.Number:
                // if type is known:
                int value = json.GetValueAsInt32();
                // else:
                object value = json.GetValueAsNumber();
                break;
            case JsonTokenType.True:
                break;
            case JsonTokenType.False:
                break;
            case JsonTokenType.Null:
                break;
            case JsonTokenType.Comment:
                // Only relevant if JsonReaderOptions.AllowComments selected
                break;
            default:
                throw new ArgumentException();
        }
    }
}
```

#### Read from Pipe with Async

```C#
public void ReadFromPipeUsingSequence()
{
    string actual = "";
    Task taskReader = Task.Run(async () =>
    {
        JsonReaderState state = default;
        string str = "";
        while (true)
        {
            ReadResult result = await _pipe.Reader.ReadAsync();
            ReadOnlySequence<byte> data = result.Buffer;
            bool isFinalBlock = result.IsCompleted;
            (state, str) = ProcessData(data, isFinalBlock, state);
            _pipe.Reader.AdvanceTo(state.Position);
            actual += str;
            if (isFinalBlock)
                break;

        }
    });
}

private static (JsonReaderState, string) ProcessData(ReadOnlySequence<byte> ros, bool isFinalBlock, JsonReaderState state = default)
{
    var builder = new StringBuilder();
    var json = new Utf8JsonReader(ros, isFinalBlock, state);
    while (json.Read())
    {
        switch (json.TokenType)
        {
            case JsonTokenType.PropertyName:
                builder.Append(json.GetValueAsString());
                break;
        }
    }
    return (json.CurrentState, builder.ToString());
}
```

#### Read from Pipe with Async - Alternative (based on B)

```C#
public void ReadFromPipeUsingSequence()
{
    string actual = "";
    Task taskReader = Task.Run(async () =>
    {
        var json = new Utf8Json();     // class object which contains the state instead of state struct
        string str = "";
        while (true)
        {
            ReadResult result = await _pipe.Reader.ReadAsync();
            ReadOnlySequence<byte> data = result.Buffer;
            bool isFinalBlock = result.IsCompleted;
            str = ProcessData(ref json, data, isFinalBlock);   // pass the object by ref
            _pipe.Reader.AdvanceTo(json.CurrentState.Position);   // get the SequencePosition from the object
            actual += str;
            if (isFinalBlock)
                break;

        }
    });
}

private static string ProcessData(ref Utf8Json json, ReadOnlySequence<byte> ros, bool isFinalBlock)
{
    var builder = new StringBuilder();
    Utf8Json.Reader reader = json.GetReader(ros, isFinalBlock);   // create the ref struct reader
    while (reader.Read())
    {
        switch (reader.TokenType)
        {
            case JsonTokenType.PropertyName:
                builder.Append(reader.GetValueAsString());
                break;
        }
    }

    return builder.ToString();
}
```

#### Dealing with multiple JSON objects inside a single payload

```C#
while (json.Read())
{
   JsonTokenType tokenType = json.TokenType;
    switch (tokenType)
    {
        case JsonTokenType.EndObject:
        case JsonTokenType.EndArray:
            if (json.CurrentDepth == 0)
            {
                json = new Utf8JsonReader(data.Slice((int)json.CurrentState.Consumed));
                // OR, if dealing with ReadOnlySequence
                json = new Utf8JsonReader(data.Slice(json.CurrentState.Position));
            }
            break;
            //...
    }
}
```

#### Read from Stream with Async

```C#
public void ReadFromStreamUsingSpan(Stream stream)
{
    byte[] _buffer = new byte[1_024];
    string actual = "";
    Task taskReader = Task.Run(async () =>
    {
        JsonReaderState state = default;
        int leftOver = 0;
        string str = "";
        while (true)
        {
            int dataLength = await stream.ReadAsync(_buffer, leftOver, _buffer.Length);
            bool isFinalBlock = dataLength == 0;
            (state, str) = ProcessData(_buffer, isFinalBlock, state);
            leftOver = dataLength - (int)state.Consumed;
            if (leftOver == dataLength)
                _buffer = GrowBuffer();
            else if (leftOver != 0)
            {
                if (leftOver < minimumSize)
                {
                    _buffer = GrowBuffer();
                }
                _buffer.AsSpan(dataLength - leftOver, leftOver).CopyTo(_buffer);
            }
            actual += str;
            if (isFinalBlock)
                break;

        }
    });
}
```
### Additional Questions & Notes

#### Do we have to provide ReadOnlySequence support?

- Yes, we do. Otherwise, the above sample to process the data using a span-based
  reader looks as follows and it becomes untenable.
- The actual json reader loop is hidden in all the boilerplate code that all
  users would have to write, which likely contains bugs.

```C#
// untested and likely buggy
private static (JsonReaderState, SequencePosition, string) ProcessDataSpan(ReadOnlySequence<byte> ros, bool isFinalBlock, JsonReaderState state = default)
{
    var builder = new StringBuilder();
    ReadOnlySpan<byte> leftOver = default;
    byte[] pooledArray = null;
    Utf8JsonReader json = default;
    long totalConsumed = 0;
    foreach (ReadOnlyMemory<byte> mem in ros)
    {
        ReadOnlySpan<byte> span = mem.Span;

        if (leftOver.Length > int.MaxValue - span.Length)
            throw new ArgumentOutOfRangeException("Current sequence segment size is too large to fit left over data from the previous segment into a 2 GB buffer.");

        // This is guaranteed to not overflow
        pooledArray = ArrayPool<byte>.Shared.Rent(span.Length + leftOver.Length);
        Span<byte> bufferSpan = pooledArray.AsSpan(0, leftOver.Length + span.Length);
        leftOver.CopyTo(bufferSpan);
        span.CopyTo(bufferSpan.Slice(leftOver.Length));

        json = new Utf8JsonReader(bufferSpan, isFinalBlock, state);

        while (json.Read())
        {
            switch (json.TokenType)
            {
                case JsonTokenType.PropertyName:
                    builder.Append(json.GetValueAsString());
                    break;
            }
        }

        if (json.Consumed < bufferSpan.Length)
        {
            leftOver = bufferSpan.Slice((int)json.Consumed);
        }
        else
        {
            leftOver = default;
        }
        totalConsumed += json.Consumed;
        if (pooledArray != null)    // TODO: Will this work if data spans more than two segments?
            ArrayPool<byte>.Shared.Return(pooledArray);

        state = json.CurrentState;
    }
    return (json.CurrentState, ros.GetPosition(totalConsumed), builder.ToString());
}
```

#### Is it possible to write a stream-based reader on top that supports async?

- Yes, but it can be tricky, especially one that is performant. It would rely on
  a static span-based JsonReader Read method (where you pass in the necessary
  states by ref).
- This can be a feature in the future if there is enough demand.
- See general skeleton below that uses a buffer borrowed from an array pool to
  hold the data. If the stream is seekable, you can reset the position to read
  the left over data (as an optimization).

```C#
public class JsonStreamReader : IDisposable
{
    private JsonReaderState _state;
    private InternalState _localState;
    private Stream _stream;
    private byte[] _pooledArray;
    private bool _isFinalBlock;
    private int _numberOfBytes;
    // ... other fields

    const int FirstSegmentSize = 1_024;

    public JsonTokenType TokenType => _state._tokenType;

    public JsonStreamReader(Stream jsonStream)
    {
        if (!jsonStream.CanRead)
            JsonThrowHelper.ThrowArgumentException("Stream must be readable.");

        _stream = jsonStream;
        _pooledArray = ArrayPool<byte>.Shared.Rent(FirstSegmentSize);
        _numberOfBytes = _stream.Read(_pooledArray, 0, FirstSegmentSize);
        _isFinalBlock = _numberOfBytes == 0;
        // ... other fields
    }

    public async Task<bool> ReadAsync()
    {
        bool result = Utf8JsonReader.Read(ref _state, data, ref _localState);
        if (!result && !_isFinalBlock)
            return await ReadNextAsync();
        else
            return result;
    }

    private async Task<bool> ReadNextAsync()
    {
        bool result = false;

        do
        {
            int leftOver = ...
            int amountToRead = StreamSegmentSize;

            if (leftOver > 0)
                if (_pooledArray.Length - leftOver < amountToRead)
                    ResizeBuffer(amountToRead, leftOver);

            _numberOfBytes = await _stream.ReadAsync(_pooledArray, leftOver, amountToRead);
            _isFinalBlock = _numberOfBytes == 0;

            result = Utf8JsonReader.Read(ref _state, data, ref _localState);
        } while (!result && !_isFinalBlock);

        return result;
    }

    public bool Read()
    {
        // Internal static method on Utf8JsonReader
        bool result = Utf8JsonReader.Read(ref _state, data, ref _localState);
        if (!result && !_isFinalBlock)
            return ReadNext();
        else
            return result;
    }

    private bool ReadNext()
    {
        bool result = false;

        do
        {
            int leftOver = ...
            int amountToRead = StreamSegmentSize;

            if (leftOver > 0)
                if (_pooledArray.Length - leftOver < amountToRead)
                    ResizeBuffer(amountToRead, leftOver);

            _numberOfBytes = _stream.Read(_pooledArray, leftOver, amountToRead);
            _isFinalBlock = _numberOfBytes == 0;

            result = Utf8JsonReader.Read(ref _state, data, ref _localState);
        } while (!result && !_isFinalBlock);
            
        return result;
    }

    public void Dispose()
    {
        if (_pooledArray != null)
        {
            ArrayPool<byte>.Shared.Return(_pooledArray);
            _pooledArray = null;
        }
    }
}
```
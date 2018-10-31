# System.Text.Json.JsonReader

```C#
namespace System.Text.Json {

    public ref struct Utf8JsonReader {
        // ctors
        public Utf8JsonReader(in ReadOnlySequence<byte> jsonData);
        public Utf8JsonReader(in ReadOnlySequence<byte> jsonData, bool isFinalBlock, JsonReaderState state = default(JsonReaderState));
        public Utf8JsonReader(ReadOnlySpan<byte> jsonData);
        public Utf8JsonReader(ReadOnlySpan<byte> jsonData, bool isFinalBlock, JsonReaderState state = default(JsonReaderState));

        // settable properties
        public int MaxDepth { get; set; }    // default = 64
        public JsonReaderOptions Options { get; set; }    // default = JsonReaderOptions.Default, i.e. strict RFC compliance

        public long Consumed { get; }
        public int CurrentDepth { get; }
        public JsonReaderState CurrentState { get; }
        public bool IsValueMultiSegment { get; }   // new
        public JsonTokenType TokenType { get; }
        public ReadOnlySpan<byte> ValueSpan { get; }
        public ReadOnlySequence<byte> ValueSequence { get; } // new
        
        public bool Read();
        public void Skip();

        // Conversion to .NET types
        public decimal GetValueAsDecimal();
        public double GetValueAsDouble();
        public int GetValueAsInt32();
        public long GetValueAsInt64();
        public object GetValueAsNumber();
        public float GetValueAsSingle();
        public string GetValueAsString();
    }

    public class JsonReaderException : Exception {
        public JsonReaderException(string message, long lineNumber, long bytePosition);
        public long BytePosition { get; }
        public long LineNumber { get; }
    }

    // [Flags]
    public enum JsonReaderOptions {
        AllowComments = 1,
        Default = 0,
        SkipComments = 3,
    }

    public struct JsonReaderState {
        public long Consumed { get; }
        public SequencePosition Position { get; }
    }

    public enum JsonTokenType : byte {
        Comment = (byte)11,
        EndArray = (byte)4,
        EndObject = (byte)2,
        False = (byte)9,
        None = (byte)0,
        Null = (byte)10,
        Number = (byte)7,
        PropertyName = (byte)5,
        StartArray = (byte)3,
        StartObject = (byte)1,
        String = (byte)6,
        True = (byte)8,
    }

}
```
```C#
namespace System.Text.Json {
    // Factory
    public static class Utf8JsonWriter {
        public static Utf8JsonWriter<TBufferWriter> Create<TBufferWriter>(TBufferWriter bufferWriter, bool prettyPrint = false) where TBufferWriter : IBufferWriter<byte>;
        // Other potential factory methods:
        public static Utf8JsonWriter<TBufferWriter> Create<TBufferWriter>(Stream stream, bool prettyPrint = false);
        // Can't support writing to a span directly.
    }
    public ref struct Utf8JsonWriter<TBufferWriter> where TBufferWriter : IBufferWriter<byte> {
        // ctors
        public Utf8JsonWriter(TBufferWriter bufferWriter, bool prettyPrint = false);

        // Advances the IBufferWriter
        public void Flush();
        
        public void WriteStartArray();
        public void WriteEndArray();
        public void WriteStartArray(ReadOnlySpan<byte> name);
        // public void WriteStartArray(string name);
        
        public void WriteStartObject();
        public void WriteEndObject(); // Throws FormatException - should it throw JsonWriterException?
        public void WriteStartObject(ReadOnlySpan<byte> name);
        // public void WriteStartObject(string name);

        // Writing all supported .NET types - which ones should we support?
        // These APIs exist mainly for performance.
        public void WriteArray(ReadOnlySpan<byte> name, ReadOnlySpan<int> values);
        public void WriteArray(ReadOnlySpan<byte> name, ReadOnlySpan<bool> values);
        public void WriteArray(ReadOnlySpan<byte> name, ReadOnlySpan<string> values);
        public void WriteArray(ReadOnlySpan<byte> name, ReadOnlySpan<ReadOnlyMemory<byte>> utf8StringBytes);
        // ... etc.

        // string-based overloads that transcode to UTF-8?
        public void WriteArray(string name, ReadOnlySpan<int> values);
        public void WriteArray(string name, ReadOnlySpan<bool> values);
        public void WriteArray(string name, ReadOnlySpan<string> values);
        public void WriteArray(string name, ReadOnlySpan<ReadOnlyMemory<byte>> utf8StringBytes);
        // ... etc.

        // Writing all supported .NET types - which ones should we support?
        public void WriteNameValue(ReadOnlySpan<byte> name, bool value);
        public void WriteNameValue(ReadOnlySpan<byte> name, DateTime value);
        public void WriteNameValue(ReadOnlySpan<byte> name, DateTimeOffset value);
        public void WriteNameValue(ReadOnlySpan<byte> name, Guid value);
        public void WriteNameValue(ReadOnlySpan<byte> name, long value);
        public void WriteNameValue(ReadOnlySpan<byte> name, ulong value);
        public void WriteNameValue(ReadOnlySpan<byte> name, string value);
        public void WriteNameValue(ReadOnlySpan<byte> name, ReadOnlySpan<byte> utf8Bytes);
        public void WriteNameValueAsBase64(ReadOnlySpan<byte> name, ReadOnlySpan<byte> utf8Bytes);
        // Provide escape overloads that allow skipping escaping the name and values where relevant?
        // ... etc.

        // string-based overloads that transcode to UTF-8?
        public void WriteNameValue(string name, bool value);
        public void WriteNameValue(string name, DateTime value);
        public void WriteNameValue(string name, DateTimeOffset value);
        public void WriteNameValue(string name, Guid value);
        public void WriteNameValue(string name, long value);
        public void WriteNameValue(string name, ulong value);
        public void WriteNameValue(string name, string value);
        public void WriteNameValue(string name, ReadOnlySpan<byte> utf8StringBytes);
        public void WriteNameValueRaw(string name, ReadOnlySpan<byte> utf8Bytes);
        public void WriteNameValueAsBase64(string name, ReadOnlySpan<byte> utf8Bytes);
        // ... etc.

        public void WriteNull(ReadOnlySpan<byte> name);
        public void WriteNull();
     
        // Matching the capabilities of WriteNameValue - which ones should we support?
        public void WriteValue(bool value);
        public void WriteValue(DateTime value);
        public void WriteValue(DateTimeOffset value);
        public void WriteValue(Guid value);
        public void WriteValue(long value);
        public void WriteValue(ulong value);
        public void WriteValue(string value);
        // public void WriteValue(string value, bool escape);
        public void WriteValue(ReadOnlySpan<byte> utf8StringBytes);
        // public void WriteValue(ReadOnlySpan<byte> utf8Bytes, bool escape);
        public void WriteValueRaw(ReadOnlySpan<byte> utf8Bytes);
        public void WriteValueAsBase64(ReadOnlySpan<byte> utf8Bytes);
        // ... etc.

        public void Write(Utf8JsonReader reader, bool writeChildren = false);
    }
}
```
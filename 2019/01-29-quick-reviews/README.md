# Quick Reviews 1/29/2019

## System.Reflection.Emit.Label should implement IEquatable

**Approved** | [#corefx/32959](https://github.com/dotnet/corefx/issues/32959) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h0m0s)

## Exception to throw when interface dispatch is ambiguous

**Approved** | [#corefx/34124](https://github.com/dotnet/corefx/issues/34124#issuecomment-458653253) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h2m50s)

Looks good. A few things:

* We should have a parameterless constructor
* We should add the serialization contructor
* We should add the inner exception constructor
* We should mark the exception serializable
* We should seal the type
## Consider moving several HWIntrinsic instance methods to be extension methods

**Approved** | [#corefx/34902](https://github.com/dotnet/corefx/issues/34902) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h22m45s)

## Take another look at the `COMISS` and `UCOMISS` hardware intrinisics

**Approved** | [#corefx/34881](https://github.com/dotnet/corefx/issues/34881#issuecomment-458659597) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h29m52s)

A derivative of @benaadams suggestion @tannergooding suggested (where `Xxx` is `Equals`, `LessThan` etc):

* CompareOrderedScalarXxx
* CompareUnorderdScalarXxx
## SafeBuffer Span<T> methods

**Approved** | [#corefx/33279](https://github.com/dotnet/corefx/issues/33279) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h40m32s)

## Add string overloads to DbDataReader.Get*()

**Approved** | [#corefx/31595](https://github.com/dotnet/corefx/issues/31595#issuecomment-458677042) | [Video](https://www.youtube.com/watch?v=3KCS0GW6lUU&t=0h46m2s)

We concluded that we want to leave these ones off for now. The reason being that ADO.NET really wants to expose a `ValueTask<T>` as opposed to `Task<T>` but we'll need to revisit how we'd plumb this through:

```C#
public Task<T> GetFieldValueAsync<T>(string name); // Forwards to CancellationToken 
public Task<T> GetFieldValueAsync<T>(string name, CancellationToken cancellationToken);
public Task<bool> IsDBNullAsync(string name); // Forwards to CancellationToken 
public Task<bool> IsDBNullAsync(string name, CancellationToken cancellationToken);
```

This one is a mistake:

```C#
public int GetProviderSpecificValues(object[] values);
```

Which leaves us with this:

```C#
public partial class DbDataReader
{        
    public bool GetBoolean(string name);
    public byte GetByte(string name);
    public long GetBytes(string name, long dataOffset, byte[] buffer, int bufferOffset, int length);
    public char GetChar(string name);
    public long GetChars(string name, long dataOffset, char[] buffer, int bufferOffset, int length);
    public DbDataReader GetData(string name);
    public string GetDataTypeName(string name);
    public DateTime GetDateTime(string name);
    public DateTime GetDbDataReader(string name);
    public decimal GetDecimal(string name);
    public double GetDouble(string name);
    public Type GetFieldType(string name);
    public T GetFieldValue<T>(string name);
    public float GetFloat(string name);
    public Guid GetGuid(string name);
    public short GetInt16(string name);
    public int GetInt32(string name);
    public long GetInt64(string name);
    public Type GetProviderSpecificFieldType(string name);
    public object GetProviderSpecificValue(string name);
    public Stream GetStream(string name);
    public string GetString(string name);
    public TextReader GetTextReader(string name);
    public object GetValue(string name);
    public bool IsDBNull(string name);
}
```

# IBufferWriter

Status: **Approved with feedback** | 
[API Ref](https://github.com/dotnet/corefx/issues/34894) |
[Video](https://www.youtube.com/watch?v=MXeuyhQ6g0k)

## Notes

* The constructor shouldn't be hardcoded to a specific buffer size
    - Just make it two constructors where the one with no argument is documented
      as "some default size"
* The `CopyTo` and `CopyToAsync` methods aren't very convenient to use, assuming
  the goal is to get the data to a stream. The reason being that the consumer is
  responsible to call them periodically to avoid buffering the entire data.
  Also, as the methods are currently implemented `CopyTo` really moves the data
  (i.e. clears the data from the `ArrayBufferWriter`.
* Should we return `ValueTask` as opposed to `Task`
* Should we seal the class?
* `CopyTo` should be renamed to `MoveTo` or `CopyToAndClear` and
  `CopyToAndClearAsync`
* Should we have a type like `StreamBufferWriter` that implements
  `IBufferWritter` and accepts a `Stream` to write to? That type could use
  `ArrayBufferWriter` and handle the periodic flushing.
* `BytesCommitted` and `BytesWritten` are confusing
    - `BytesCommitted` -> `TotalBytesWritten`
    - `BytesWritten` -> `BytesWritten`
* We don't expose the size of the buffer. We should add a property called
  `Capacity`, of type `int`.
* We should consider exposing `BytesAvailable` and doc it as returning
  `Capacity - BytesWritten`.
* Consider implementing the `IBufferWriter` APIs explicitly

### Follow-up

* We should take another look at `BufferWriter`. We were hoping that the JSON
  APIs would drive its requirements, but we ended up bypassing it entirely.
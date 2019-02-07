# Adapter for System.IO.Pipelines

Status: **Approved with Feedback** | 
[API Ref](https://github.com/dotnet/corefx/issues/27246) |
[Video](https://www.youtube.com/watch?v=9TSP4awoHuI)

# Notes

* The proposal is to throw an exception when a anybody uses synchronous
  (blocking) APIs on the returned `Stream`.
    - The reason being that the blocking APIs don't perform as well.
    - We should reverse the default so that synchronous APIs work by default but
      advanced customers can opt out so they can a "slap on the wrist" when the
      stack accidentally uses synchronous APIs.
    - Actually, we decided to get rid of the argument so that we can make the
      method `AsStream` idempotent.
* Extension methods vs. virtual methods
    - We'd make `AsStream()` virtual methods on `PipeReader` and `PipeWriter`.
    - Since we don't have default interface methods yet, we should not expose
      an extension method to avoid behavior inconsistencies.
* `AsStream()` should return the same instance when called multiple times on the
  same instance (will be idempotent)
* We decided against making them structs so that it's consistent `PipeOptions`
* Remove constants for defaults
* Renames
    - `MinimumSegmentSize` -> `BufferSize`
* `PipeReader.CopyToAsync()`
    - Should be virtual
    - Should return `Task`
* `PipeWriter.CopyFromAsync()`
    - Should be virtual
    - Should return `Task`
    - Should be `protected internal` because the name is very unusual
* Consider overriding `Stream.CopyToAsync` to handle the case where the caller
  passed in a stream that was produced from a `PipeWriter`. This way, you can
  by-pass the naïve `Stream`-based copy and go straight to the pipeline.
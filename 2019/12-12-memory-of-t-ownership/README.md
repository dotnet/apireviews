# Memory<T> ownership

Status: **Needs work** |
[Video](https://youtu.be/VRTCAkk7wKU)

## Existing guidance

* [@bartonjs](https://github.com/bartonjs) did a summary of how the guidelines
  treat arrays vs. span vs. memories
* Synchronous, non-Set, method input parameter:
    - Array: Concurrent access is not expected, method should interact with the
      array and return without saving a reference.
    - Span: Same as array
    - Memory: Same as array
* Synchronous Set method input parameter:
    - Array: Default behavior: make a copy
    - Span: Make a copy (on ref struct: Maybe don’t make a copy)
    - **Memory: Default behavior: Don’t make a copy**
* Asynchronous, non-Set, method input parameter:
    - Array: Concurrent access is not expected, method will save a reference to
      the array while the operation is in progress.  When the operation
      finishes, no further actions on the array happen.
    - Span: Violation of design guidelines.  (Fails to compile with async
      keyword, has to make a copy otherwise).
    - Memory: Same as array.
* Asynchronous Set method: I don’t know of any.  This confuses me.
* Constructor input parameter
    - Array: Default behavior: Make a copy
    - Span: Make a copy (**on ref struct: Don’t make a copy**)
    - **Memory: Default behavior: Don’t make a copy**
* Property return value
    - Array: Default behavior: Return a copy
    - Span: **Ill defined**, except for a ref struct.
    - Memory:
        + ReadOnlyMemory: **Don’t make a copy**, the buffer is associated with
          the instance.
        + Memory: **Not sure what this means**.
* Method return value
    - Array: Caller owns the array.
    - Span: Ill defined unless the span was an input argument.
    - Memory: Same as Span.

## Proposed guidance

* When an member returns a `Memory<T>` you can access that as long as the
  instanced they invoked the member on is alive/not disposed.
* Prefer back a `Memory<T>` by an array (instead of native memory).

## Next steps

* [@bartonjs](https://github.com/bartonjs) will propose new guidelines
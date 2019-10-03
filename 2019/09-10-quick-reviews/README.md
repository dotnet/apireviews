# Quick Reviews 9/10/2019

## Review System.Buffers.SequenceReader<T> proposals

**Approved** | [#corefx/40962](https://github.com/dotnet/corefx/issues/40962#issuecomment-530060857) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=0h0m0s)

* We should rename the `skip` parameters to `offset`
    - We don't use `skip` anywhere else and it kind of implies a side effect
    - `offset` implies relative to *something* and for a reader it makes to be relative to current position
* We should the `count` parameter to `length`
* We should remove the overloads of TryPeek that take two arguments and whose first argument is `count` -- because it conflicts `TryPeek(int skip, out T value)`
* Arguments referring to position and lengths should be typed as `long`
* We should add `readonly` annotations
* It's sad that `TryPeek()` that returns a span might have to allocate if the span crosses buffers but there is no way around that.
* FDG: we need to consider what we do for out parameters where the out type is the same as the input type as that allows you modify the `this` while it's running (ask Levi for details).
* `public void SetPosition(SequencePosition position);` needs more work as there are a lot of concerns (like O(n) complexity).

```C#
namespace System.Buffers
{
    public ref struct SequenceReader<T>
    {
        // Optimized API to position the reader at the end of the sequence (much faster than what users can write)
        public void AdvanceToEnd();

        // Pairs with existing Span<T> UnreadSpan;
        public readonly ReadOnlySequence<T> UnreadSequence { get; }

        // Overloads for TryRead that allow reading out a given count rather than to some delimiter (as with existing
        // API span out will slice if it can or allocate and copy if it has to).
        public bool TryRead(int length, out ReadOnlySpan<T> value);
        public bool TryRead(long length, out ReadOnlySequence<T> value);

        // Peeking out T, while skipping. This is more performant than users can write (avoids rewinding).
        public readonly bool TryPeek(long offset, out T value);

        // Equivalent "Peek" versions. They need a offset as peeking doesn't advance the reader and rewinding is super expensive.
        public readonly bool TryPeek(long offset, int length, out ReadOnlySpan<T> value);
        public readonly bool TryPeek(long offset, long length, out ReadOnlySequence<T> value);
 
        // Pairs with existing TryCopyTo(Span<T> destination), which does not advance the reader (neither does this)
        public readonly bool TryCopyTo(long offset, Span<T> destination);
   }
}
```

## SequenceReader<T>.AdvanceTo(SequencePosition)

**Needs Work** | [#corefx/40843](https://github.com/dotnet/corefx/issues/40843#issuecomment-530066043) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h15m19s)

This was reviewed in #40962.  Concerns about [performance and usability](https://github.com/dotnet/corefx/issues/40962#issuecomment-530060857) pretty much as described so putting back to "needs work".
## Allow custom format strings for boolean values

**Needs Work** | [#corefx/8126](https://github.com/dotnet/corefx/issues/8126#issuecomment-530066335) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h25m49s)

Honestly, while I agree with that being useful, it's trivial for people to define helper methods:

```C#
public static string ToState(this bool value, string trueValue, string falseValue)
    => value ? trueValue : falseValue;
public static string ToState(this bool? value, string trueValue, string falseValue, string nullValue)
    => value is null ? nullValue : value.Value ? trueValue : nullValue;
```

For `bool`:

```C#
string.Format($"The kettle is {kettle.ToState("on", "off")}");
```

For `bool?`:

```C#
string.Format($"The kettle is {kettle.ToState("on", "off", "Unknown")}");
```

Using the formatting APIs seems way too complex for this case.
## SequenceReader<T>.ReadToEnd / AdvanceToEnd

**Approved** | [#corefx/37156](https://github.com/dotnet/corefx/issues/37156#issuecomment-530067301) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h29m56s)

This was approved in #40962 as:

``` C#
namespace System.Buffers
{
    public ref struct SequenceReader<T>
    {
        // Optimized API to position the reader at the end of the sequence (much faster than what users can write)
        public void AdvanceToEnd();

        // Pairs with existing Span<T> UnreadSpan;
        public readonly ReadOnlySequence<T> UnreadSequence { get; }
    }
}
```

Here are the rough implementations:

``` C#
        /// <summary>
        /// The unread portion of the <see cref="Sequence"/>.
        /// </summary>
        public readonly ReadOnlySequence<T> UnreadSequence
        {
            get => Sequence.Slice(Position);
        }
```


``` C#
        public void AdvanceToEnd()
        {
            if (_moreData)
            {
                Consumed = Length;
                CurrentSpan = default;
                CurrentSpanIndex = 0;
                _currentPosition = Sequence.End;
                _moreData = false;
            }
        }
```

Marking as up-for-grabs to fully implement/test.
## SequenceReader<T>.TryRead overloads to read a specified number of elements

**Approved** | [#corefx/40870](https://github.com/dotnet/corefx/issues/40870#issuecomment-530068389) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h32m38s)

Approved: https://github.com/dotnet/corefx/issues/40962#issuecomment-530060857

``` C#
        public bool TryRead(int length, out ReadOnlySpan<T> value);
        public bool TryRead(long length, out ReadOnlySequence<T> value);
```
## SequenceReader<T>.TryPeek overloads for look ahead

**Approved** | [#corefx/40845](https://github.com/dotnet/corefx/issues/40845#issuecomment-530069306) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h35m2s)

Approved: #40962 (comment):

``` C#
        public readonly bool TryPeek(long offset, out T value);
```
## SequenceReader<T>.TryPeek overloads to read a specified number of elements from any position

**Approved** | [#corefx/40871](https://github.com/dotnet/corefx/issues/40871#issuecomment-530070064) | [Video](https://www.youtube.com/watch?v=Vf-Gj0h8Uws&t=1h37m29s)

Approved: #40962 (comment):

``` C#
        // Equivalent "Peek" versions. They need a offset as peeking doesn't advance the reader and rewinding is super expensive.
        public readonly bool TryPeek(long offset, int length, out ReadOnlySpan<T> value);
        public readonly bool TryPeek(long offset, long length, out ReadOnlySequence<T> value);
 
        // Pairs with existing TryCopyTo(Span<T> destination), which does not advance the reader (neither does this)
        public readonly bool TryCopyTo(long offset, Span<T> destination);
```

# System.Buffers.SequenceReader

```C#
namespace System.Buffers
{
    public ref struct SequenceReader<T> where T : unmanaged, IEquatable<T>
    {
        public SequenceReader(in ReadOnlySequence<T> buffer);

        // State
        public bool End { get; }

        // This is awkward- we can know that we are in the last segment quickly
        // but we can't express that there are no more spans without costly checks
        // as if we have more segments they may potentially be empty.
        public bool AtLastSegment { get; }

        public void Advance(long count);
        public void Rewind(long count);

        public long Consumed { get; }                // The total advanced
        public ReadOnlySpan<T> CurrentSpan { get; }
        public int CurrentSpanIndex { get; }
        public ReadOnlySpan<T> UnreadSpan { get; }

        public SequencePosition Position { get; }
        public ReadOnlySequence<T> Sequence { get; } // The original ReadOnlySequence<T>

        // Peek does not advance
        public bool TryPeek(out T value);
        public readonly ReadOnlySpan<T> Peek(Span<T> copyBuffer);

        // Read will advance if successful
        public bool TryRead(out T value);

        public bool IsNext(T value, bool advancePast = false);
        public readonly bool IsNext(ReadOnlySpan<T> next, bool advancePast = false);
        public bool AreAnyNext(T value0, T value1, bool advancePast = false);
        public bool AreAnyNext(T value0, T value1, T value2, bool advancePast = false);
        public bool AreAnyNext(T value0, T value1, T value2, T value3, bool advancePast = false);
        public readonly bool AreAnyNext(ReadOnlySpan<T> values, bool advancePast = false);

        public long AdvancePast(T value);
        public long AdvancePastAny(T value0, T value1);
        public long AdvancePastAny(T value0, T value1, T value2);
        public long AdvancePastAny(T value0, T value1, T value2, T value3);
        public long AdvancePastAny(ReadOnlySpan<T> values);
        public bool TryAdvanceTo(T delimiter, bool advancePastDelimiter = true);
        public bool TryAdvanceToAny(ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);

        public bool TryReadTo(out ReadOnlySpan<T> span, T delimiter, bool advancePastDelimiter = true);
        public bool TryReadTo(out ReadOnlySpan<T> span, T delimiter, T delimiterEscape, bool advancePastDelimiter = true);
        public bool TryReadTo(out ReadOnlySpan<T> span, ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
        public bool TryReadToAny(out ReadOnlySpan<T> span, ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);

        public bool TryReadTo(out ReadOnlySequence<T> sequence, T delimiter, bool advancePastDelimiter = true);
        public bool TryReadTo(out ReadOnlySequence<T> sequence, T delimiter, T delimiterEscape, bool advancePastDelimiter = true);
        public bool TryReadTo(out ReadOnlySequence<T> sequence, ReadOnlySpan<T> delimiter, bool advancePastDelimiter = true);
        public bool TryReadToAny(out ReadOnlySequence<T> sequence, ReadOnlySpan<T> delimiters, bool advancePastDelimiter = true);
    }
```
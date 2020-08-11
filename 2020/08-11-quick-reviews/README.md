# Quick Reviews 08/11/2020

## Add ability to perform zero byte reads in StreamPipeReader

**NeedsWork** | [#runtime/37539](https://github.com/dotnet/runtime/issues/37539#issuecomment-672127133) | [Video](https://www.youtube.com/watch?v=pgAFuCF6Kp8&t=0h0m0s)

* Zero byte reads are technically a violation of the `Stream.Read()` contract, but granted many streams probably support it by accident.
* We should have an API on `Stream` instead
* We should validate the `SslStream`, `SocketListenerStream`, `NetworkStream`, and other compression streams
* This should be investigated and the API proposal should be updated

```C#
namespace System.IO.Pipelines
{
    public class StreamPipeReaderOptions
    {
        public bool WaitForData { get; set; }
    }
}
namespace System.IO
{
    public partial class Stream
    {
        public virtual Task WaitForDataAsync() => return Task.CompletedTask;
    }
}
```

## Make enum RegexParseError and RegexParseException public

**Approved** | [#runtime/38872](https://github.com/dotnet/runtime/issues/38872#issuecomment-672176154) | [Video](https://www.youtube.com/watch?v=pgAFuCF6Kp8&t=0h36m29s)

* The type is currently serializable and people do serialize the exception by catching `ArgumentException` (or `Exception`). To preserve cross-framework deserialization, like when someone deserializes this exception on .NET Framework (or earlier versions of .NET Core), we need to keep serializing as `ArgumentException`, not as `RegexParseException`.
* This also means that `Error` and `Offset` are lost when crossing framework boundaries. Hence, we should also improve the message.
* We don't want to have a separate public type from the internal one, but we should make sure that all enum members are actually used by the implementation/are reachable. If there are unused values, we should remove them.
* We should probably preserve the current ordering, if only to discourage the urge to make future additions alphabetically sorted.

```C#
namespace System.Text.RegularExpressions
{
    [Serializable]
    public sealed class RegexParseException : ArgumentException
    {
        public RegexParseException(RegexParseError error, int offset);
        private RegexParseException(SerializationInfo info, StreamingContext context)
        {
            // It means someone modified the payload.
            throw new NotImplementedException();
        }
        public override void GetObjectData(SerializationInfo info, StreamingContext context)
        {
            // We'll serialize as an instance of ArgumentException
        }
        public RegexParseError Error { get; }
        public int Offset { get; }
    }
    public enum RegexParseError
    {
        Unknown,
        AlternationHasComment,
        AlternationHasMalformedCondition,
        AlternationHasMalformedReference,
        AlternationHasNamedCapture,
        AlternationHasTooManyConditions,
        AlternationHasUndefinedReference,
        CaptureGroupNameInvalid,
        CaptureGroupOfZero,
        ExclusionGroupNotLast,
        InsufficientClosingParentheses,
        InsufficientOpeningParentheses,
        InsufficientOrInvalidHexDigits,
        InvalidGroupingConstruct,
        InvalidUnicodePropertyEscape,
        MalformedNamedReference,
        MalformedUnicodePropertyEscape,
        MissingControlCharacter,
        NestedQuantifiersNotParenthesized,
        QuantifierAfterNothing,
        QuantifierOrCaptureGroupOutOfRange,
        ReversedCharacterRange,
        ReversedQuantifierRange,
        ShorthandClassInCharacterRange,
        UndefinedNamedReference,
        UndefinedNumberedReference,
        UnescapedEndingBackslash,
        UnrecognizedControlCharacter,
        UnrecognizedEscape,
        UnrecognizedUnicodeProperty,
        UnterminatedBracket,
        UnterminatedComment
    }
}
```


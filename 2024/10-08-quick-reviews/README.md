# API Review 10/08/2024

## Allow collections expression for other collections

**NeedsWork** | [#runtime/108457](https://github.com/dotnet/runtime/issues/108457#issuecomment-2400438835) | [Video](https://www.youtube.com/watch?v=Zs5PckgBVjE&t=0h0m0s)


* Rather than putting these on CollectionExtensions, we should just add public ctors that accept ReadOnlySpan
* That might introduce ambiguity for F#, VB, and/or other .NET languages (COBOL.NET, anyone?), we should double-check.
    * Definitely a problem for arrays, both IEnumerable and implicitly convertible to ReadOnlySpan
    * Also a problem for strings
    * And any other type with an implicit conversion operator to ReadOnlySpan and is also IEnumerable.

```C#
namespace System.Collections.Generic;

#if NECESSARY

public partial class CollectionExtensions
{
    [EditorBrowsable(EditorBrowsableState.Never)]
    public static LinkedList<T> CreateLinkedList<T>(params ReadOnlySpan<T> values);

    [EditorBrowsable(EditorBrowsableState.Never)]
    public static Stack<T> CreateStack<T>(params ReadOnlySpan<T> values);

    [EditorBrowsable(EditorBrowsableState.Never)]
    public static Queue<T> CreateQueue<T>(params ReadOnlySpan<T> values);
}

[CollectionBuilder(typeof(CollectionExtensions), "CreateLinkedList")]
public partial class LinkedList<T>;

[CollectionBuilder(typeof(CollectionExtensions), "CreateStack")]
public partial class Stack<T>;

[CollectionBuilder(typeof(CollectionExtensions), "CreateQueue")]
public partial class Queue<T>;

#else

public partial class Queue<T>
{
    public Queue<T>(ReadOnlySpan<T> items);
}

public partial class Stack<T>
{
    public Stack<T>(ReadOnlySpan<T> items);
}

public partial class LinkedList<T>
{
    public LinkedList<T>(ReadOnlySpan<T> items);
}

#endif
```

## [System.Text.Json] Expose a setting disallowing duplicate JSON properties

**Approved** | [#runtime/108521](https://github.com/dotnet/runtime/issues/108521#issuecomment-2400572601) | [Video](https://www.youtube.com/watch?v=Zs5PckgBVjE&t=0h21m49s)


* We cycled through many names for the enum members, and in the end decided on just a boolean

```C#
namespace System.Text.Json
{
    public partial struct JsonDocumentOptions
    {
        public bool AllowDuplicateProperties { get; set; } = true;
    }
}

namespace System.Text.Json.Serialization
{
    public partial class JsonSerializerOptions
    {
        public bool AllowDuplicateProperties { get; set; } = true;

    }

    public partial class JsonSourceGenerationsOptionsAttribute
    {
        public bool AllowDuplicateProperties { get; set; } = true;
    }
}
```

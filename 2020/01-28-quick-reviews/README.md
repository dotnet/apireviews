# Quick Reviews 1/28/2020

## Add StringContent ctor providing tighter control over charset

**Approved** | [#corefx/7864](https://github.com/dotnet/corefx/issues/7864#issuecomment-579398933) | [Video](https://www.youtube.com/watch?v=keHv3W-2-Q4&t=0h0m0s)

We believe the correct API is this:

```C#
public StringContent(string content, MediaTypeHeaderValue mediaType)
```

## Is there a function to copy directories?

**Rejected** | [#corefx/15708](https://github.com/dotnet/corefx/issues/15708#issuecomment-579405459) | [Video](https://www.youtube.com/watch?v=keHv3W-2-Q4&t=0h23m57s)

We generally have avoided IO APIs with complex policy (such as, cancellation, error handling, recovery, permissions etc). The API shape would work, but we're concerned about the complexity in implementation and don't think it's worth it.

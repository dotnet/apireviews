# Quick Reviews 10/10/2017

## Productizing APIs for {ReadOnly}Memory<T> and friends

**Approved** | [#corefx/23862](https://github.com/dotnet/corefx/issues/23862#issuecomment-335543258) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h0m0s)

@ahsonkhan mentioned he's not ready for a review yet.
## Add protected {Unmanaged}MemoryStream.Read/WriteSpan method

**Needs Work** | [#corefx/24039](https://github.com/dotnet/corefx/issues/24039#issuecomment-335544053) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h2m25s)

@KrzysztofCwalina suggested to generalize this beyond these two types; @stephentoub said he has to think about to do this and whether it's even possible.
## Add ProcessStartInfo.ArgumentList

**Approved** | [#corefx/23592](https://github.com/dotnet/corefx/issues/23592#issuecomment-335552652) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h4m53s)

The API looks good, and it's a good problem to solve. However, we don't like the idea that we have to roundtrip data between the two different representations because it means we have to both, parse and combine per operating system and get the semantics right. Instead, we'd prefer a model like this:

```C#
partial class ProcessStartInfo
{
    // Exists
    string Arguments { get; set; }   

    // new
    IReadOnlyCollection<string> ArgumentList { get; }
}
```

`Start` would verify that not both `!string.IsNullOrEmpty(Arguments)` and `ArgumentList.Count > 1` are true, i.e. the caller may either use `Arguments` or `ArgumentList`, but not both.

[edit @danmosemsft changed Collection typo to IREadOnlyCollection]
## Add Array.Sort<T>(T[], int, int, Comparison<T>) overload

**Rejected** | [#corefx/24115](https://github.com/dotnet/corefx/issues/24115#issuecomment-335553699) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h36m32s)

Looks good to us, but we should probably add the same API to `List<T>`
## Change ReadOnlySpan indexer to return ref readonly

**Approved** | [#corefx/24105](https://github.com/dotnet/corefx/issues/24105) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h40m21s)

## StreamWriter .ctor pooling overloads

**Needs Work** | [#corefx/23874](https://github.com/dotnet/corefx/issues/23874#issuecomment-335568547) | [Video](https://www.youtube.com/watch?v=b96co3sNzNI&t=0h41m0s)

Couple of thoughts:

* It seems opting in wouldn't get huge wins for existing code because everyone has to change their code. At the same time, making it opt-out might be too breaking. The question is whether we can find a way to get 90% of the wins without requiring calling a new API while making it 90% safe (of course, on .NET Framework pooling would be quirked so that apps only get it when retargeting against the latest version).
* We generally have to think about more aggressive buffer pooling
* We'd prefer the bool over the `pool` because it allows us to change the implementation


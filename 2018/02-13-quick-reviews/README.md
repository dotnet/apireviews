# Quick Reviews 2/13/2018

## Add time-constant equals and other utility methods to System.Security

**Approved** | [#corefx/10749](https://github.com/dotnet/corefx/issues/10749#issuecomment-365354366) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=0h0m0s)

Looks good as updated in [the last comment](https://github.com/dotnet/corefx/issues/10749#issuecomment-365027340).
## Helper class for dealing with native shared libraries and function pointers

**Approved** | [#corefx/17135](https://github.com/dotnet/corefx/issues/17135#issuecomment-365365224) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=0h4m50s)

@dotMorten 

The `name` argument for `TryLoad` allows relative paths but behavior depends on the value of `paths`.
## Add API for reading/writing PDB Checksum Debug Directory entry

**Approved** | [#corefx/26935](https://github.com/dotnet/corefx/issues/26935#issuecomment-365370670) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=0h40m44s)

We just took a look. Looks good as proposed. Questions that came up:

* Should the `ImmtuableArray<byte>` be a `ReadOnlySpan<byte>`. We assumed the answer to be no, as S.R.M doesn't depend on span yet and you need to ship downlevel. Once you spannify S.R.M we can still add overloads.
* `IdStartOffset` are 32 bit enough? Assumption is yes, but hey :-)
## Cleanup after removal of MemoryExtensions As* api.

**Approved** | [#corefx/26894](https://github.com/dotnet/corefx/issues/26894#issuecomment-365381215) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=0h59m53s)

Looks good as proposed. Regarding the open issues at the end:

5. **Limit constructors to the longest (most flexible) overload.**. Makes sense, we want the .ctors to be largely plumbing. However, we don't want to lose perf due to excessive argument validation. We'll add overloads as needed for perf, but not usability.

6. **Provide `AsReadOnly()` methods on r/w/ slice types.**. We agreed to not adding these methods.

7. **Skip `ReadOnly` from conversion method names if the source type is already read-only. For example, string can be converted only to `ReadOnlySpan<char>` and so the As<slice_type> method should be called `AsSpan`, not `AsReadOnlySpan`**. Makes sense.
## Add overloads to enumeration APIs that take FindOptions flags

**Rejected** | [#corefx/25875](https://github.com/dotnet/corefx/issues/25875#issuecomment-365383129) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=1h35m56s)

Superseded by #25873
## Add file enumeration extensibility points

**Approved** | [#corefx/25873](https://github.com/dotnet/corefx/issues/25873#issuecomment-365388259) | [Video](https://www.youtube.com/watch?v=4hIcv01O3aQ&t=1h42m24s)

Looks good. Comments:

* `MatchType`, `MatchCasing`, `EnumerationOptions` should live in `System.IO`. Please submit a proposal with the updated `System.IO` APIs
* `EnumerationOptions.IgnoreInaccessible` should be `true` by default
* `EnumerationOptions.AttributesToSkip` should include system and hidden files.

# .NET API PROPOSAL

## Summary

Resolving relative paths via `Path.GetFullPath` currently depends on the current
working directory (CWD) for the process
(`SetCurrentDirectory`/`GetCurrentDirectory`). Using the CWD for path resolution
is not recommended as it is process wide and as such, not thread friendly.

Additionally, long paths (260+ characters) cannot be used with
`SetCurrentDirectory`.

To facilitate writing more robust and long path friendly code we should provide
an API that assists in resolving relative paths. The proposal is to add an
overload to `Path.GetFullPath` that takes a path and a base path.

We should also tweak behavior to defer to Windows `GetFullPathName` on Windows.

## Details

Returns the absolute path for the specified path string using the specified base
path if relative.

### Syntax

```C#
public static string GetFullPath(
    string basePath,
    string path
)
public static string GetFullPath(
    string path
)
```

### Parameters

Parameter | Description
----------|---------------------------------------------------------------------
basePath  | The path to resolve relative against if path is relative.
path      | The file or directory for which to obtain absolute path information.

### Return Value

The fully qualified location of path, such as `C:\MyFile.txt`.

### Exceptions

Exception             | Description
----------------------|---------------------------------------------------------
ArgumentException     | `path` is zero-length string, contains only white space [Windows only], or contains one or more of the invalid characters defined in `GetInvalidPathChars`
                      | **-or-**
                      | `basePath` is zero-length string, contains only white space [Windows only], or contains one or more of the invalid characters defined in `GetInvalidPathChars`
                      | **-or-**
                      | `basePath` is not an absolute path
                      | **-or-**
                      | The system could not retrieve the absolute path
SecurityException     | The caller does not have the required permissions.
ArgumentNullException | path or `basePath` is null
NotSupportedException | path or `basePath` contains a colon (`:`) that is not part of a volume identifier (for example, `c:\`). [Windows only]
PathTooLongException  | path, `basePath`, or the resolved path is over the maximum path length [Windows 32,767 characters] [Unix varies]

### Remarks

The .NET Framework does not support direct access to physical disks through
paths that are device names, such as `\\.\PHYSICALDRIVE0 `.

The absolute path includes all information required to locate a file or
directory on a system.

The file or directory specified by path or `basePath` are not required to exist.

However, if path does exist, the caller must have permission to obtain path
information for path. Note that unlike most members of the `Path` class, this
method accesses the file system.

Relative paths use the current directory to evaluate the absolute path. The
overload `basePath` uses the specified `basePath` to evaluate the absolute path.

If you pass in a short file name it is expanded to a long file name.

If a path contains no significant characters it is invalid unless it contains
one or more `.`` characters followed by any number of spaces, then it will be
parsed as either `.` or `..`.

#### New Content

If path has a volume and is rooted (for example `C:\A\MyFile.txt`), `basePath`
isn’t needed to calculate the absolute path. If path has a volume and is not
rooted (for example, `C:foo.txt`) it will use basePath only if it also specifies
the same volume (for example, `C:\bar\`)- otherwise the working directory is
assumed to be the root of the volume (in this example, `C:\`). In all other
cases the basePath will be used to figure out the absolute path.

If `basePath` is an extended path (begins with `\\?\`), the returned path will
also have the extended prefix. Paths with `\\?\` will only be evaluated for
current and parent directory segments.

#### Unix Details

Single and double dot path segments will be evaluated and removed.

## Design and other considerations

This `basePath` API overload is intended to be functionally similar to combining
`SetCurrentDirectory` and `GetFullPath` without actually setting the current
directory.

This API should not actually touch the disk/network. It is intended to generate
a well-formed absolute path (a path that is unambiguous). (With the notable
exception of resolving short, or alternate, names.)

Did not put with `Path.Combine()` as it is a simple string concat. We need to put
information on both API documentation pages about utilizing this API to get
proper analysis of relative second paths. Something like:

> If the paths being combined contain relative segments (`.`, `..`) consider
> using `Path.GetFullPath(string, string)`. If you want relative segments to not
> be considered, use this API, otherwise use `Path.GetFullPath`.

### Windows specific

Windows (`GetFullPathName()`) should be deferred to for correctness and
collapsing single and double dot segments. We will no longer try to evaluate
segment correctness and will only perform additional post-checks for
compatibility with prior iterations. (See API Behavior Comparison below)

We will continue to only call GetLongPathName if the path has a tilde
(`xxxxxx~x`), even though this isn’t correct. We will push this check to the end
of the validation and walk backwards through the string to optimize the number
of expansions we attempt.

We will use `GetFullPathName()` on Windows with a concatenated basePath to deal
with the overload.

To avoid creating directories with trailing spaces or periods we will ensure
that these are trimmed from our `CreateDirectory` API (as it is recursive) if
the original path was not using extended syntax.

Using extended syntax (paths that begin with `\\?\` or `\\?\UNC`) is a contract
with .NET that says you own correctness and absolute state. Significant
performance improvements and access to hereto unavailable paths can be achieved
(including alternate streams). When we have extended syntax paths  we will not
call `GetFullPathName`. We will also not check for invalid characters, or
collapse relative segments, or try to find alternate (short) path names.

For extended syntax, relative segments (`.`, `..`) will only be evaluated from
path. Relative segments will only be modified in the basePath if the path
consumes them. For example, `\\?\C:\A\..\B\.\` and `..\..\C.\D.txt` will be
evaluated as `\\?\C:\A\..\C.\D.txt`. No other processing will be done with path
outside of relative segments. Spaces and dots will otherwise not be touched.

### Unix specific

Unix should validate there are no invalid characters (`null`). Unix should not
do any of the additional checks that Windows does. We should not use any APIs
that evaluate symlinks.

## Windows API Behavior Comparison

### GetFullPathName

DOS device names are converted into their `\\.\` equivalent syntax and returned.
(Devices can have alternate streams `:` and extensions, these are considered.)

The given name is examined for a known root:

* UNC: `\\Server\Share`
* Local or Root Local: `\\.\` `\\?\`
* Drive Absolute: `C:\`
* Drive Relative: `C:`
* Rooted: `\`
* Relative: all others

Relative or rooted paths (`C:A`, `\A`, `A`) are combined with the current
directory or process environment block’s current directory if it doesn’t exist.
For drive relative that do not match the current directory, the hidden
environment variable is checked (`=X:`) for a value, the root of the drive is
used if it isn’t found.

Dots and spaces are removed the trailing end of the path (past the final slash).
Internal segments have a few different behaviors:

* Single dot segments are taken along with the preceding separator
* Double dot segments are taken along with the preceding segment, if not at the
  root
* Trailing single dot is taken (so `...` or `A..` will be left alone)

If the path started as drive absolute it will always have a trailing separator
when returned to avoid changing the identity to drive relative. (For example:
`C:\..` becomes `C:\`, while `\\Server\Share\..` becomes `\\Server\Share`.

One important takeaway is that behavior with extended syntax (`\\?\`) does not
give expected matching rooting behavior. Stripping the extended prefix would
give identical results.

### System.IO.GetFullPath

.NET tries to mimic the native behavior with some additional filters
Unfortunately the behavior isn’t exactly aligned Here are some of the
differences:

* Any segment that starts with a space will throw `ArgumentException`
* Segments with just dots and spaces are always removed
* Trailing spaces are removed from segments
* `\\Server\Share\.` throws `ArgumentException`
* Illegal characters are checked (inadvertently blocking access to alternate
  data streams)
* Short names are expanded for each part of the path (incorrectly as it assumes
  they must have `~`)
* `http:` and `file:` are explicitly rejected
* `\\?\GlobalRoot` is explicitly rejected
* Throws `ArgumentException` if there isn’t a full Server\Share for UNCs

Ironically .NET calls `GetFullPathName` after making all of its similar changes.

`GetDirectoryName` and `GetPathRoot` skip full checks, which are:

* Trimming trailing spaces
* Checking invalid characters
* Calling GetFullPathName
* Checking for `http:` and `file:`

## Other Thoughts

Should we always check basePath even if we don’t need it? Throwing always will
be predictable but the perf hit will not be negligible.

Paths should not have to exist. Windows has APIs that evaluate these as well
(`GetFinalPathNameByHandle`). We should introduce a `GetFinalPath()` API to
`System.IO.Path` and use `realpath()` on Unix.

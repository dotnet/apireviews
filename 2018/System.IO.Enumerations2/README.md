# System.IO.Enumerations

Status: **Approved with Feedback** | 
[API](System.IO.FileSystem.md)

## Notes

- `EnumerationOptions`
    - Check whether we can use a different word than `Inaccessible`
    - `Recurse` should be `EnumerateSubdirectories`
    - `Default` and `default` don't necessarily have the same values, which
      might be confusing.
    - We should make it a class, it solves most problems. And for allocations,
      it should be a non-issue as the options themselves are static and don't
      need to be created per enumeration.

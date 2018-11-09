# UTF8 String (Session 2)

Status: **Needs more work** | 
[API](https://github.com/dotnet/corefx/issues/30503) |

## UnicodeScalar

* `String.GetScalarAt(): UnicodeScalar`
    - Should be a static on `UnicodeScalar`, as it's tricky
* `String.GetScalars()` should be an instance `String`
* `ToUtfXxx()` should use the `Try`-pattern and should be named `TryEncode`

* `UnicodeScalar`
    - The tuples should be an a struct called `UnicodeScalarPosition`
    - This struct should have two deconstruct methods
        - `(UnicodeScalar, int StartIndex)`
        - `(UnicodeScalar, int StartIndex, int Length)`
        - We don't expose a nullable `UnicodeScalar`, instead there is a bool on the struct `WasReplaced`
        - People that use deconstruction should can use `GetScalarAt()` -- they know the start index

* Replace `UnicodeScalar` with `Rune`
* All types will go into `System.Text`
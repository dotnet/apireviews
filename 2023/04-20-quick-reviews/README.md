# API Review 04/20/2023

## Add first class System.Threading.Lock type

**Approved** | [#runtime/34812](https://github.com/dotnet/runtime/issues/34812#issuecomment-1516694095) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=0h0m0s)

We discussed the proposed change to make `Lock : IDisposable`, and believe that is not the right answer at this time.

If the general patterns of using the type result in too many finalizations we can try to mitigate that in implementation, or make it IDisposable in a future release.

```C#
namespace System.Threading;

public sealed class Lock
{
    public Scope EnterScope();

    public void Enter();
    public void Exit();
    public bool TryEnter();
    public bool TryEnter(TimeSpan timeout);
    public bool TryEnter(int millisecondsTimeout);

    public bool IsHeldByCurrentThread { get; }

    public struct Scope : IDisposable
    {
        public void Dispose();
    }
}
```
## Ascii.Equals

**Approved** | [#runtime/84887](https://github.com/dotnet/runtime/issues/84887#issuecomment-1516723569) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=0h19m8s)

* We added `<char>,<byte>` overloads so we weren't opinionated on the order of UTF-16 and ASCII inputs.
* We discussed collapsing the two methods groups into one with a `bool` parameter (or `StringComparison`),
but decided that it was better as-proposed.
* We've not yet had a public method named `EqualsIgnoreCase`, but we were all sort of surprised by that, and believe it's the best name.

```C#
namespace System.Text;

public static partial class Ascii
{
    /// <summary>
    /// Determines whether the provided buffers contain equal ASCII characters.
    /// </summary>
    /// <param name="left">The buffer to compare with <paramref name="right" />.</param>
    /// <param name="right">The buffer to compare with <paramref name="left" />.</param>
    /// <returns><see langword="true" /> if the corresponding elements in <paramref name="left" /> and <paramref name="right" /> were equal and ASCII. <see langword="false" /> otherwise.</returns>
    /// <remarks>If both buffers contain equal, but non-ASCII characters, the method returns <see langword="false" />.</remarks>
    public static bool Equals(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
    public static bool Equals(ReadOnlySpan<byte> left, ReadOnlySpan<char> right);
    public static bool Equals(ReadOnlySpan<char> left, ReadOnlySpan<byte> right);
    public static bool Equals(ReadOnlySpan<char> left, ReadOnlySpan<char> right);
    
    /// <summary>
    /// Determines whether the provided buffers contain equal ASCII characters, ignoring case considerations.
    /// </summary>
    /// <param name="left">The buffer to compare with <paramref name="right" />.</param>
    /// <param name="right">The buffer to compare with <paramref name="left" />.</param>
    /// <returns><see langword="true" /> if the corresponding elements in <paramref name="left" /> and <paramref name="right" /> were equal ignoring case considerations and ASCII. <see langword="false" /> otherwise.</returns>
    /// <remarks>If both buffers contain equal, but non-ASCII characters, the method returns <see langword="false" />.</remarks>
    public static bool EqualsIgnoreCase(ReadOnlySpan<byte> left, ReadOnlySpan<byte> right);
    public static bool EqualsIgnoreCase(ReadOnlySpan<byte> left, ReadOnlySpan<char> right);
    public static bool EqualsIgnoreCase(ReadOnlySpan<char> left, ReadOnlySpan<byte> right);
    public static bool EqualsIgnoreCase(ReadOnlySpan<char> left, ReadOnlySpan<char> right);
}
```
## Parallel.ForAsync

**Approved** | [#runtime/59019](https://github.com/dotnet/runtime/issues/59019#issuecomment-1516740016) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=0h43m51s)

We brought this back to discuss collapsing the int/long variants into a single generic representation, with a result of we do feel like one generic version is the better approach.

The discussion says that IBinaryInteger is the correct restriction, so we're going with that.

```C#
namespace System.Threading.Tasks
{
    public static partial class Parallel
    {
        public static Task ForAsync<T>(T fromInclusive, T toExclusive, Func<T, CancellationToken, ValueTask> body) where T : notnull, IBinaryInteger<T>;
        public static Task ForAsync<T>(T fromInclusive, T toExclusive, CancellationToken cancellationToken, Func<T, CancellationToken, ValueTask> body) where T : notnull, IBinaryInteger<T>;
        public static Task ForAsync<T>(T fromInclusive, T toExclusive, ParallelOptions parallelOptions, Func<T, CancellationToken, ValueTask> body) where T : notnull, IBinaryInteger<T>;
    }
}
```
## Introduce TimeProvider extension method to create a CTS to be cancelled after a specified time period.

**Approved** | [#runtime/85080](https://github.com/dotnet/runtime/issues/85080#issuecomment-1516751126) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=0h57m0s)

On the basis that this is only provided in the `Microsoft.Bcl.TimeProvider` compat-package, and that `TimeProviderTaskExtensions` already exists, this looks good as proposed.


```C#
namespace System.Threading.Tasks
{
    public static partial class TimeProviderTaskExtensions
    {

        /// <summary>Initializes a new instance of the <see cref="CancellationTokenSource"/> class that will be canceled after the specified <see cref="TimeSpan"/>. </summary>
        /// <param name="timeProvider">The <see cref="TimeProvider"/> with which to interpret the <paramref name="delay"/>. </param>
        /// <param name="delay">The time interval to wait before canceling this <see cref="CancellationTokenSource"/>. </param>
        /// <exception cref="ArgumentOutOfRangeException"><paramref name="delay"/>'s <see cref="TimeSpan.TotalMilliseconds"/> is less than -1 or greater than <see cref="uint.MaxValue"/> - 1. </exception>
        /// <remarks>
        /// The countdown for the delay starts during the call to the constructor.  When the delay expires,
        /// the constructed <see cref="CancellationTokenSource"/> is canceled if it has
        /// not been canceled already. Subsequent calls to CancelAfter will reset the delay for the constructed
        /// <see cref="CancellationTokenSource"/> if it has not been canceled already.
        /// </remarks>
        public static CancellationTokenSource CreateCancellationTokenSource(this TimeProvider timeProvider, TimeSpan delay) ;
    }
}
```
## Encoding.TryGetBytes/Chars

**Approved** | [#runtime/84425](https://github.com/dotnet/runtime/issues/84425#issuecomment-1516764848) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=1h5m40s)

Corrected the proposal to make these methods `virtual`.  Otherwise, looks good as proposed.

We discussed the names `source`/`destination` versus the `chars`/`bytes` pattern, but since the existing non-Try method has `chars`/`bytes`, we went for local consistency.

```C#
namespace System.Text;

public abstract partial class Encoding
{
    public virtual bool TryGetBytes(ReadOnlySpan<char> chars, Span<byte> bytes, out int bytesWritten);
    public virtual bool TryGetChars(ReadOnlySpan<byte> bytes, Span<char> chars, out int charsWritten);
}
```
## Provide "Left Handed" overloads of matrix create functions

**Approved** | [#runtime/80332](https://github.com/dotnet/runtime/issues/80332#issuecomment-1516780972) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=1h19m11s)

We expanded "LH" to "LeftHanded", otherwise looks good as proposed.

```C#
namespace System.Numerics;

public partial struct Matrix4x4
{
    public static Matrix4x4 CreateLookAtLeftHanded(Vector3 cameraPosition, Vector3 cameraTarget, Vector3 cameraUpVector);

    public static Matrix4x4 CreateLookToLeftHanded(Vector3 cameraPosition, Vector3 cameraDirection, Vector3 cameraUpVector);

    public static Matrix4x4 CreateOrthographicLeftHanded(float width, float height, float zNearPlane, float zFarPlane);
    public static Matrix4x4 CreateOrthographicOffCenterLeftHanded(float left, float right, float bottom, float top, float zNearPlane, float zFarPlane);

    public static Matrix4x4 CreatePerspectiveLeftHanded(float width, float height, float nearPlaneDistance, float farPlaneDistance);
    public static Matrix4x4 CreatePerspectiveFieldOfViewLeftHanded(float fieldOfView, float aspectRatio, float nearPlaneDistance, float farPlaneDistance);
    public static Matrix4x4 CreatePerspectiveOffCenterLeftHanded(float left, float right, float bottom, float top, float nearPlaneDistance, float farPlaneDistance);
    
    public static Matrix4x4 CreateViewportLeftHanded(float x, float y, float width, float height, float minDepth, float maxDepth)
}
```
## Guid.TryWriteBytes(..., out int bytesWritten)

**Approved** | [#runtime/30940](https://github.com/dotnet/runtime/issues/30940#issuecomment-1516794663) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=1h32m14s)

Looks good as proposed.

Marking the non-out variant with EditorBrowsable.Never, but we decided to defer that to a later discussion (or never, as the case may be).  It may be more related to adding a non-try `int WriteBytes(destination)` than this overload.

```C#
namespace System
{
    public partial struct Guid
    {
        public bool TryWriteBytes(Span<byte> destination, out int bytesWritten);
    }
}
```
## [Analyzer] BitConverter.ToString(bytes).Replace("-", "") to Convert.ToHexString

**Approved** | [#runtime/81796](https://github.com/dotnet/runtime/issues/81796#issuecomment-1516803485) | [Video](https://www.youtube.com/watch?v=m9OgkGRMDqk&t=1h44m26s)

The analyzer seems like goodness.

Category: Performance
Severity: Info

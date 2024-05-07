# API Review 01/10/2022

## Introduce IFeatureCollection.GetRequiredFeature

**Approved** | [#aspnetcore/25471](https://github.com/dotnet/aspnetcore/issues/25471#issuecomment-1009232097)

API review:

```diff
+ namespace Microsoft.AspNetCore.Http.Features;
+ 
+ /// <summary>
+ /// Extension methods for getting feature from <see cref="IFeatureCollection"/>
+ /// </summary>
+ public static class FeatureCollectionExtensions
+ {
+     /// <summary>
+     /// Retrives the requested feature from the collection.
+     /// Throws an <see cref="InvalidOperationException"/> if the feature is not present.
+     /// </summary>
+     /// <param name="featureCollection">The <see cref="IFeatureCollection"/>.</param>
+     /// <typeparam name="TFeature">The feature key.</typeparam>
+     /// <returns>The requested feature.</returns>
+     public static TFeature GetRequiredFeature<TFeature>(this IFeatureCollection featureCollection) where TFeature : notnull
+ 
+     /// <summary>
+     /// Retrives the requested feature from the collection.
+     /// Throws an <see cref="InvalidOperationException"/> if the feature is not present.
+     /// </summary>
+     /// <param name="featureCollection">feature collection</param>
+     /// <param name="key">The feature key.</param>
+     /// <returns>The requested feature.</returns>
+     public static object GetRequiredFeature(this IFeatureCollection featureCollection, Type key);
+ }
```

Usage:

```C#
using Microsoft.AspNetCore.Http.Features;

...
var coolFeature = httpContext.Features.GetRequiredFeature<ICoolFeature>();
```
## Add Results.Stream overload that takes a Func<Stream, Task>

**Approved** | [#aspnetcore/39383](https://github.com/dotnet/aspnetcore/issues/39383#issuecomment-1009261849)

API review:

We decided to add a few more overloads during review.

```diff
public class Results
{

+ public IResult Stream(
+     Func<Stream, Task> streamWriterCallback, 
+     string? contentType = null,
+     string? fileDownloadName = null,
+     DateTimeOffset? lastModified = null,
+     EntityTagHeaderValue? entityTag = null,
+     bool enableRangeProcessing = false);

+ public IResult Stream(
+     PipeReader pipeReader, 
+     string? contentType = null,
+     string? fileDownloadName = null,
+     DateTimeOffset? lastModified = null,
+     EntityTagHeaderValue? entityTag = null,
+     bool enableRangeProcessing = false);


+ public IResult Stream(
+     Func<PipeWriter, Task> pipeWriterCallback, 
+     string? contentType = null,
+     string? fileDownloadName = null,
+     DateTimeOffset? lastModified = null,
+     EntityTagHeaderValue? entityTag = null,
+     bool enableRangeProcessing = false);


+ public IResult Bytes(
+     ReadOnlyMemory<byte> byteBuffer, 
+     string? contentType = null,
+     string? fileDownloadName = null,
+     bool enableRangeProcessing = false,
+     DateTimeOffset? lastModified = null,
+     EntityTagHeaderValue? entityTag = null);
}
```

```diff
public class ControllerBase
{
+  public PushFileStreamResult File(
+      Func<Stream, Task> streamWriterCallback, 
+      string? contentType = null,
+      string? fileDownloadName = null,
+      DateTimeOffset? lastModified = null,
+      EntityTagHeaderValue? entityTag = null,
+      bool enableRangeProcessing = false);

// This needs to add a remark that the byte buffer
// should not use a pooled source since it's lifetime
// isn't tied to the action result execution.
// Accessing FileContentResult.FileContents should copy the results from the memory
+ public FileContentResult File(
+     ReadOnlyMemory<byte> fileContents, 
+     string? contentType = null,
+     string? fileDownloadName = null,
+     bool enableRangeProcessing = false,
+     DateTimeOffset? lastModified = null,
+     EntityTagHeaderValue? entityTag = null);

+ public class PushFileStreamResult : FileResult
+ {
+    public Func<Stream, Task> StreamWriterCallback { get; set; }
+ }
}
```

We should also add PipeReader / PipeWriter overloads for controllers. @rafikiassumani-msft could we have someone propose the API for this and bring it to review?

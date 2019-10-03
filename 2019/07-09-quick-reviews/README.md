# Quick Reviews 7/9/2019

## Add async versions of PipeReader and PipeWriter completion

**Approved** | [#corefx/39241](https://github.com/dotnet/corefx/issues/39241) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h0m0s)

## Deprecate PipeWriter.OnReaderCompleted

**Approved** | [#corefx/38362](https://github.com/dotnet/corefx/issues/38362#issuecomment-509732308) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h10m42s)

Decision:

* Make the implementation virtual.
* Make the implementation a no-op
* Mark the API as `[Obsolete]`
* Apply the same to `PipeWriter.OnWriterCompleted`

@davidfowl, what should the message be for `PipeReader.OnReaderCompleted` and `PipeWriter.OnWriterCompleted`?
## PipeOptions, StreamPipeReaderOptions, and StreamPipeWriterOptions should not hardcode default sizes.

**Approved** | [#corefx/37984](https://github.com/dotnet/corefx/issues/37984#issuecomment-509735688) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h16m46s)

Conclusion:

* Nullable would have been nice, but we can't change the type for some of the APIs (because they shipped) and we prefer consistency, so we're going with the sentinel value of `-1`.
## Make JsonConverterFactory.CreateConverter public.

**Approved** | [#corefx/39307](https://github.com/dotnet/corefx/issues/39307#issuecomment-509740229) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h26m33s)

* Making the API public makes sense.
* However, it seems if the scenario is composition, we should pass in the `JsonSerializerOptions` so that someone implementing a converter for a generic type, can ask the options for getting a converter for its type arguments.
     - The same logic would also apply to `CanConvert()`, however, it seems the design seems to be that the answer can be deferred.
     - In fact, it seems `GetConverter()` could too because the logic can be deferred to `JsonConverter<T>.Read` and `JsonConverter<T>.Write`
     - If that's the case the API can be approved.

@JeremyKuhne please take a look.
    
## Make Microsoft.Win32.SystemEvents static

**Approved** | [#corefx/39308](https://github.com/dotnet/corefx/issues/39308#issuecomment-509744182) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h41m37s)

(You can use this issue for any class where all public APIs are static, all constructors are private, and the type is sealed)
## Add AppContext.ApplicationConfig

**Approved** | [#corefx/36897](https://github.com/dotnet/corefx/issues/36897#issuecomment-509749369) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=0h48m38s)

Conclusion

* Exposing the API on `AppDomainSetup` is fine, but we shouldn't expose the property on `AppContext`
* We may want to expose the backing store for .NET Core's `runtimeconfig.json` (and that should go on `AppContext`) but it would point to a different file, obviously.
## ServiceBase.IsRunningInWindowsService

**Needs Work** | [#corefx/36842](https://github.com/dotnet/corefx/issues/36842) | [Video](https://www.youtube.com/watch?v=EERe5C9qty0&t=1h5m59s)


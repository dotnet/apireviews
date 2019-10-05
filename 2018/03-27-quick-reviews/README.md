# Quick Reviews 3/27/2018

## SNI API for SslStream

**Approved** | [#corefx/24553](https://github.com/dotnet/corefx/issues/24553#issuecomment-376614353) | [Video](https://www.youtube.com/watch?v=cwX05hKkp8s&t=-7h-8m-18s)

@krwq, please make sure that deciding on the cert via a callback is viable, i.e. you don't have to pass a mapping of host name to cert up front on any of the platforms we support with .NET Core. Otherwise, the API shape won't work.

> When caller returns `null` certificate we will throw `AuthenticationException`

Alternative design would be that `null` means use the default certificate (`SslServerAuthenticationOptions.ServerCertificate`). That might be more convenient without forcing the delegate to allocate a closure.

@davidfowl @halter73, preferences?


# Quick Reviews 5/14/2019

## Add missing async methods in System.Data.Common and implement IAsyncDisposable

**Approved** | [#corefx/35012](https://github.com/dotnet/corefx/issues/35012#issuecomment-492336595) | [Video](https://www.youtube.com/watch?v=cP2E0gSe8is&t=0h0m0s)

* To follow the existing pattern, `BeginConnection` should be non-virtual and we should add a single BeginDbConnectionAsync that takes an isolation level
* Consider for reach API whether you want `Task<T>` or `ValueTask<T>`

## Add missing feature detection properties to DbProviderFactory

**Approved** | [#corefx/35564](https://github.com/dotnet/corefx/issues/35564) | [Video](https://www.youtube.com/watch?v=cP2E0gSe8is&t=0h31m15s)

## New System.Data.Common batching API

**Approved** | [#corefx/35135](https://github.com/dotnet/corefx/issues/35135#issuecomment-492347155) | [Video](https://www.youtube.com/watch?v=cP2E0gSe8is&t=0h36m11s)

* Have you considered making `DbBatch` extend `DbCommand`?
* `IList<DbBatchCommand>` should probably be `DbBatchCollection : Collection<DbBatch>`
* Validate this against multiple providers; shipping a new concept is best validated by having multiple implementations.

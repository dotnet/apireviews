# API Review 03/07/2024

## Add RegisterSyntaxNodeStartAction to allow analyzers to immediately bail out from analyzing a file prior to processing nodes.

**NeedsWork** | [#roslyn/72053](https://github.com/dotnet/roslyn/issues/72053#issuecomment-1984571740)

### API Review

* Naming needs to be changed
    * RegisterSemanticModelStartAction implies much more functionality - being able to register operation or symbol actions
    * RegisterSyntaxTreeStartAction seems more appropriate
* Could the AnalyzerDriver actually know about this and just not run the analyzers?
    * Are most analyzers going to define their options in a way that can be applied per-file?
* Did prototype, saved a few percent CPU gains. Not huge, not nothing.
* Will this bloat the state tracking for analyzers?
    * Manish looked into this, it didn't seem bad.

**Conclusion**: Needs work. We'll come back after looking at whether the analyzer driver itself can handle the optimization
## Add `ArgumentKind.ParamCollection`

**Approved** | [#roslyn/72222](https://github.com/dotnet/roslyn/issues/72222#issuecomment-1984572270)

### API Review

* No comments

**Conclusion**: Ship it
## Add `SpecialType` entry for `ReadOnlySpan`

**NeedsWork** | [#roslyn/72102](https://github.com/dotnet/roslyn/issues/72102#issuecomment-1984572814)

### API Review

* Potential for confusion around being in SpecialType when ROS doesn't come from corlib
    * API has documentation emphasizing this
    * Definitely some angst in the team about this
* Could we make a SpecialTypeInternal?
    * Would need to intercept a public request for the internal constant for ReadOnlySpan to avoid returning it
    * Would allow the compiler to make changes like this that shouldn't be public in the future.

**Conclusion**: Needs work. Let's investigate a SpecialTypeInternal API instead of adding to SpecialType

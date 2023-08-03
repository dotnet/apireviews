# API Review 08/03/2023

## Make ProgrammaticSuppressionInfo public

**NeedsWork** | [#roslyn/69000](https://github.com/dotnet/roslyn/issues/69000#issuecomment-1664646444)

### API Review

* Can we move `Suppressions` to `SuppressionInfo` instead and not have `ProgrammaticSuppressionInfo` at all?
    * Let's see if we can consolidate into `SuppressionInfo`. Passing a `Compilation` might be a problem?

**Conclusion**: See if we can do that move, and send email to the api review alias to follow up with the results. If it can't be done, we can make sure we like the names proposed and then approve over email.
## Add async `SourceText` APIs

**Rejected** | [#roslyn/61489](https://github.com/dotnet/roslyn/issues/61489#issuecomment-1664648377)

### API Review

* What's the use case for this?
    * Need concrete users for this, particularly if there are perf reasons for adding the api.
* This could encourage bad behavior in source generators

**Conclusion**: Rejected until concrete usage scenarios arise.
## Allow inheritdoc expansion when using `ISymbol.GetDocumentationCommentXml`

**NeedsWork** | [#roslyn/68879](https://github.com/dotnet/roslyn/issues/68879#issuecomment-1664650168)

### API Review

* Proposal would put the `inheritdoc` feature into the compiler itself.
    * Currently exists as a workspaces feature.
* The implementation is likely incomplete, as it was only implemented well enough for QuickInfo
    * May have various holes in the implementation.
    * Ref assemblies don't have documentation at build time, couldn't implement that in the compiler today.
* More work would need to be done to make this available to analyzers, since the compiler doesn't have the documentation assemblies
    * Putting in the workspace is likely fine.
    * Where is this actually needed? In an analyzer, or in some feature that has access to a workspace? The former can likely only happen if the feature is actually added to the C# language.

**Conclusion**: Needs work.

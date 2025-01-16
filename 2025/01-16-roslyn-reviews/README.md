# API Review 01/16/2025

## `IncrementalGeneratorPostInitializationContext.AddEmbeddedAttributeDefinition`

**Approved** | [#roslyn/76584](https://github.com/dotnet/roslyn/issues/76584#issuecomment-2596945826)

### API Review

* We don't think that this should be used as a justification for other polyfills, such as nullability attributes downlevel.
* If the BCL ever decides to add a public EmbeddedAttribute definition, there will need to be more work done in general to permit the definition and honor it, and we adjust the implementation of this method at that point.

**Conclusion**: Approved.

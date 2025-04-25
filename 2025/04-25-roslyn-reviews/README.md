# API Review 04/25/2025

## API for generating the virtual project behind file-based programs

**NeedsWork** | [#roslyn/78159](https://github.com/dotnet/roslyn/issues/78159#issuecomment-2831080054)

### API Review

* Names will require work
* The builder approach feels a bit non-Roslyn; rather, it seems like the more idiomatic approach would be to have a static method that provides all files up front.
* Putting this in the compiler seems like it may be an issue. What if we want to add project references in the future; that's a workspaces layer issue, not a compiler-level issue.
    * This is effectively an API for construction a compilation, much like other workspaces apis for construction compilations over projects.
* ArtifactsPath is causing some consternation; what sets it? Where does it come from?
* Should we look at using a SyntaxTree over a SourceText?
    * Can't reuse that SyntaxTree, because the parse options will be influenced by the directives
    * Pit of failure for users calling SyntaxTree APIs that then causes waste because you likely can't reuse the tree.
* FileBasedProgramProjectBuilder.ParseDirectives - filePath is a confusing name. It's only used for diagnostics, but first in the order. Should probably be renamed and moved later in the args list?
* Are we going to want to suport multiple TFMs? Should we design the API to support that case in the future?
    * Current design is that that's a line where you need a real project.
* Should this API be internal for the SDK?
    * Can we think of any third-parties that would want to use the API?
* What if this isn't an API? What if there's just a `dotnet run` switch that produces the project file the IDE can consume?
    * There are assumptions baked into the file generation on SDK targets, which doesn't seem like a good fit for any Roslyn layer.
    * This is a good path. Let's explore it more before we bring anything back on the roslyn side.

**Conclusion**: We will explore the SDK-based file generation strategy as the "API" for the IDE here, rather than a Roslyn-based solution.
## New APIs for "User Defined Compound Assignment Operators" feature

**Approved** | [#roslyn/78261](https://github.com/dotnet/roslyn/issues/78261#issuecomment-2831080716)

### API Review

* Rubber stamp, APIs look good.

**Conclusion**: Approved

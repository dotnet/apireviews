# API Review 10/10/2024

## Make GeneratorDriverOptions.BaseDirectory public

**Approved** | [#roslyn/75387](https://github.com/dotnet/roslyn/issues/75387#issuecomment-2406014762)

### API Review

- Are there hosts that don't are about the base path? Should we leave the existing driver options alone?
    - Let's not obsolete the existing constructors
- Will this have impacts to users in the IDE? Will it cause the IDE to show things that don't exist?
    - Should the syntax tree have 2 paths, one relative and one absolute?
    - We already have cases in the IDE where have documents with paths, but those paths don't actually exist
- Compiler paths aren't exposed anywhere else, why is this different?
    - Differing layers of the compiler: this is not the main compiler loop. Source generated files are
      an intermediate layer that then gets fed into the file compilation.
        - This argument is convincing
- Esoteric hypothetical: what would the path be if the interactive window ran a generator?
    - Interactive would create some non-existant file paths based on the current submission
- EmitOptions works on strings as well, so using strings for the path here works as well

**Conclusion**: Original design is approved, minus obsoleting the existing constructors.
## Add GeneratedFilesOutputDirectory to Solution APIs

**Approved** | [#roslyn/75389](https://github.com/dotnet/roslyn/issues/75389#issuecomment-2406015193)

### API Review

- This reflects the compiler generated files path and comes from the project system, not the compiler

**Conclusion**: API is approved

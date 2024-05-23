# API Review 05/23/2024

## Public API for partial properties

**Approved** | [#roslyn/73411](https://github.com/dotnet/roslyn/issues/73411#issuecomment-2128000031)

### API Review

- `IsPartialDefinition`
    - What about error scenarios where only the definition exists?
    - This will be particularly common for SG scenarios

**Conclusion**: Approved, with the addition of `IsPartialDefinition`.
## New syntax related APIs to support "Ref Struct Interfaces" feature

**Approved** | [#roslyn/73552](https://github.com/dotnet/roslyn/issues/73552#issuecomment-2128000873)

### API Review

- Some discussion on whether to make the clause a single element, rather than a list of elements
    - Wouldn't keep the future-proofing if LDM ever wants `allows ref struct, pointer`

**Conclusion**: Approved

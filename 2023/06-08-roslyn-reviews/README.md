# API Review 06/08/2023

## Expose internal API CodeAction.CodeActionPriority 

**Approved** | [#roslyn/42431](https://github.com/dotnet/roslyn/issues/42431#issuecomment-1583284978)

### API Review

* Not exposing high pri for safety of the core system
* Validating inputs, 3 is not an option
    * Is the validation necessary?
    * People who want to replace our high-pri refactorings with their own?
    * Ship as-is with validation, wait for use cases?
    * Clamp instead? Would allow future version to clamp higher without a breaking change.

**Conclusion**: Approved, clamp higher values to highest public value.

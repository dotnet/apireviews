# API Review 06/17/2021

## Missing compiler generated switch expression arm in/from ISwitchExpressionOperation

**Approved** | [#roslyn/52577](https://github.com/dotnet/roslyn/issues/52577#issuecomment-863562850)

## API Review

* Naming seems fine as long as the documentation about nullable implications is good.
## SyntaxTree.GetLineMappings

**Approved** | [#roslyn/53752](https://github.com/dotnet/roslyn/issues/53752#issuecomment-863563291)

## API Review

* How will the new `#line` changes be incorporated?
    * Offset should be added before we ship.
* Otherwise looks good.
## Consider making Operation.ChildOperations public

**Approved** | [#roslyn/49475](https://github.com/dotnet/roslyn/issues/49475#issuecomment-863563588)

## API Review

* We're fine with the nesting level.
## New IOperation: IAttributeOperation

**NeedsWork** | [#roslyn/53618](https://github.com/dotnet/roslyn/issues/53618#issuecomment-863564797)

## API Review

* We'll defer giving information about the attribute target (ie, the field/method/property/etc the attribute is applied to) to future feedback. It's not a cheap thing to track because we don't track it today, and we'd prefer users to move from Symbols -> Operations, not the other way around.
* Should have a single `IOperation` child instead, which either be an `IObjectCreationOperation` for success cases, or some form of `IInvalidOperation` for invalid cases where a constructor wasn't bound.
## Add predicate version of IOperation.Descendants

**Approved** | [#roslyn/53621](https://github.com/dotnet/roslyn/issues/53621#issuecomment-863565123)

## API Review

* Seems like a fine convenience API. We already have the non-descendIntoChildren version.
* Our future selves can consider a cross-operation API break if we want to change to a struct-enumerator return type.

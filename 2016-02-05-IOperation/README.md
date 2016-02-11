# API Review 2016-02-05

This API review was also recorded and is available on [Google Hangouts](https://plus.google.com/events/cq30ha00iliq8p65100q664em40).

## IOperation

Status: **Needs work** |
[API Reference](IOperation.md) |
[Video](https://plus.google.com/events/cq30ha00iliq8p65100q664em40)

With `IOperation`, Roslyn will provide a language agnostic model for code (statements, expressions) that is implemented by both C# and VB, and potentially even IL. This will allow FxCop like tools to be able to have rules that can reason about code without having to drop down to the language specific syntax- and semantic APIs. It also allows to unify source and binary based analysis tools.

The entry point APIs for analyzers are [RegisterOperationAction](http://source.roslyn.io/#Microsoft.CodeAnalysis/DiagnosticAnalyzer/DiagnosticAnalysisContext.cs,2be7b73ecaa34492) and [RegisterOperationBlockAction](http://source.roslyn.io/#Microsoft.CodeAnalysis/DiagnosticAnalyzer/DiagnosticAnalysisContext.cs,2d5fe0e9ee88d525). You can find usages [here](https://github.com/dotnet/roslyn-analyzers/search?utf8=%E2%9C%93&q=RegisterOperationAction). They provide you the hierarchy of [IOperation](http://source.roslyn.io/#Microsoft.CodeAnalysis/Compilation/IOperation.cs,3ffbbad2c704a2bf) nodes to analyze.

### Notes

* Roslyn has a unified symbol API
* For the code, they currently don't share a common API -- you're directly exposed to syntax
* `IOperation` provides a unified view over the internal bound tree represent the compiler has
* Another goal is to raise the `IOperation` tree from IL
* The compiler lowers the bound tree
    - Roslyn currently exposes the highest level, because it's closest to source
    - Since we can raise from IL, the believe is that we could expose all lowered representations as well, if needed. It might, however, require adding additional nodes.
* The name `IOperation`: the name needs to convey for unification of expressions and expressions. We believe `operation` is the closest term without dropping too much down to compiler lingo (such as `ISemanticNode`)
* Should we follow the syntax and symbols and move some core concepts like `IOperation` and `OperationKind` to `Microsoft.CodeAnalysis`?
    - This would leave breadcrumbs in the root that can aid with discoverability
    - Consistency with syntax and symbols
* Consider an `IHasArgument` interface
* Right now, consumers can't extend the syntax nor the symbol APIs
    - The IL implementation has to
* `IOperation` has different kinds:
    - `OperationKind`, `BinaryOperatorKind`, `CaseKind`
    - Syntax doesn't do that
    - Enum members should be organized so that common checks can be expressed efficient via be lower/higher bound checks
    - `BinaryOperatorKind` seems a bit big, like it multiplies the operators, the specific argument types, and the specific semantics (like VB equals vs regular equals)
* `IExpression` and `IStatement` should be purely abstract, i.e. there shouldn't be instances whose only public type is `IExpression`/`IStatement`. There should be specific subtypes, even if they have no members. This makes visitors much more logical.

Additional comments from @nguerrera:

* `IInstanceReferenceExpression` is currently also an `IParameterReferenceExpression` to the hidden 'this' parameter. AFAICT, the hidden 'this' parameter is not otherwise exposed from `IMethodSymbol` and isn't really very useful so we might prefer `IInstanceReferenceExpression` inheriting directly from `IReferenceExpression`. This would resolve another case of a node-type that can be both interior and leaf, which came up as something to avoid across the board.

* Nodes of type `ICase` have `OperationKind.SwitchSection` and nodes of type `ICatch` have `OperationKind.CatchHandler`. These are rare departures from the 1:1 between interface names and operation kinds. We should rename one way or the other to make them match.

* Consider having a single `OperationKind.Branch` for `IBranchStatement` with a separate `BranchKind` enum member with `GoTo`, `Break`, `Exit` and `Continue` values. In general, I'd fined a strict 1:1 between the operation kinds and the leaf-node interfaces cleaner and easier for analyzer authors to internalize as a pattern.

* There are both `ILabeledStatement` and `ILabelStatement` corresponding to differences in C#/VB. `IOperation` should choose one representation or the other and normalize accordingly.

More comments are in the [API reference](IOperation.md).

# API Review 04/01/2025

## API for IgnoredDirectiveTrivia

**Approved** | [#roslyn/77697](https://github.com/dotnet/roslyn/issues/77697#issuecomment-2770195306)

### API Review

* Does consistency win on the trivia aspect?
* Could we make Shebang also consistent?
* We could add a calculated property to Shebang that will do the digging for the user?
    * Would that also need a SyntaxFactory overload?
* We like the idea of shimming Shebang for consistency.
* We like the name Content, it matches with other comment field names.

**Approved**: We will add an explicit `Content` token to `IgnoredDirective`. We will add a calculated property `Content` to `ShebangDirective`. We will add a `SyntaxFactory` method for `ShebangDirective` that matches the signature of the full factory method for `IgnoredDirective`.

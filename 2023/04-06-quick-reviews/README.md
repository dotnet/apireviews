# API Review 04/06/2023

## System.CommandLine: Parsing

**Approved** | [#runtime/84177](https://github.com/dotnet/runtime/issues/84177#issuecomment-1499477999) | [Video](https://www.youtube.com/watch?v=vDv6CJ-Oa7Q&t=0h0m0s)

* Why is the SymbolResult recursive?
* What is the primary output? `SymbolResult` or `ParseError`?
* `ParseResult`
    - `UnmatchedTokens` should probably be an `IReadOnlyList<string>`
* `Cli` prefix isn't used consistently
    - We probably don't want all types to be prefixed, but we should probably ensure that types that are commonly used together appear consistent
* `DefaultValueFactory`
    - These are invoked when providing help but the example includes cases where those report errors. Can this "mess up" the help page?
* Virtual/Abstract
    - If we consider them plumging, we shouldn't have public/protected virtuals
    - If we want to support overriding, we should make them more consistent
* Setters should generally not be modified after the first parse but we don't believe we need to enforce that
    - We considered using `init` but we believe this negatively affects source generation and/or usability of newing types up, also doesn't work for .NET Standard 2.0 consumers
* Can we make `CliParser` internal?
* `FindResultFor` should probably be `GetResult`, so that it matches `GetValue`

```C#
namespace System.CommandLine.Parsing;

public enum TokenType
{
    Argument,
    Command,
    Option,
    DoubleDash,
    Directive
}
public sealed class CliToken : IEquatable<CliToken>
{
    public CliToken(string? value, TokenType type, CliSymbol symbol);
    public string Value { get; }
    public TokenType Type { get; }
}
public sealed class ParseError
{
    public string Message { get; }
    public SymbolResult? SymbolResult { get; }
}
public abstract class SymbolResult
{
    public SymbolResult? Parent { get; }
    public IReadOnlyList<CliToken> Tokens { get; }
    public virtual void AddError(string errorMessage);
    public ArgumentResult? FindResultFor(CliArgument argument);
    public CommandResult? FindResultFor(CliCommand command);
    public OptionResult? FindResultFor(CliOption option);
    public DirectiveResult? FindResultFor(CliDirective directive);
    public T? GetValue<T>(CliArgument<T> argument);
    public T? GetValue<T>(CliOption<T> option);
}
public sealed class ArgumentResult : SymbolResult
{
    public CliArgument Argument { get; }
    public T GetValueOrDefault<T>();
    public void OnlyTake(int numberOfTokens);
}
public sealed class OptionResult : SymbolResult
{
    public CliOption Option { get; }
    public bool Implicit { get; }
    public CliToken? IdentifierToken { get; }
    public T? GetValueOrDefault<T>();
}
public sealed class DirectiveResult : SymbolResult
{
    public IReadOnlyList<string> Values { get; }
    public CliDirective Directive { get; }
    public CliToken IdentifierToken { get; }
}
public sealed class CommandResult : SymbolResult
{
    public CliCommand Command { get; }
    public CliToken IdentifierToken { get; }
    public IEnumerable<SymbolResult> Children { get; }
}
```

# API Review 03/30/2023

## System.CommandLine APIs

**Approved** | [#runtime/68578](https://github.com/dotnet/runtime/issues/68578#issuecomment-1490763062) | [Video](https://www.youtube.com/watch?v=yDQGsZSEDOk&t=0h0m0s)

* CliSymbol.IsHidden => Hidden (drop the "Is")
* CliSymbol.Parents uses a yield return iterator, so it's correct to be an IEnumerable instead of a more specific type.
* `CliArgument<T>` doesn't yet seem justified as being generic, but the T is promised to be applicable later.
* CliOption.AppliesToSelfAndChildren => Recursive
* CliOption.IsRequred = Required
* CliOption.Aliases is a private custom collection type, so the interface return is correct.
  * We discussed ISet vs ICollection, and feel ISet is not appropriate.
* CliOption we added ValueType for symmetry with CliArgument
* CliCommand.Children uses yield return, so IEnumerable is OK
* CliChildren.Arguments, Options, and Subcommands are all non-public custom collection types, so IList is correct.
* DiagramDirective: errorExitCode was renamed to parseErrorReturnValue, and made a get/set property instead of a ctor parameter.
* We discussed the "Cli" prefix.  We agree there does need to be some particle (because "Symbol", "Option", "Command", etc are too general and will probably have app collisions).  No one thought it was great, but no one had a better name.

The other four phases should be opened as 4 separate issues to be discussed individually.

```C#
namespace System.CommandLine;

/// <summary>
/// Defines a named symbol that resides in a hierarchy with parent and child symbols.
/// </summary>
public abstract class CliSymbol
{
    private protected CliSymbol(string name);

    /// <summary>
    /// Gets or sets the description of the symbol.
    /// </summary>
    public string? Description { get; set; }

    /// <summary>
    /// Gets the name of the symbol.
    /// </summary>
    public string Name { get; }

    /// <summary>
    /// Gets or sets a value indicating whether the symbol is hidden.
    /// </summary>
    public bool Hidden { get; set; }

    /// <summary>
    /// Gets the parent symbols.
    /// </summary>
    public IEnumerable<CliSymbol> Parents { get; }
}

/// <summary>
/// A symbol defining a value that can be passed on the command line to a <see cref="CliCommand">command</see> or <see cref="CliOption">option</see>.
/// </summary>
public abstract class CliArgument : CliSymbol
{
    private protected CliArgument(string name);

    /// <summary>
    /// The name used in help output to describe the argument. 
    /// </summary>
    /// Example: `dotnet build --help`
    /// Arguments:
    /// <PROJECT | SOLUTION>  The project or solution file to operate on.
    /// "PROJECT | SOLUTION" is the HelpName here
    public string? HelpName { get; set; }

    /// <summary>
    /// Gets or sets the <see cref="Type" /> that the argument token(s) will be converted to.
    /// </summary>
    public abstract Type ValueType { get; }
}

public class CliArgument<T> : CliArgument
{
    /// <summary>
    /// Initializes a new instance of the Argument class.
    /// </summary>
    /// <param name="name">The name of the argument. It's not used for parsing, only when displaying Help or creating parse errors.</param>>
    public CliArgument(string name);

    /// <inheritdoc />
    public override Type ValueType => typeof(T);
}

/// <summary>
/// A symbol defining a named parameter and a value for that parameter. 
/// </summary>
public abstract class CliOption : CliSymbol
{
    private protected CliArgument(string name);

    /// <summary>
    /// Gets or sets the name of the Option when displayed in help.
    /// </summary>
    /// <value>
    /// The name of the option when displayed in help.
    /// </value>
    /// <remarks>Useful for localization, as it's not used for actual parsing.</remarks>
    public string? HelpName { get; set; }

    /// <summary>
    /// When set to true, this option will be applied to the command and recursively to subcommands.
    /// It will not apply to parent commands.
    /// </summary>
    public bool Recursive { get; set; }

    /// <summary>
    /// Indicates whether the option is required when its parent command is invoked.
    /// </summary>
    /// <remarks>When an option is required and its parent command is invoked without it, an error results.</remarks>
    public bool Required { get; set; }

    /// <summary>
    /// Gets the unique set of strings that can be used on the command line to specify the Option.
    /// </summary>
    /// <remarks>The collection does not contain the <see cref="CliSymbol.Name"/> of the Option.</remarks>
    public ICollection<string> Aliases { get; }

    /// <summary>
    /// Gets or sets the <see cref="Type" /> that the argument token(s) will be converted to.
    /// </summary>
    public abstract Type ValueType { get; }
}

public class CliOption<T> : CliOption
{
    /// <summary>
    /// Initializes a new instance of the Option class.
    /// </summary>
    /// <param name="name">The name of the option. It's used for parsing, displaying Help and creating parse errors.</param>>
    /// <param name="aliases">Optional aliases. Used for parsing, suggestions and displayed in Help.</param>
    public CliOption(string name, params string[] aliases);

    /// <inheritdoc />
    public override Type ValueType => typeof(T);
}

/// <summary>
/// Represents a specific action that the application performs.
/// </summary>
/// <remarks>
/// Use the Command object for actions that correspond to a specific string (the command name). See
/// <see cref="RootCommand"/> for simple applications that only have one action. For example, <c>dotnet run</c>
/// uses <c>run</c> as the command.
/// </remarks>
public class CliCommand : CliSymbol, IEnumerable<CliSymbol>
{
    /// <summary>
    /// Initializes a new instance of the Command class.
    /// </summary>
    /// <param name="name">The name of the command.</param>
    /// <param name="description">The description of the command, shown in help.</param>
    public CliCommand(string name, string? description = null);

    /// <summary>
    /// Gets the child symbols.
    /// </summary>
    public IEnumerable<CliSymbol> Children { get; }

    /// <summary>
    /// Represents all of the arguments for the command.
    /// </summary>
    public IList<CliArgument> Arguments { get; }

    /// <summary>
    /// Represents all of the options for the command, including global options that have been applied to any of the command's ancestors.
    /// </summary>
    public IList<CliOption> Options { get; }

    /// <summary>
    /// Represents all of the subcommands for the command.
    /// </summary>
    public IList<CliCommand> Subcommands { get; }

    /// <summary>
    /// Gets the unique set of strings that can be used on the command line to specify the command.
    /// </summary>
    /// <remarks>The collection does not contain the <see cref="CliSymbol.Name"/> of the Command.</remarks>
    public ICollection<string> Aliases { get; }

    /// <summary>
    /// Adds a <see cref="CliSymbol"/> to the command.
    /// </summary>
    /// <param name="symbol">The symbol to add to the command.</param>
    [EditorBrowsable(EditorBrowsableState.Never)] // hide from intellisense, it's public for C# duck typing
    public void Add(CliSymbol symbol);

    /// <summary>
    /// Represents all of the symbols for the command.
    /// </summary>
    public IEnumerator<CliSymbol> GetEnumerator() => Children.GetEnumerator();

    /// <inheritdoc />
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

/// <summary>
/// The purpose of directives is to provide cross-cutting functionality that can apply across command-line apps.
/// Because directives are syntactically distinct from the app's own syntax, they can provide functionality that applies across apps.
/// 
/// A directive must conform to the following syntax rules:
/// * It's a token on the command line that comes after the app's name but before any subcommands or options.
/// * It's enclosed in square brackets.
/// * It doesn't contain spaces.
/// </summary>
public class CliDirective : CliSymbol
{
    /// <summary>
    /// Initializes a new instance of the Directive class.
    /// </summary>
    /// <param name="name">The name of the directive. It can't contain whitespaces.</param>
    public CliDirective(string name);
}

/// <summary>
/// Enables the use of the <c>[env:key=value]</c> directive, allowing environment variables to be set from the command line during invocation.
/// </summary>
public sealed class EnvironmentVariablesDirective : CliDirective
{
    public EnvironmentVariablesDirective();
}

/// <summary>
/// Enables the use of the <c>[suggest]</c> directive which when specified in command line input short circuits normal command handling and writes a newline-delimited list of suggestions suitable for use by most shells to provide command line completions.
/// </summary>
/// <remarks>The <c>dotnet-suggest</c> tool requires the suggest directive to be enabled for an application to provide completions.</remarks>
public sealed class SuggestDirective : CliDirective
{
    public SuggestDirective();
}

/// <summary>
/// Enables the use of the <c>[diagram]</c> directive, which when specified on the command line will short 
/// circuit normal command handling and display a diagram explaining the parse result for the command line input.
/// </summary>
/// Example: dotnet [diagram] build -c Release -f net7.0
/// Output: [ dotnet [ build [ -c <Release> ] [ -f <net7.0> ] ] ]
public sealed class DiagramDirective : CliDirective
{
    /// <param name="errorExitCode">If the parse result contains errors, this exit code will be used when the process exits.</param>
    public DiagramDirective();
    
    public int ParseErrorReturnValue { get; set; } = DefaultErrorExitCode);
}
```
## Add an overload of the StackTrace constructor to take in an array of frames

**Approved** | [#runtime/82871](https://github.com/dotnet/runtime/issues/82871#issuecomment-1490783317) | [Video](https://www.youtube.com/watch?v=yDQGsZSEDOk&t=1h38m20s)

* We felt that IEnumerable was more flexible, allowing for callers to use Linq methods, etc, more easily.

```C#
namespace System.Diagnostics;

public partial class StackTrace
{
    //Additional constructor
    public StackTrace(IEnumerable<StackFrame> frames)
    {
        // Make a draining copy
    }
}
```

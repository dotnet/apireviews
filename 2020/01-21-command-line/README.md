# System.CommandLine

## Pieces: 

* Parser and invocation
* Console

## Console:

* Ansi Terminal/VT 100 - System.Console has limited and inconsistent support
* Redirected output/input
* Better console and terminal abstractions
* Working with Windows Console and PowerShell teams
* New Windows terminal, Rich Turner

## How we got there on the parser side:

* Recognized we had experts (Jon and Nate)
  - .NET CLI is both very complex and has known shortcomings
  - Mark Michaelis
* Evaluated other parsers
* Syntax abstraction is core and missing from most parsers
* Layered - generalized API intended to sit under app models - DragonFruit proof of concept
* A few features (all free for the author)
  - Expected - description, alias, etc. 
  - Strongly typed with validation: native, File/Dir, enums, NuGet packages
  - Custom validation
  - Help, Tab Completion/Suggestions and Response files
  - Linux/Posix support and base for being prefix varied or neutral
  - Author support: Diagramming, exceptions, transforms/partial parsing
  - Future: 
    - GUI and Vision impaired (reader support)
    - GUI may be relevant for the next to iteration of templates
    - We may also support tooling
    - App Models - for example, suggestions from non-System.CommandLine help files
* Intend to work with community for protocols like content negotiation (suggestions, GUI, Accessibility)
* Where we want to go:
* Don't need to be in the BCL, at least for now
  - Still iterating
  - Refactoring to smaller pieces for BCL/Not BCL not helpful
* Today: 
  - User study

## Notes

* Handler should probably not use delegates and instead take a raw method info
  if the implementation relies on the parameter names coming from the user
  defined method. The compiler might create a wrapper delagate which would
  result in loss of said names. According to Levi, that's what ASP.NET Core was
  running into.
* `Option<T>` and `Argument<T>` are good names but too generic. We should come
  up with a common prefix (such as `Command`).
* The low-level API should be very flexible
* Should `Handler` be a property or an argument to `Invoke`?
* It seems unlikely that people would start with the type name `RootCommand`.
  Have you considered a different entry point?
* Should *Dragonfruit* support multiple Main methods (for subcommands)?
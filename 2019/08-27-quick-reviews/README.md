# Quick Reviews 8/27/2019

## API Change Request: Expression support for ref and readonly ref types

**Needs Work** | [#corefx/26772](https://github.com/dotnet/corefx/issues/26772#issuecomment-525417923) | [Video](https://www.youtube.com/watch?v=maDBdlZfCGA&t=0h0m0s)

There is a general item on the compiler team to figure out how expression trees are going to be evolved (and if).
## Add a ValueStringBuilder

**Needs Work** | [#corefx/28379](https://github.com/dotnet/corefx/issues/28379#issuecomment-525424732) | [Video](https://www.youtube.com/watch?v=maDBdlZfCGA&t=0h5m44s)

We believe that without a feature that forces consumers to pass value types by reference (either language feature or in-box analyzer) this will cause unreliable applications due to corruption of the array pool when accidental copies are being made.

@JeremyKuhne do you want to drive that discussion with @jaredpar?
## Add Path.RemoveRelativeSegments Api

**Approved** | [#corefx/30701](https://github.com/dotnet/corefx/issues/30701#issuecomment-525429074) | [Video](https://www.youtube.com/watch?v=maDBdlZfCGA&t=0h24m8s)

@Anipik you mentioned you don't want to use `GetFullPath()` because you want to preserve the relativeness. What's the scenario for this API then? Functionally, it seems `GetFullPath()` or keeping the path with the dots seems both OK.
## List<T>.AsSpan()

**Needs Work** | [#corefx/19814](https://github.com/dotnet/corefx/issues/19814#issuecomment-525432693) | [Video](https://www.youtube.com/watch?v=maDBdlZfCGA&t=0h34m56s)

Let's start with no :)

# API Review 11/04/2021

## Provide API access to MS.CA.Rebuild functionality

**Approved** | [#roslyn/57356](https://github.com/dotnet/roslyn/issues/57356#issuecomment-961227384)

Part of the reason there is no API access is that this feature is not complete. The project is somewhere between alpha-beta phases. The underlying project that was motivating this work is more / less delayed at this point hence our project is delayed as well. 

If this would benefit other scheduled feature areas I would be interested in understanding that so we can consider re-prioritizing it.

Before we could offer an API we'd essentially need to finish up the underlying implementation.


## IFlowCaptureReferenceOperation.IsInitialization

**Approved** | [#roslyn/57484](https://github.com/dotnet/roslyn/issues/57484#issuecomment-961390590)

## API Review Notes

We rename `IsInitialization` to `IsCapture` for consistency with the `IFlowCaptureOperation` name. Otherwise, the API is approved.

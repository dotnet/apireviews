# System.Device.Gpio

Status: **Needs more work** | 
[API Ref](System.Device.Gpio2.md) |
[Spec](System.Device.Gpio.Proposal2.md) |
[Video](https://www.youtube.com/watch?v=wtkPtOpI3CA)

Links:

* https://gist.github.com/joperezr/5da1bde93e2985ecead2d453f9c1a34f
* https://gist.github.com/joperezr/a293d30fc714b9e92756c10f1ea89afe

## Notes

* We'll want to expose other protocols too (a, b, c), hence we probably don't
  want to name the assembly `System.Device.Gpio`. We need a more general name,
  `System.Device` is too broad (and conflicts with an existing .NET Framework
  assembly).
* Should the APIs be static? Right now there are no scenarios for multiple
  boards. That's what Python did.
* We could decided to future proof our API for the case where we need multiple
  boards, but it seems there is a high chance that this domain changes
  drastically over the next years, so maybe we'll just expose a new API anyways.
    - This would also means we probably don't want to use the `System` namespace.
    - For instance, `Microsoft.Device.Xxx`?
* The `WaitForEvent` method is still prone to problem of lossy even as events
  that occur between two calls of `WaitForEvent` will not be observed.
    - Jose should explore what to here
    - It would be good to see what WinRT did here (it seems they expose an
      object that has a lifetime that manages a queue) and what Python does.
    - Jan's proposal is set beginners on the path of `while (true) { /* check state */ }` pattern.
    > Sure, the chip gets a bit hotter, but who cares?
* We need to think about how we can expose new protocols, e.g. *MCP23008*, which
  is a GPIO extender. The question is why how we model this in the API. Is it
  inheritance?
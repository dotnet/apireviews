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

## API Feedback

* `GpioController.OpenPin()`
    - [@janvorli](https://github.com/janvorli):
        > Regarding the `OpenPin` name- there are applications where you
        > actually need to switch the pin mode multiple times. `WiringPy` uses
        > `PinMode` name, which sounds more appropriate. Maybe we could use the
        > same name or `SetPinMode`. And as for the `ClosePin`, what is it
        > supposed to do? It seems it is not needed in the no pin object model
        > world.
    - [@joperezr](https://github.com/joperezr) 
        > We will also add the SetPinMode method, but OpenPin's functionality is
        > not just to set its mode. There are some Internal Drivers that will
        > require the actual pin to be exported for use (For example, the Unix
        > driver which uses SYSFS). In this model, we need to first Open the
        > pin, and then we can do things with it, like set its mode or perform
        > read/write operations.
        >
        > The ClosePin, would basically make sure it unexports the pin so that
        > it goes into a clean state. This is very important, and if not done
        > manually, it will be called on the controller.Dispose() for each
        > opened pin.

* `GpioController.AddCallbackForValueChangedEvent()`
    - [@janvorli](https://github.com/janvorli):
        > Is there any device where you can get some kind of native callback
        > when a pin changes? It seems that e.g. RPi only has a polling way to
        > detect that an event has occurred on a pin (an example for one of the
        > c libraries for GPIO can be seen here:
        > http://www.airspayce.com/mikem/bcm2835/event_8c-example.html - there
        > is a flag related to the pin that you clear and when the change you
        > are interested in happens, the flag gets set until you clear it - so
        > it is essentially similar to Windows events). If we wanted to support
        > events like this, we would need to have some internal background
        > thread checking for the events in a loop and then issuing the event. I
        > wonder if it wouldn't be better to keep just the WaitForEvent API.

* `GetPinNumberFromConfig()`
    - [@janvorli](https://github.com/janvorli):
        > Can you please explain what this method is going to do? I am not sure
        > where does the config come from.
    - [@joperezr](https://github.com/joperezr) 
        > This was discussed in the review and was supposed to help with the
        > docker scenario. Basically the idea behind it is that if your app
        > would run in a docker container(meaning that the actual program is
        > probably already built in the image) then you would have a way to
        > override the pins used for hooking up whatever device you would use.
        > So for example, if one specific microcontroller where you wanted to
        > run your app inside a container is already using one of the default
        > pins, then you could override it with some configuration setup (this
        > being either an environment variable or a config file.) In any case,
        > during the API Review it was decided that this sort of functionality
        > shouldn't live with the GPIO API, and if a user needed this then they
        > should write their own code which would take care of this.

* `BatchOpenPins()`
    - [@janvorli](https://github.com/janvorli):
        > Why do we add two functions differing in just an additional parameter
        > instead of the `mode` parameter having a default value and keeping
        > just the two parameter version?
    - [@joperezr](https://github.com/joperezr) 
        > This is because for some drivers (like the SYSFS driver) you can open
        > a pin, but not set its mode. In this case, the pin will be opened
        > using whichever mode it was last used for (or Input if it has never
        > been used before) If we provide a default, then we would be overriding
        > this behavior.

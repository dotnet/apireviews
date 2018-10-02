# System.Device.Gpio

Status: **Needs more work** | 
[API Ref](System.Device.Gpio.md) |
[Spec](System.Device.Gpio.Proposal.md) |
[Video](https://www.youtube.com/watch?v=OK0jDe8wtyg)

## Notes

* `EnableRasingEvents` looks like a usability trap. Could we do this
  automtaically, e.g. when a callback is registered, when `NotifyFilter` is set,
  or when `WaitForEvent` is called
* Should we also offer `WaitForEventAsync`?
* `WaitForEvent` has race condition, unless the system buffers events.
* `GpiController.Open()` probably should have validation to ensure folks don't
  pick the wrong board by accident.
* Instead of `GpiController.Open()` with a enum and a drive, should we just
  expose concrete controllers, such as `RaspberryPiController`?
* The `Open()` method with no arguments seems like a trap too, as the generic
  driver will not be performant. We should let people chose the one they want.
* In cases the board has multiple controllers, the `Open()` method can be
  extended in the future.
* What happens if people open multiple drivers or controllers? Should we block
  this?

Lots of dicussion around the factories. We'd like to make things consistent
between the controller and the pin. Also, we'd like to have the pins open ended,
i.e. support derived types. We have two options:

1. Both can be new'ed up
    ```c#
    using (var controller = new GpioController(GpiDriverKind.RaspberryPi2))
    var pin = new GpioPin(controller, 5)
    ```
2. Both are created via factories.
    ```c#
    using (var controller = GpioController.Open(GpiDriverKind.RaspberryPi2))
    var pin = GpioPin.Open(controller, 5);
    ```

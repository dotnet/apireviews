# System.Device.Gpio (Round 3)

Status: **Needs more work** | 
[API Ref](System.Device.Gpio3.md) |
[Video](https://www.youtube.com/watch?v=UZc3sbJ0-PI)

## Notes

* `AddCallbackForPinValueChangedEvent`/`RemoveCallbackForPinValueChangedEvent` is weird.
    - Can we simplify the arguments?
    - Can we provide a lightweight struct that allows regular event subscription
    ```C#
    controller.PinEvents[pinNumber, evenType].Raising += MyHandler;
    controller.PinEvents[pinNumber, evenType].Falling += MyHandler;

    public void MyHandler(object sender, PinEventArgs e)
    {
        if (e.IsRaising)
        {
            // ...
        }
        else if (e.IsFalling)
        {
            // ...
        }
    }
    ```
* Timeouts should be in `TimeSpan`
* Can we replace the timeout with `CancellationToken` or should we at least add
  an overload that takes a cancellation token
* `GpioDriver` looks pretty big. Could some of the methods be virtual rather than abstract?
* `Dispose()` should be non-virtual. Instead, implement the Dispose-pattern.
* `GpioException` isn't needed. Use `IOException`.
* `WaitForEvent` is lossy
* `GpioController` shouldn't have a finalizer

## I2c

* `uint` -> `int`
* The interface should be abstract class
* Consider hiding the Unix type and replace it with a factory on the the abstract class
* Ideally though, you make it consistent with `Gpio`
* Instead of implementing reading/writing yourself, consider exposing a `Stream`. You might
  want to consider overloads that return a `BinaryReader`/`BinaryWriter` too.
* `ConnectionSettings` --> `Configuration`

## PWm

* `frequency`. May want to make sure to align the unit, which is Hertz.
* What happens if we start support reading, can does the naming make sense here,
  e.g. would `Start` and `Stop` have a different name?

## Spi

* See comments on `I2c`
* The `SpiMode` enum is weird as it adds zero over raw integers, plus/minus epsilon.
* `ConnectionSettings` --> `Configuration`

# System.Device.Gpio Proposal

The goal of this API is to allow .Net Core developers to access General Purpose
IO (GPIO) pins of IoT devices like Raspberry Pi, Hummingboard, or Odroid. This
will allow applications to read and write data from/to sensors, leds and other
kind of peripherals connected to those pins. This API should also support most
commonly used serial communication protocols like SPI and I2C.

## Rationale and Usage

### API Design Goals

* Extensible to many different kinds of IoT devices
* Multiplatform (Linux and Windows OS)
* General enough to allow the same client user code to work in all supported
  platforms
* Simple to use
* Good performance

### Main classes the user interacts with

#### GpioController

This class will act as a Object Factory which will have several `Open()` methods
that will take different parameters which will return you an instance of the
controller for the device you are running in. You can specify the driver that
you want to use in order to perform all the low level operations, or you can
instead call the default factory method which will return the default controller
for the platform you are running on. The controller will support both the
physical pin numbering scheme, and the Logical Gpio pin numbering scheme. The
controller will also contain a Dispose method that will take care of all of the
cleanup required, like closing and disposing all of the open pins for that
controller. For this reason, the common pattern will be that whenever we want to
interact with a pin, we will wrap it inside a `using` clause that will get the
controller.

#### GpioPin

This class will be initialized by calling a constructor and specifying the
controller that it belongs to, and the location of the pin on the controller. It
will contain instance methods that take care of basic pin operations, like
reading and writing a pin's value, setting a pin's mode, and also the ability
for registering for specific event notifications and setting callbacks when
there is a state change. It will also contain more advance methods like using
PWM to read/write analog values.

### Blinking LED

``` C#
using (GpioController controller = GpioController.Open())
{
	GpioPin pin = new GpioPin(controller, 18, PinMode.Output);
	while (true)
	{
	    pin.WriteValue(PinValue.High);
	    Thread.Sleep(TimeSpan.FromSeconds(1));
	    pin.WriteValue(PinValue.Low);
	    Thread.Sleep(TimeSpan.FromSeconds(1));
	}
}
```

### Turning on LED while pressing a button
```C#
using (GpioController controller = GpioController.Open())
{
    GpioPin button = new GpioPin(controller, 13, PinMode.Input);
    GpioPin led = new GpioPin(controller, 18, PinMode.Output);
    while (true)
    {
        PinValue buttonValue = button.ReadValue();
        led.WriteValue(buttonValue);
    }
}
```

### Waiting for a button to be pressed

```C#
using (GpioController controller = GpioController.Open())
{
    GpioPin button = new GpioPin(controller, 13, PinMode.Input);
    button.EnableRaisingEvents = true;
    button.NotifyFilter = PinEvent.RisingEdge;
    bool eventRecieved = button.WaitForEvent(TimeSpan.FromSeconds(10));
    if (eventRecieved)
        Console.WriteLine("An event was received.");
    else
        Console.WriteLine("No event was received.");
}
```

### Registering callbacks when a button is pressed.

```C#
    using (GpioController controller = GpioController.Open())
    {
        GpioPin button = new GpioPin(controller, 13, PinMode.Input);
        button.EnableRaisingEvents = true;
        button.NotifyFilter = PinEvent.FallingEdge | PinEvent.RisingEdge;
        button.ValueChanged += DoSomething;
    }
}

public static void DoSomething(object sender, EventArgs args);
```

### Migrating to a different OS and/or IoT device

Each controller will internally have a driver which is the one in charge of
actually interacting with the pins and performing all of the actions at the low
level. We have two types of these drivers, generic ones(platform-specific) and
specific ones (board-specific). The specific drivers will be more performant, as
they will be fliping bits in registers directly, but they won't be usable on
other microcontrollers. The generic ones, will use SYSFS in order to perform its
operations, which means that they will work on different boards.

``` C#
// For drivers that ship with the framework, we will provide a Factory method
// that takes an enum so that the user can pick the driver.
GpioController controller = GpioController.Open(GpioDriverKind.RaspberryPi3);

// For extensibility, we also have a Factory method that takes in a user
// specified driver that will support a new board.
// MyAwesomeGpioDriver is a subclass of GpioDriver class
GpioController controller = GpioController.Open(new MyAwesomeGpioDriver());

// If not specified, then the controller will use the default driver for the
// platform OS
// Will use the UnixDriver for Debian systems, and WindowsIOT Driver for
// Windows 10
GpioController controller = GpioController.Open(); 
```

### Using a different pin numbering scheme

We will support two different types of numbering schemes: Board and Logical. The
Board numbering is the physical number of where the pin is located, and the
Logical is where the pin is represented by the Gpio number.

``` C#
var controller = GpioController.Open(); // defaults to PinNumberingScheme.Logical
var controller = GpioController.Open(PinNumberingScheme.Logical); // or
var controller = GpioController.Open(PinNumberingScheme.Board);
```

### Communicating with sensors using SPI and I2C

*TODO: These two protocols will be discussed in the next iteration of the Api
Review*

## Proposed API

Following are the proposed APIs.

### GpioController class
``` C#
public class GpioController : IDisposable
{
    protected GpioDriver driver { get; }
    public static GpioController Open(PinNumberingScheme numbering = PinNumberingScheme.Logic);
    public static GpioController Open(GpioDriver driver, PinNumberingScheme numbering = PinNumberingScheme.Logic);
    public static GpioController Open(GpioDriverKind kind, PinNumberingScheme numbering = PinNumberingScheme.Logic);
    public PinNumberingScheme Numbering { get; set; }
    public IEnumerable<IGpioConnection> OpenConnections { get; }
    public int PinCount { get; }
    public bool isPinOpen(int pinNumber);
    public bool isPinModeSupported(int pinNumber, PinMode mode);
    public void Dispose();
    public void CloseAllPins();
}

public enum GpioDriverKind
{
    RaspberryPi3,
    Unix,
    WindowsIOT,
    ODroid,
    HummingboardEdge
}
```

### GpioPin class
``` C#
public class GpioPin : GpioConnection
{
    // Core Functionality of a Pin
    public GpioPin(GpioController controller, int gpioNumber, PinMode mode);
    public GpioPin(GpioController controller, int gpioNumber, PinMode mode, bool initialValue);
    public new void Dispose();
    public bool ReadValue();
    public void WriteValue(bool value);
    
    // PWM
    public int PWMFrequency { get; set; }
    public float PWMDutyCycle { get; set; }
    public int PWMRange { get; set; }
    public PWMMode PWMMode { get; set; }

    // Analog RW
    public int AnalogRead();
    public void AnalogWrite(int value);
    public int AnalogReadWait(TimeSpan timeout);

    // Eventing
    public TimeSpan DebounceTimeout { get; set; }
    public bool EnableRaisingEvents { get; set; }
    public PinEvent NotifyFilter { get; set; }
    public event EventHandler<EventArgs> ValueChanged;
    public bool WaitForEvent(TimeSpan timeout);
}
```

### GpioConnection abstract class

This class will represent anything that will be plugged into a controller and
that is capable to read and write values. This is an extensibility point for
future sensor-specific classes. ```C# public abstract class GpioConnection :
IDisposable { public IDictionary<int, PinMode> Pins { get; } public void
Write(byte[] value, int offset, int count); public int Read(byte[] buffer, int
offset, int count); public void Dispose(); }

### GpioDriver classes

``` C#
public abstract class GpioDriver
{
    protected GpioPin OpenPin(int pinNumber, PinMode mode);
    protected bool TryOpenPin(int pinNumber, PinMode mode, out GpioPin);
    protected void ClosePin(int pinNumber);
    protected int Read(byte[] buffer, int offset, int count);
    protected void Write(byte[] value, int offset, int count);
}

public class UnixDriver : GpioDriver
{
    // Driver that implements the GpioDriver actions by using SYSFS
}

public class RaspberryPi3Driver : GpioDriver
{
    // Driver that implements the GpioDriver actions by using the Raspberry Pi's
    // BCM2835 Peripheral regisers.
}

public class WindowsIOTDriver : GpioDriver
{
    // Driver that implements the GpioDriver actions by calling into the WINRT
    // APIs.
}
```

### Things that we might consider adding

* Sharing the same pin on different instances of GpioPin. WinRT Apis expose this
  as GpioSharingMode.
* Once a pin is initialized, should we allow changing the PinMode? WinRT does
  allow that.
* WinRT also supports keeping track of all the changes in value that happen on a
  pin, similar to what a FileSystemWatcher would do.
* Most methods should take the GpioPin object, instead of just the number, since
  we support different numbering schemes.
* Having GpioPin be an abstract class that each driver provides an
  implementation for. This would break some cycles we have for some types today,
  but it would also make the actual consumption of the code harder, since you
  would have to know the driver's specific class in order to consume it if we
  were to continue using the logic where the way to open pins is by calling a
  pin's constructor and pass in the controller it belongs to as a parameter.
* Changing `PinNumberingScheme` to be a class instead of an enum. This will be
  an extensibility so that users could add different ways of naming pins. I
  don't really see much benefits of doing this, since we could assume user's
  knowledge of the hardware plus most methods should take the Pin object, and
  not the number.
# System.Devices.Gpio

```C#
namespace System.Device.Gpio {
  // Immo Landwerth: Why is this needed? Could we expose this later?
  public abstract class GpioConnection : IDisposable {
    protected GpioConnection();
    public IDictionary<int, PinMode> Pins { get; }
    public void Dispose();
    public int Read(byte[] buffer, int offset, int count);
    public void Write(byte[] value, int offset, int count);
  }
  public class GpioController : IDisposable {
    protected internal GpioDriver driver { get; }
    public PinNumberingScheme Numbering { get; set; }
    // Immo Landwerth: Why is this needed? If people want to store a pin, they
    //                 can (e.g. in a local or a collection).
    public IEnumerable<GpioConnection> OpenConnections { get; }
    public int PinCount { get; }
    // Immo Landwerth: Why do we need this?
    public void CloseAllPins();
    public void Dispose();
    // Immo Landwerth: Fix casing
    public bool isPinModeSupported(int pinNumber, PinMode mode);
    public bool isPinOpen(int pinNumber);
    public static GpioController Open(GpioDriver driver, PinNumberingScheme numbering=(PinNumberingScheme)(1));
    public static GpioController Open(GpioDriverKind kind, PinNumberingScheme numbering=(PinNumberingScheme)(1));
    public static GpioController Open(PinNumberingScheme numbering=(PinNumberingScheme)(1));
  }
  public abstract class GpioDriver {
    protected GpioDriver();
    protected internal void ClosePin(int pinNumber);
    protected internal GpioPin OpenPin(int pinNumber, PinMode mode);
    protected internal int Read(byte[] buffer, int offset, int count);
    protected internal bool TryOpenPin(int pinNumber, PinMode mode);
    protected internal void Write(byte[] value, int offset, int count);
  }
  public enum GpioDriverKind {
    HummingboardEdge = 4,
    ODroid = 3,
    RaspberryPi3 = 0,
    Unix = 1,
    WindowsIOT = 2,
  }
  public class GpioPin : GpioConnection {
    public GpioPin(GpioController controller, int gpioNumber, PinMode mode);
    public GpioPin(GpioController controller, int gpioNumber, PinMode mode, bool initialValue);
    public TimeSpan DebounceTimeout { get; set; }
    public bool EnableRaisingEvents { get; set; }
    public PinEvent NotifyFilter { get; set; }
    public float PWMDutyCycle { get; set; }
    public int PWMFrequency { get; set; }
    public PWMMode PWMMode { get; set; }
    public int PWMRange { get; set; }
    public event EventHandler<EventArgs> ValueChanged;
    public int AnalogRead();
    public int AnalogReadWait(TimeSpan timeout);
    public void AnalogWrite(int value);
    public new void Dispose();
    public bool ReadValue();
    public bool WaitForEvent(TimeSpan timeout);
    public void WriteValue(bool value);
  }
  public enum PinEvent {
    FallingEdge = 1,
    RisingEdge = 2,
  }
  public enum PinMode {
    I2C = 6,
    Input = 3,
    Output = 4,
    Pull_Down = 0,
    Pull_Up = 1,
    PWM = 2,
    SPI = 5,
    UART = 7,
    Unknown = 8,
  }
  public enum PinNumberingScheme {
    Board = 0,
    Logic = 1,
  }
  public enum PWMMode {
    BALANCED = 1,
    MARK_SPACE = 0,
  }
}
namespace System.Device.Gpio.Samples {
  public static class Examples {
    public static void DefaultDriver_TurnLEDOn();
    public static void DoSomething(object sender, EventArgs args);
    public static void InitializeLightTurnedOn();
    public static void PullUpButton();
    public static void RaspberryPiDriver_TurnLEDOn();
    public static void ReadButton();
    public static void RegisterACallbackWhenButtonPressed();
    public static void WaitForAButtonToBePressed();
  }
}
```

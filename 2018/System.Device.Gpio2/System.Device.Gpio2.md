```C#
using System.Collections.Generic;
using System.Device.Gpio;
using System.Threading;
using System.Threading.Tasks;

namespace System.Device.Gpio
{
    public enum PinNumberingScheme
    {
        Logical,
        Board
    }

    public enum PinMode
    {
        Input,
        Output,
        InputPullDown,
        InputPullUp
    }

    public enum PinValue
    {
        High = 1,
        Low = 0
    }

    public struct WaitForEventResult
    {
        public PinEventTypes EventType;
        public bool TimedOut;
    }

    [Flags]
    public enum PinEventTypes
    {
        Rising = 1,
        Falling = 2
    }

    public class PinValueChangedEventArgs : EventArgs
    {
        public PinValueChangedEventArgs(PinEventTypes changeType, int pinNumber);
        public PinEventTypes ChangeType { get; }
        public int PinNumber { get; }
    }

    public delegate void PinChangeEventHandler(object sender, PinValueChangedEventArgs e);

    public sealed class GpioController : IDisposable
    {
        // Fields
        private GpioDriver _driver;

        // Constructor and singleton getters.
        private GpioController();
        public static GpioController GetController(PinNumberingScheme numberingScheme = PinNumberingScheme.Logical);
        public static GpioController GetController(GpioDriver driver, PinNumberingScheme numberingScheme = PinNumberingScheme.Logical);

        // Controller fields
        public PinNumberingScheme NumberingScheme { get; }
        public int PinCount { get; }

        // Open and close operations on Pins
        public void OpenPin(int pinNumber);
        public void OpenPin(int pinNumber, PinMode mode);
        public void OpenPin(int pinNumber, PinMode mode, PinValue value);
        public void BatchOpenPins(IEnumerable<int> pinNumbers);
        public void BatchOpenPins(IEnumerable<int> pinNumbers, PinMode mode);
        public void ClosePin(int pinNumber);
        public void BatchClosePins(IEnumerable<int> pinNumbers);

        // Read and Write operations on Pins
        public PinValue Read(int pinNumber);
        public void Write(int pinNumber, PinValue value);
        public void Write(IEnumerable<int> pinNumbers, PinValue value);

        // Eventing
        public WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
        public async Task<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout, CancellationToken cancellationToken);
        public void AddCallbackForValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
        public void RemoveCallbackForValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
        
        // Misc functions
        public bool isPinOpen(int pinNumber);
        public bool isPinModeSupported(int pinNumber, PinMode mode);
        public static int GetPinNumberFromConfig(string configurationName, int defaultValue); 
        public void ChangePinMode(int pinNumber, PinMode newPinMode);       
        
        // Dispose and Finalizer
        ~GpioController();
        public void Dispose();

    }

    public static class Utils
    {
        // Checking if Interfaces are enabled or not on the board.
        public static bool isSPIEnabled();
        public static bool isI2CEnabled();
    }

    public abstract class GpioDriver : IDisposable
    {
        protected internal abstract void OpenPin(int pinNumber, PinMode mode);
        protected internal abstract void ClosePin(int pinNumber);
        protected internal abstract int PinCount { get; }
        protected internal abstract PinValue Read(int pinNumber);
        protected internal abstract void Write(int pinNumber, PinValue pinValue);
        protected internal abstract bool isPinModeSupported(int pinNumber, PinMode mode);
        protected internal abstract WaitForEventResult WaitForEvent(PinEventTypes eventType, int timeout);
        protected internal abstract Task<WaitForEventResult> WaitForEventAsync(PinEventTypes eventType, int timeout, CancellationToken cancellationToken);
        public abstract void Dispose();
        protected internal abstract bool EnableRaisingEvents { get; set; }
        protected internal abstract PinEventTypes NotifyFilter { get; set; }
    }
}

namespace System.Device.Pwm
{
    public sealed class PwmController : IDisposable
    {
        // Constructors and Singleton
        private PwmController();
        public static PwmController GetController();
        public static PwmController GetController(PwmDriver driver);

        // Opening and closing channels
        public void OpenChannel(int pwmChannel);
        public void CloseChannel(int pwmChannel);

        // Change duty cycle while pwm is started
        public void ChangeDutyCycle(int pwmChannel, double dutyCycle);

        //PWM
        public void Start(int pwmChannel, int frequency, double dutyCycle);
        public void Stop(int pwmChannel);

        // Others
        public bool isPwmEnabled();

        // Finalizer and Dispose
         ~PwmController();
        public void Dispose();
    }

    public abstract class PwmDriver : IDisposable
    {
        protected internal abstract void OpenChannel(int pwmChannel);
        protected internal abstract void CloseChannel(int pwmChannel);
        protected internal abstract void Start(int pwmChannel, int frequency, double dutyCycle);
        protected internal abstract void Stop(int pwmChannel);
        public abstract void Dispose();
    }
}

namespace System.Device.I2c
{
    public sealed class I2cConnectionSettings
    {
        public uint BusId { get; set; }
        public uint DeviceAddress { get; set; }
        public I2cConnectionSettings(uint busId, uint DeviceAddress);
    }

    public abstract class I2cDevice : IDisposable
    {
        protected I2cConnectionSettings _settings;
        public abstract void Dispose();
        public abstract I2cConnectionSettings GetConnectionSettings();
        public abstract void Read(byte[] buffer, int index, int count);
        public abstract byte ReadByte();
        public abstract ushort ReadUInt16();
        public abstract uint ReadUInt32();
        public abstract ulong ReadUInt64();
        public abstract void Write(byte[] buffer, int index, int count);
        public abstract void WriteByte(byte value);
        public abstract void WriteUInt16(ushort value);
        public abstract void WriteUInt32(uint value);
        public abstract void WriteUInt64(ulong value);
    }
}

namespace System.Device.Spi
{
    public enum SpiMode
    {
        Mode0,
        Mode1,
        Mode2,
        Mode3
    }

    public sealed class SpiConnectionSettings
    {
        public uint BusId { get; set; }
        public uint ChipSelectLine { get; set; }
        public SpiMode Mode {get; set;}
        public uint DataBitLength { get; set; }
        public uint ClockFrequency { get;set; }
        public SpiConnectionSettings(uint busId, uint chipSelectLine);
    }

    public abstract class SpiDevice : IDisposable
    {
        protected SpiConnectionSettings _settings;
        public abstract void Dispose();
        public SpiConnectionSettings GetConnectionSettings();
        public abstract void Read(byte[] buffer, int index, int count);
        public abstract byte ReadByte();
        public abstract ushort ReadUInt16();
        public abstract uint ReadUInt32();
        public abstract ulong ReadUInt64();
        public abstract void Write(byte[] buffer, int index, int count);
        public abstract void WriteByte(byte value);
        public abstract void WriteUInt16(ushort value);
        public abstract void WriteUInt32(uint value);
        public abstract void WriteUInt64(ulong value);
    }
}
```

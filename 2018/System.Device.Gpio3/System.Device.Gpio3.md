```C#
 namespace System.Device.Gpio {
     public sealed class GpioController : IDisposable {
         public PinNumberingScheme NumberingScheme { get; }
         public int PinCount { get; }
         public void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         public void ClosePin(int pinNumber);
         public void Dispose();
         ~GpioController();
         public static GpioController GetController();
         public static GpioController GetController(GpioDriver driver, PinNumberingScheme numberingScheme);
         public static GpioController GetController(PinNumberingScheme numberingScheme);
         public bool IsPinModeSupported(int pinNumber, PinMode mode);
         public bool IsPinOpen(int pinNumber);
         public void OpenPin(int pinNumber);
         public void OpenPin(int pinNumber, PinMode mode);
         public PinValue Read(int pinNumber);
         public void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         public void SetPinMode(int pinNumber, PinMode mode);
         public WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         public ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         public void Write(int pinNumber, PinValue value);
     }
     public abstract class GpioDriver : IDisposable {
         protected GpioDriver();
         protected internal abstract int PinCount { get; }
         protected internal abstract void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         protected internal abstract void ClosePin(int pinNumber);
         protected internal abstract int ConvertPinNumberToLogicalNumberingScheme(int pinNumber);
         public abstract void Dispose();
         protected internal abstract bool IsPinModeSupported(int pinNumber, PinMode mode);
         protected internal abstract void OpenPin(int pinNumber);
         protected internal abstract PinValue Read(int pinNumber);
         protected internal abstract void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         protected internal abstract void SetPinMode(int pinNumber, PinMode mode);
         protected internal abstract WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal abstract ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal abstract void Write(int pinNumber, PinValue value);
     }
     public class GpioException : Exception {
         public GpioException();
         public GpioException(string message);
     }
     internal class HummingboardDriver : GpioDriver {
         public HummingboardDriver();
         protected internal override int PinCount { get; }
         protected internal override void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         protected internal override void ClosePin(int pinNumber);
         protected internal override int ConvertPinNumberToLogicalNumberingScheme(int pinNumber);
         public override void Dispose();
         protected internal override bool IsPinModeSupported(int pinNumber, PinMode mode);
         protected internal override void OpenPin(int pinNumber);
         protected internal override PinValue Read(int pinNumber);
         protected internal override void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         protected internal override void SetPinMode(int pinNumber, PinMode mode);
         protected internal override WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override void Write(int pinNumber, PinValue value);
     }
     public delegate void PinChangeEventHandler(object sender, PinValueChangedEventArgs pinValueChangedEventArgs);
     public enum PinEventTypes {
         Falling = 2,
         None = 0,
         Rising = 1,
     }
     public enum PinMode {
         Input = 0,
         InputPullDown = 2,
         InputPullUp = 3,
         Output = 1,
     }
     public enum PinNumberingScheme {
         Board = 1,
         Logical = 0,
     }
     public enum PinValue {
         High = 1,
         Low = 0,
     }
     public class PinValueChangedEventArgs : EventArgs {
         public PinValueChangedEventArgs(PinEventTypes changeType, int pinNumber);
         public PinEventTypes ChangeType { get; }
         public int PinNumber { get; }
     }
     internal class RaspberryPi3Driver : GpioDriver {
         public RaspberryPi3Driver();
         protected internal override int PinCount { get; }
         protected internal override void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         protected internal override void ClosePin(int pinNumber);
         protected internal override int ConvertPinNumberToLogicalNumberingScheme(int pinNumber);
         public override void Dispose();
         protected internal override bool IsPinModeSupported(int pinNumber, PinMode mode);
         protected internal override void OpenPin(int pinNumber);
         protected internal override PinValue Read(int pinNumber);
         protected internal override void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         protected internal override void SetPinMode(int pinNumber, PinMode mode);
         protected internal override WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override void Write(int pinNumber, PinValue value);
     }
     internal class UnixDriver : GpioDriver {
         public UnixDriver();
         protected internal override int PinCount { get; }
         protected internal override void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         protected internal override void ClosePin(int pinNumber);
         protected internal override int ConvertPinNumberToLogicalNumberingScheme(int pinNumber);
         public override void Dispose();
         protected internal override bool IsPinModeSupported(int pinNumber, PinMode mode);
         protected internal override void OpenPin(int pinNumber);
         protected internal override PinValue Read(int pinNumber);
         protected internal override void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         protected internal override void SetPinMode(int pinNumber, PinMode mode);
         protected internal override WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override void Write(int pinNumber, PinValue value);
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct WaitForEventResult {
         public bool TimedOut;
         public PinEventTypes EventType;
     }
     internal class Windows10Driver : GpioDriver {
         public Windows10Driver();
         protected internal override int PinCount { get; }
         protected internal override void AddCallbackForPinValueChangedEvent(int pinNumber, PinEventTypes eventType, PinChangeEventHandler callback);
         protected internal override void ClosePin(int pinNumber);
         protected internal override int ConvertPinNumberToLogicalNumberingScheme(int pinNumber);
         public override void Dispose();
         protected internal override bool IsPinModeSupported(int pinNumber, PinMode mode);
         protected internal override void OpenPin(int pinNumber);
         protected internal override PinValue Read(int pinNumber);
         protected internal override void RemoveCallbackForPinValueChangedEvent(int pinNumber, PinChangeEventHandler callback);
         protected internal override void SetPinMode(int pinNumber, PinMode mode);
         protected internal override WaitForEventResult WaitForEvent(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override ValueTask<WaitForEventResult> WaitForEventAsync(int pinNumber, PinEventTypes eventType, int timeout);
         protected internal override void Write(int pinNumber, PinValue value);
     }
 }
```

## System.Device.I2c

```C#
 namespace System.Device.I2c {
     public sealed class I2cConnectionSettings {
         public I2cConnectionSettings(uint busId, uint deviceAddress);
         public uint BusId { get; }
         public uint DeviceAddress { get; }
     }
     public interface II2cDevice : IDisposable {
         I2cConnectionSettings GetConnectionSettings();
         void Read(byte[] buffer, int index, int count);
         byte ReadByte();
         ushort ReadUInt16();
         uint ReadUInt32();
         ulong ReadUInt64();
         void Write(byte[] buffer, int index, int count);
         void WriteByte(byte value);
         void WriteUInt16(ushort value);
         void WriteUInt32(uint value);
         void WriteUInt64(ulong value);
     }
     public class UnixI2cDevice : IDisposable, II2cDevice {
         public UnixI2cDevice(I2cConnectionSettings settings);
         public string DevicePath { get; set; }
         public void Dispose();
         public I2cConnectionSettings GetConnectionSettings();
         public void Read(byte[] buffer, int index, int count);
         public byte ReadByte();
         public ushort ReadUInt16();
         public uint ReadUInt32();
         public ulong ReadUInt64();
         public void Write(byte[] buffer, int index, int count);
         public void WriteByte(byte value);
         public void WriteUInt16(ushort value);
         public void WriteUInt32(uint value);
         public void WriteUInt64(ulong value);
     }
 }
```

## System.Device.Pwm

```C#
 namespace System.Device.Pwm {
     public interface IPwmDriver : IDisposable {
         void CloseChannel(int pwmChannel);
         void OpenChannel(int pwmChannel);
         void Start(int pwmChannel, double frequency, double dutyCycle);
         void Stop(int pwmChannel);
     }
     public sealed class PwmController : IDisposable {
         public void ChangeDutyCycle(int pwmChannel, double dutyCycle);
         public void CloseChannel(int pwmChannel);
         public void Dispose();
         ~PwmController();
         public static PwmController GetController();
         public static PwmController GetController(IPwmDriver driver);
         public static bool IsPwmEnabled();
         public void OpenChannel(int pwmChannel);
         public void Start(int pwmChannel, double frequency, double dutyCycle);
         public void Stop(int pwmChannel);
     }
     internal class UnixPwmDriver : IDisposable, IPwmDriver {
         public UnixPwmDriver();
         public void CloseChannel(int pwmChannel);
         public void Dispose();
         public void OpenChannel(int pwmChannel);
         public void Start(int pwmChannel, double frequency, double dutyCycle);
         public void Stop(int pwmChannel);
     }
 }
```

## System.Device.Spi

```C#
 namespace System.Device.Spi {
     public interface ISpiDevice : IDisposable {
         SpiConnectionSettings GetConnectionSettings();
         void Read(byte[] buffer, int index, int count);
         byte ReadByte();
         ushort ReadUInt16();
         uint ReadUInt32();
         ulong ReadUInt64();
         void Write(byte[] buffer, int index, int count);
         void WriteByte(byte value);
         void WriteUInt16(ushort value);
         void WriteUInt32(uint value);
         void WriteUInt64(ulong value);
     }
     public sealed class SpiConnectionSettings {
         public SpiConnectionSettings(uint busId, uint chipSelectLine);
         public uint BusId { get; set; }
         public uint ChipSelectLine { get; set; }
         public uint ClockFrequency { get; set; }
         public uint DataBitLength { get; set; }
         public SpiMode Mode { get; set; }
     }
     public enum SpiMode {
         Mode0 = 0,
         Mode1 = 1,
         Mode2 = 2,
         Mode3 = 3,
     }
     public class UnixSpiDevice : IDisposable, ISpiDevice {
         public UnixSpiDevice(SpiConnectionSettings settings);
         public string DevicePath { get; set; }
         public void Dispose();
         public SpiConnectionSettings GetConnectionSettings();
         public void Read(byte[] buffer, int index, int count);
         public byte ReadByte();
         public ushort ReadUInt16();
         public uint ReadUInt32();
         public ulong ReadUInt64();
         public void Write(byte[] buffer, int index, int count);
         public void WriteByte(byte value);
         public void WriteUInt16(ushort value);
         public void WriteUInt32(uint value);
         public void WriteUInt64(ulong value);
     }
 }
```


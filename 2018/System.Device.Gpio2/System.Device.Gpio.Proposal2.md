# System.Device.Gpio Proposal

The goal of this API is to allow .Net Core developers to access General Purpose
IO (GPIO) pins of IoT devices like Raspberry Pi, Hummingboard, or Odroid. This
will allow applications to read and write data from/to sensors, LEDs and other
kind of peripherals connected to those pins. This API should also support most
commonly used serial communication protocols like SPI and I2C.

## Namespaces

* `System.Device.Gpio`: Will contain the types that allow access to the GPIO
  pins in order to opening and closing pins, reading and writing values to them,
  and subscribing for event notifications.
* `System.Device.Pwm` : Will contain the types that allow access to PWM (Pulse
  Width Modulation). This will allow developers to send square waves over a pin
  in order to simulate analog values from digital outputs.
* `System.Device.I2c` : Will contain the types that allow developers to
  communicate with devices that use the I2C protocol to transfer data.
* `System.Device.Spi` : Will contain the types that allow developers to
  communicate with devices that use the SPI protocol to transfer data.

## Changes vs Iteration 1

This new iteration of the review addresses most of the points that where
discussed on the first iteration of the review. These include:

* Small changes like casing on members, switching member names, and removing
  members and types that are not really needed and where only added as
  extensibility points.
* Some bigger changes implementation-wise like making classes implement the
  singleton pattern, and other changes related to ownership, life-time of
  objects and handling multi-threaded applications.
* Biggest point discussed (after the fact) which changed how the interaction
  with the pins happened and changed most of the API surface : **Removing the
  pin class**

### Pros of a no pin object model

* Using `int` is simpler and pins do not really provide a lot of state
* Discoverability of the surface area (flat surface area)
* Similarity with other frameworks (like RPI.GPIO or WiringPi)
* Simpler model where pins are represented as Integers

### Cons of a no pin object model

* We now have a controller class that takes in a `(int)pin` number as a
  parameter to pretty much all of its methods.
* Eventing (will talk about this later)
* Less Object Oriented

### Examples of both models

#### Turning on a LED

##### No pin object model

```C#
using (GpioController controller = GpioController.GetController())
{
    int led = 5;
    controller.OpenPin(led, PinMode.Output);
    for (int i = 0; i < 5; i++)
    {
        controller.Write(led, PinValue.High);
        Thread.Sleep(TimeSpan.FromSeconds(1));
        controller.Write(led, PinValue.Low);
        Thread.Sleep(TimeSpan.FromSeconds(1));
    }
}
```

##### Pin Object model

```C#
using (GpioController controller = GpioController.GetController())
{
    GpioPin led = controller.OpenPin(5, PinMode.Output);
    for (int i = 0; i < 5; i++)
    {
        led.Write(PinValue.High);
        Thread.Sleep(TimeSpan.FromSeconds(1));
        led.Write(PinValue.Low);
        Thread.Sleep(TimeSpan.FromSeconds(1));
    }
}
```

#### Reading if a button is pressed

##### No pin object model

```C#
using (GpioController controller = GpioController.GetController())
{
    int button = 4;
    controller.OpenPin(button, PinMode.Input);
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine($"Button value is: {controller.Read(button)}");
        Thread.Sleep(TimeSpan.FromSeconds(1));
    }
}
```

##### Pin Object model

```C#
using (GpioController controller = GpioController.GetController())
{
    GpioPin button = controller.OpenPin(4, PinMode.Input);
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine($"Button value is: {button.Read()}");
        Thread.Sleep(TimeSpan.FromSeconds(1));
    }
}
```

#### Waiting for a button to be pressed

##### No pin object model

```C#
using (GpioController controller = GpioController.GetController())
{
    int button = 6;
    controller.OpenPin(button, PinMode.Input);
    WaitForEventResult result = controller.WaitForEvent(button, PinEventTypes.Falling | PinEventTypes.Rising, TimeSpan.FromSeconds(5).Milliseconds);
    Console.WriteLine((result.TimedOut) ? "Timed out waiting for an event" : "Succesfully got an Event!");
}
```

##### Pin Object model

```C#
using (GpioController controller = GpioController.GetController())
{
    GpioPin button = controller.OpenPin(button, PinMode.Input);
    WaitForEventResult result = button.WaitForEvent(PinEventTypes.Falling | PinEventTypes.Rising, TimeSpan.FromSeconds(5).Milliseconds);
    Console.WriteLine((result.TimedOut) ? "Timed out waiting for an event" : "Succesfully got an Event!");
}
```

#### Registering for a callback

##### No pin object model

[Link to a similar model in the framework](https://docs.microsoft.com/en-us/dotnet/api/system.threading.threadpool.registerwaitforsingleobject?view=netcore-2.1)

```C#
using (GpioController controller = GpioController.GetController())
{
    int button = 5;
    controller.OpenPin(button, PinMode.Input);
    controller.AddCallbackForValueChangedEvent(button, PinEventTypes.Rising, OnPinValueChanged);

    Console.WriteLine("Press \'q\' to quit the sample.");
    while(Console.Read()!='q');
}
```

##### Pin Object model

```C#
using (GpioController controller = GpioController.GetController())
{
    GpioPin button = controller.OpenPin(5, PinMode.Input);
    button.NotifyFilter = PinEventTypes.Rising;
    button.OnValueChanged += OnPinValueChanged;
    button.EnableRaisingEvents = true;

    Console.WriteLine("Press \'q\' to quit the sample.");
    while(Console.Read()!='q');
}
```

#### Starting PWM

##### No pin object model

```C#
using (PwmController controller = PwmController.GetController())
{
    int led = 0;
    controller.OpenChannel(led);
    controller.Start(led, 800, 50.0);
    for (int i = 0; i < 5; i++)
    {
        controller.ChangeDutyCycle(led, 50.0);
        Thread.Sleep(TimeSpan.FromSeconds(2));
        controller.ChangeDutyCycle(led, 100.0);
        Thread.Sleep(TimeSpan.FromSeconds(2));
    }
}
```

##### Pin Object model

```C#
using (PwmController controller = PwmController.GetController())
{
    PwmPin led = controller.OpenChannel(led, 800, 50.0);
    led.Start();
    for (int i = 0; i < 5; i++)
    {
        led.DutyCycle = 50.0;
        Thread.Sleep(TimeSpan.FromSeconds(2));
        led.DutyCycle = 100.0;
        Thread.Sleep(TimeSpan.FromSeconds(2));
    }
}
```

### Real world examples

Our real world scenarios are aiming that end developers(people writing apps that
interact with sensors on their boards) won't need to depend on our library.
Instead, we are aiming for a model where in a Board application, there will be
three components. Here are the listed components for the following scenario "I
want to write an app that runs on my Raspberry Pi that talks to a temperature
sensor (BME280) in order to get temperature readings, and then sends this data
over to Azure IoT Edge so that I can get notifications on my phone when
temperature is too low or too high". In this scenario, here are the components
list.

* **Console Application**: This is the one the end-user writes which will talk
  to the sensor and to IoT edge.
* **Sensor Library**: This is going to be a library written in to control BME280
  sensor. It will expose methods like `ReadTemperature()` so that the console
  app can get the data which has already been processed.
* **System.Device.Gpio**: This library will contain our library code which knows
  how to read and write from pins talking in several protocols.

In this scenario, we may ask ourselves the following questions:

* Does my console app need to be aware of the `System.Device.Gpio` library?
* Who owns the setup and tear down of the controller's and pins?
* How does the console app pass in the configuration setup to the sensor
  library?

#### How Console app code would look like in a no pin object model

```C#
//.. some code
using (BME280 myBme280Sensor = new BME280(5, 6, 7, 8)) // <-- we only pass in the ints representing the pins we connected our sensor to.
{
    double currentTemperature = myBme280Sensor.ReadTemperature();
    // .. send data to iot Edge
}
```

#### How Console app code could look like in a pin object model
```C#
using (GpioController controller = GpioController.GetController())
{
    GpioPin pin5 = controller.OpenPin(5);
    GpioPin pin6 = controller.OpenPin(6);
    GpioPin pin7 = controller.OpenPin(7);
    GpioPin pin8 = controller.OpenPin(8);
    using (BME280 myBme280Sensor = new BME280(pin5, pin6, pin7, pin8))
    {
        double currentTemperature = myBme280Sensor.ReadTemperature();
    }
}
```

The above questions become obvious answers in the first snippet, but in the
second one we basically will have sensor libraries that:

* Don't have 1 convention as library devs could decide to take pin objects or
  integers.
* It isn't clear who owns setup and tear down of the resources, so we could end
  up in a state where user initializes the pins in the console app, passes them
  on into the sensor library, which might decide to dispose them, and then the
  console app tries to use them for something else.
* Object ownership isn't very clear
* The second snippet obviously might require that the Console app use the
  `System.Device.Gpio` if the sensor decides to take pin objects in the
  constructor.

For the above reasons, we believe that the approach with no pins is better to the end user.

## Other functionalities of the surface area

#### Specifying Pin numbering scheme

```C#
using (GpioController controller = GpioController.GetController(PinNumberingScheme.Board))
{
    int led = 1; // The led we operate on is the physical (or board) 1st pin.
    controller.OpenPin(led, PinMode.Output);
    //.. do something with the led.
}
```

#### Using a custom driver that supports a new micro controller board

```C#
using (GpioController controller = GpioController.GetController(new MyNewBoardDriver()))
{
    //.. Perform operations with the controller
}
```

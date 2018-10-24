# Xamarin.Essentials

```C#
namespace Xamarin.Essentials {
  public static class Accelerometer {
    public static bool IsMonitoring { get; }
    public static event EventHandler<AccelerometerChangedEventArgs> ReadingChanged;
    // Immo Landwerth: From the review it sounds like events might be raised when the
    //                 value hasn't changed. Consider eating the event.
    public static void Start(SensorSpeed sensorSpeed);
    // Immo Landwerth: Consider throwing InvalidOperationException if monitoring is in
    //                 progrees already.
    public static void Stop();
  }
  public class AccelerometerChangedEventArgs : EventArgs {
    // Immo Landwerth: Consider making the event ctors public so that people can at
    //                 least unit test the event receivers.
    public AccelerometerData Reading { get; }
  }
  public struct AccelerometerData : IEquatable<AccelerometerData> {
    public Vector3 Acceleration { get; }
    public override bool Equals(object obj);
    public bool Equals(AccelerometerData other);
    public override int GetHashCode();
    public static bool operator ==(AccelerometerData left, AccelerometerData right);
    public static bool operator !=(AccelerometerData left, AccelerometerData right);
  }
  public static class AppInfo {
    public static string BuildString { get; }
    public static string Name { get; }
    public static string PackageName { get; }
    public static Version Version { get; }
    public static string VersionString { get; }
    public static void OpenSettings();
    // Immo Landwerth: This open UI of the application's settings page. Does this
    //                 belong on this type? Getting a build number "feels" different
    //                 from this. Also, consider a name that implies UI, e.g. ShowSettings().
    //                 If you have mutliple UI actions, maybe you can group them together.
  }
  public static class Barometer {
    public static bool IsMonitoring { get; }
    public static event EventHandler<BarometerChangedEventArgs> ReadingChanged;
    public static void Start(SensorSpeed sensorSpeed);
    public static void Stop();
  }
  public class BarometerChangedEventArgs : EventArgs {
    public BarometerData Reading { get; }
  }
  public struct BarometerData : IEquatable<BarometerData> {
    public double Pressure { get; }
    // Immo Landwerth: What's the unit?
    public override bool Equals(object obj);
    public bool Equals(BarometerData other);
    public override int GetHashCode();
    public static bool operator ==(BarometerData left, BarometerData right);
    public static bool operator !=(BarometerData left, BarometerData right);
  }
  public static class Battery {
    public static double ChargeLevel { get; }
    // Immo Landwerth: Consider returning 1.0 when the device is using AC. This makes
    //                 common case where folks check against a threshold much simpler.
    //                 Consider percentage in the name as those are customary between
    //                 0.0 and 1.0.
    public static BatteryPowerSource PowerSource { get; }
    public static BatteryState State { get; }
    public static event EventHandler<BatteryChangedEventArgs> BatteryChanged;
  }
  public class BatteryChangedEventArgs : EventArgs {
    public double ChargeLevel { get; }
    public BatteryPowerSource PowerSource { get; }
    public BatteryState State { get; }
  }
  public enum BatteryPowerSource {
    AC = 2,
    Battery = 1,
    Unknown = 0,
    Usb = 3,
    Wireless = 4,
  }
  public enum BatteryState {
    Charging = 1,
    Discharging = 2,
    Full = 3,
    NotCharging = 4,
    NotPresent = 5,
    Unknown = 0,
  }
  public static class Browser {
    // Immo Landwerth: Why are these APIs async? How is this different from
    //                 OpenSettings()? This seems like a fire-and-forget style
    //                 API. Consider return void or at least make it consistent
    //                 with OpenSettings().
    public static Task OpenAsync(string uri);
    public static Task OpenAsync(string uri, BrowserLaunchMode launchMode);
    // Immo Landwerth: Consider optional parameter to communicate the default
    //                 better.
    public static Task OpenAsync(Uri uri);
    public static Task OpenAsync(Uri uri, BrowserLaunchMode launchMode);
  }
  public enum BrowserLaunchMode {
    External = 0,
    SystemPreferred = 1,
    // Immo Landwerth: If this is the default, this should be zero.
  }
  public static class Clipboard {
    // Immo Landwerth: You should make Get and Set consistent, so consider making
    //                 Task SetTextAsync(string text).
    public static bool HasText { get; }
    public static Task<string> GetTextAsync();
    public static void SetText(string text);
  }
  public static class Compass {
    public static bool ApplyLowPassFilter { get; set; }
    public static bool IsMonitoring { get; }
    public static event EventHandler<CompassChangedEventArgs> ReadingChanged;
    public static void Start(SensorSpeed sensorSpeed);
    public static void Stop();
  }
  public class CompassChangedEventArgs : EventArgs {
    public CompassData Reading { get; }
  }
  public struct CompassData : IEquatable<CompassData> {
    public double HeadingMagneticNorth { get; }
    // Immo Landwerth: What are the units? Degrees? Radiants? Snickers?
    public override bool Equals(object obj);
    public bool Equals(CompassData other);
    public override int GetHashCode();
    public static bool operator ==(CompassData left, CompassData right);
    public static bool operator !=(CompassData left, CompassData right);
  }
  public enum ConnectionProfile {
    Bluetooth = 0,
    Cellular = 1,
    Ethernet = 2,
    Other = 5,
    // Immo Landwerth: Consider making this 0 so that when new shows up, Other
    //                 isn't in the middle of your enum. And name it Unknown.
    WiFi = 4,
    WiMAX = 3,
    // Immo Landwerth: Should probably be WiMax.
  }
  public static class Connectivity {
    public static NetworkAccess NetworkAccess { get; }
    public static IEnumerable<ConnectionProfile> Profiles { get; }
    public static event EventHandler<ConnectivityChangedEventArgs> ConnectivityChanged;
  }
  public class ConnectivityChangedEventArgs : EventArgs {
    public NetworkAccess NetworkAccess { get; }
    public IEnumerable<ConnectionProfile> Profiles { get; }
    // Immo Landwerth: It's a bit unclear whether the profiles refers to the affected
    //                 profiles or the profiles that are now available. Maybe AvailableProfiles?
  }
  public static class DataTransfer {
    // Immo Landwerth: Pick a name that developers are more likely to search for. You
    //                 probably want "sharing" somewhere.
    public static Task RequestAsync(string text);
    public static Task RequestAsync(string text, string title);
    public static Task RequestAsync(ShareTextRequest request);
    // Immo Landwerth: Align the name with the type.
  }
  public static class DeviceDisplay {
    // Immo Landwerth: Do may want to think of multiple displays, because that seems
    //                 common enough.
    public static ScreenMetrics ScreenMetrics { get; }
    public static event EventHandler<ScreenMetricsChangedEventArgs> ScreenMetricsChanged;
  }
  public static class DeviceInfo {
    public static DeviceType DeviceType { get; }
    public static string Idiom { get; }
    public static string Manufacturer { get; }
    public static string Model { get; }
    public static string Name { get; }
    public static string Platform { get; }
    public static Version Version { get; }
    public static string VersionString { get; }
    public static class Idioms {
      // Immo Landwerth: Consider making this type a struct that takes string in the ctor,
      //                 this way it's clear what the return value is.
      //                 See https://apisof.net/catalog/System.Runtime.InteropServices.OSPlatform
      //                 Seems like this mostly about getting a handle on screen size and
      //                 input modality. 
      //                 Should this include Watch?
      public const string Desktop = "Desktop";
      public const string Phone = "Phone";
      public const string Tablet = "Tablet";
      public const string TV = "TV";
      public const string Unsupported = "Unsupported";
    }
    public static class Platforms {
      public const string Android = "Android";
      public const string iOS = "iOS";
      public const string Unsupported = "Unsupported";
      public const string UWP = "UWP";
    }
  }
  public enum DeviceType {
    Physical = 0,
    Virtual = 1,
  }
  public enum DistanceUnits {
    Kilometers = 0,
    Miles = 1,
  }
  public static class Email {
    public static Task ComposeAsync();
    public static Task ComposeAsync(string subject, string body, params string[] to);
    public static Task ComposeAsync(EmailMessage message);
  }
  public enum EmailBodyFormat {
    Html = 1,
    PlainText = 0,
  }
  public class EmailMessage {
    public EmailMessage();
    public EmailMessage(string subject, string body, params string[] to);
    public List<string> Bcc { get; set; }
    public string Body { get; set; }
    public EmailBodyFormat BodyFormat { get; set; }
    public List<string> Cc { get; set; }
    public string Subject { get; set; }
    public List<string> To { get; set; }
  }
  public enum EnergySaverStatus {
    Off = 2,
    On = 1,
    Unknown = 0,
  }
  public class EnergySaverStatusChangedEventArgs : EventArgs {
    public EnergySaverStatus EnergySaverStatus { get; }
  }
  public class FeatureNotSupportedException : NotSupportedException {
    public FeatureNotSupportedException();
    public FeatureNotSupportedException(string message);
    public FeatureNotSupportedException(string message, Exception innerException);
  }
  public static class FileSystem {
    public static string AppDataDirectory { get; }
    public static string CacheDirectory { get; }
    public static Task<Stream> OpenAppPackageFileAsync(string filename);
  }
  public static class Flashlight {
    public static Task TurnOffAsync();
    public static Task TurnOnAsync();
  }
  public static class Geocoding {
    public static string MapKey { get; set; }
    public static Task<IEnumerable<Location>> GetLocationsAsync(string address);
    public static Task<IEnumerable<Placemark>> GetPlacemarksAsync(double latitude, double longitude);
    public static Task<IEnumerable<Placemark>> GetPlacemarksAsync(Location location);
  }
  public static class Geolocation {
    public static Task<Location> GetLastKnownLocationAsync();
    public static Task<Location> GetLocationAsync();
    public static Task<Location> GetLocationAsync(GeolocationRequest request);
    public static Task<Location> GetLocationAsync(GeolocationRequest request, CancellationToken cancelToken);
  }
  public enum GeolocationAccuracy {
    Best = 4,
    High = 3,
    Low = 1,
    Lowest = 0,
    Medium = 2,
  }
  public class GeolocationRequest {
    public GeolocationRequest();
    public GeolocationRequest(GeolocationAccuracy accuracy);
    public GeolocationRequest(GeolocationAccuracy accuracy, TimeSpan timeout);
    public GeolocationAccuracy DesiredAccuracy { get; set; }
    public TimeSpan Timeout { get; set; }
  }
  public static class Gyroscope {
    public static bool IsMonitoring { get; }
    public static event EventHandler<GyroscopeChangedEventArgs> ReadingChanged;
    public static void Start(SensorSpeed sensorSpeed);
    public static void Stop();
  }
  public class GyroscopeChangedEventArgs : EventArgs {
    public GyroscopeData Reading { get; }
  }
  public struct GyroscopeData : IEquatable<GyroscopeData> {
    public Vector3 AngularVelocity { get; }
    public override bool Equals(object obj);
    public bool Equals(GyroscopeData other);
    public override int GetHashCode();
    public static bool operator ==(GyroscopeData left, GyroscopeData right);
    public static bool operator !=(GyroscopeData left, GyroscopeData right);
  }
  public static class Launcher {
    public static Task<bool> CanOpenAsync(string uri);
    public static Task<bool> CanOpenAsync(Uri uri);
    public static Task OpenAsync(string uri);
    public static Task OpenAsync(Uri uri);
  }
  public class Locale {
    public string Country { get; }
    public string Id { get; }
    public string Language { get; }
    public string Name { get; }
  }
  public class Location {
    public Location();
    public Location(double latitude, double longitude);
    public Location(double latitude, double longitude, DateTimeOffset timestamp);
    public Location(Location point);
    public Nullable<double> Accuracy { get; set; }
    public Nullable<double> Altitude { get; set; }
    public Nullable<double> Course { get; set; }
    public double Latitude { get; set; }
    public double Longitude { get; set; }
    public Nullable<double> Speed { get; set; }
    public DateTimeOffset TimestampUtc { get; set; }
    public static double CalculateDistance(double latitudeStart, double latitudeEnd, double longitudeStart, double longitudeEnd, DistanceUnits units);
    public static double CalculateDistance(double latitudeStart, double longitudeStart, Location locationEnd, DistanceUnits units);
    public static double CalculateDistance(Location locationStart, double latitudeEnd, double longitudeEnd, DistanceUnits units);
    public static double CalculateDistance(Location locationStart, Location locationEnd, DistanceUnits units);
    public static double KilometersToMiles(double kilometers);
    public static double MilesToKilometers(double miles);
  }
  public static class LocationExtensions {
    public static double CalculateDistance(this Location locationStart, double latitudeEnd, double longitudeEnd, DistanceUnits units);
    public static double CalculateDistance(this Location locationStart, Location locationEnd, DistanceUnits units);
    public static Task OpenMapsAsync(this Location location);
    public static Task OpenMapsAsync(this Location location, MapsLaunchOptions options);
  }
  public static class Magnetometer {
    public static bool IsMonitoring { get; }
    public static event EventHandler<MagnetometerChangedEventArgs> ReadingChanged;
    public static void Start(SensorSpeed sensorSpeed);
    public static void Stop();
  }
  public class MagnetometerChangedEventArgs : EventArgs {
    public MagnetometerData Reading { get; }
  }
  public struct MagnetometerData : IEquatable<MagnetometerData> {
    public Vector3 MagneticField { get; }
    public override bool Equals(object obj);
    public bool Equals(MagnetometerData other);
    public override int GetHashCode();
    public static bool operator ==(MagnetometerData left, MagnetometerData right);
    public static bool operator !=(MagnetometerData left, MagnetometerData right);
  }
  public static class MainThread {
    public static bool IsMainThread { get; }
    public static void BeginInvokeOnMainThread(Action action);
  }
  public enum MapDirectionsMode {
    Bicycling = 1,
    Default = 2,
    Driving = 3,
    None = 0,
    Transit = 4,
    Walking = 5,
  }
  public static class Maps {
    public static Task OpenAsync(double latitude, double longitude);
    public static Task OpenAsync(double latitude, double longitude, MapsLaunchOptions options);
    public static Task OpenAsync(Location location);
    public static Task OpenAsync(Location location, MapsLaunchOptions options);
    public static Task OpenAsync(Placemark placemark);
    public static Task OpenAsync(Placemark placemark, MapsLaunchOptions options);
  }
  public class MapsLaunchOptions {
    public MapsLaunchOptions();
    public MapDirectionsMode MapDirectionsMode { get; set; }
    public string Name { get; set; }
  }
  public enum NetworkAccess {
    ConstrainedInternet = 3,
    Internet = 4,
    Local = 2,
    None = 1,
    Unknown = 0,
  }
  public class NotImplementedInReferenceAssemblyException : NotImplementedException {
    public NotImplementedInReferenceAssemblyException();
  }
  public static class OrientationSensor {
    public static bool IsMonitoring { get; }
    public static event EventHandler<OrientationSensorChangedEventArgs> ReadingChanged;
    public static void Start(SensorSpeed sensorSpeed);
    public static void Stop();
  }
  public class OrientationSensorChangedEventArgs : EventArgs {
    public OrientationSensorData Reading { get; }
  }
  public struct OrientationSensorData : IEquatable<OrientationSensorData> {
    public Quaternion Orientation { get; }
    public override bool Equals(object obj);
    public bool Equals(OrientationSensorData other);
    public override int GetHashCode();
    public static bool operator ==(OrientationSensorData left, OrientationSensorData right);
    public static bool operator !=(OrientationSensorData left, OrientationSensorData right);
  }
  public class PermissionException : UnauthorizedAccessException {
    public PermissionException(string message);
  }
  public static class PhoneDialer {
    public static void Open(string number);
  }
  public class Placemark {
    public Placemark();
    public Placemark(Placemark placemark);
    public string AdminArea { get; set; }
    public string CountryCode { get; set; }
    public string CountryName { get; set; }
    public string FeatureName { get; set; }
    public string Locality { get; set; }
    public Location Location { get; set; }
    public string PostalCode { get; set; }
    public string SubAdminArea { get; set; }
    public string SubLocality { get; set; }
    public string SubThoroughfare { get; set; }
    public string Thoroughfare { get; set; }
  }
  public static class PlacemarkExtensions {
    public static Task OpenMapsAsync(this Placemark placemark);
    public static Task OpenMapsAsync(this Placemark placemark, MapsLaunchOptions options);
  }
  // Immo Landwerth: What is this for?
  public static class Platform {
  }
  public static class Power {
    public static EnergySaverStatus EnergySaverStatus { get; }
    public static event EventHandler<EnergySaverStatusChangedEventArgs> EnergySaverStatusChanged;
  }
  public static class Preferences {
    public static void Clear();
    public static void Clear(string sharedName);
    public static bool ContainsKey(string key);
    public static bool ContainsKey(string key, string sharedName);
    public static bool Get(string key, bool defaultValue);
    public static bool Get(string key, bool defaultValue, string sharedName);
    public static DateTime Get(string key, DateTime defaultValue);
    public static DateTime Get(string key, DateTime defaultValue, string sharedName);
    public static double Get(string key, double defaultValue);
    public static double Get(string key, double defaultValue, string sharedName);
    public static int Get(string key, int defaultValue);
    public static int Get(string key, int defaultValue, string sharedName);
    public static long Get(string key, long defaultValue);
    public static long Get(string key, long defaultValue, string sharedName);
    public static float Get(string key, float defaultValue);
    public static float Get(string key, float defaultValue, string sharedName);
    public static string Get(string key, string defaultValue);
    public static string Get(string key, string defaultValue, string sharedName);
    public static void Remove(string key);
    public static void Remove(string key, string sharedName);
    public static void Set(string key, bool value);
    public static void Set(string key, bool value, string sharedName);
    public static void Set(string key, DateTime value);
    public static void Set(string key, DateTime value, string sharedName);
    public static void Set(string key, double value);
    public static void Set(string key, double value, string sharedName);
    public static void Set(string key, int value);
    public static void Set(string key, int value, string sharedName);
    public static void Set(string key, long value);
    public static void Set(string key, long value, string sharedName);
    public static void Set(string key, float value);
    public static void Set(string key, float value, string sharedName);
    public static void Set(string key, string value);
    public static void Set(string key, string value, string sharedName);
  }
  public static class ScreenLock {
    public static bool IsActive { get; }
    public static void RequestActive();
    public static void RequestRelease();
  }
  public struct ScreenMetrics : IEquatable<ScreenMetrics> {
    public double Density { get; }
    public double Height { get; }
    public ScreenOrientation Orientation { get; }
    public ScreenRotation Rotation { get; }
    public double Width { get; }
    public override bool Equals(object obj);
    public bool Equals(ScreenMetrics other);
    public override int GetHashCode();
    public static bool operator ==(ScreenMetrics left, ScreenMetrics right);
    public static bool operator !=(ScreenMetrics left, ScreenMetrics right);
  }
  public class ScreenMetricsChangedEventArgs : EventArgs {
    public ScreenMetricsChangedEventArgs(ScreenMetrics metrics);
    public ScreenMetrics Metrics { get; }
  }
  public enum ScreenOrientation {
    Landscape = 2,
    Portrait = 1,
    Unknown = 0,
  }
  public enum ScreenRotation {
    Rotation0 = 0,
    Rotation180 = 2,
    Rotation270 = 3,
    Rotation90 = 1,
  }
  public static class SecureStorage {
    public static Task<string> GetAsync(string key);
    public static bool Remove(string key);
    public static void RemoveAll();
    public static Task SetAsync(string key, string value);
  }
  public enum SensorSpeed {
    Fastest = 0,
    Game = 1,
    Normal = 3,
    UI = 2,
  }
  public class ShareTextRequest {
    public ShareTextRequest();
    public ShareTextRequest(string text);
    public ShareTextRequest(string text, string title);
    public string Subject { get; set; }
    public string Text { get; set; }
    public string Title { get; set; }
    public string Uri { get; set; }
  }
  public static class Sms {
    public static Task ComposeAsync();
    public static Task ComposeAsync(SmsMessage message);
  }
  public class SmsMessage {
    public SmsMessage();
    public SmsMessage(string body, string recipient);
    public string Body { get; set; }
    public string Recipient { get; set; }
  }
  public class SpeakSettings {
    public SpeakSettings();
    public Locale Locale { get; set; }
    public Nullable<float> Pitch { get; set; }
    public Nullable<float> Volume { get; set; }
  }
  public static class TextToSpeech {
    public static Task<IEnumerable<Locale>> GetLocalesAsync();
    public static Task SpeakAsync(string text, CancellationToken cancelToken=default(CancellationToken));
    public static Task SpeakAsync(string text, SpeakSettings settings, CancellationToken cancelToken=default(CancellationToken));
  }
  public static class VersionTracking {
    public static IEnumerable<string> BuildHistory { get; }
    public static string CurrentBuild { get; }
    public static string CurrentVersion { get; }
    public static string FirstInstalledBuild { get; }
    public static string FirstInstalledVersion { get; }
    public static bool IsFirstLaunchEver { get; }
    public static bool IsFirstLaunchForCurrentBuild { get; }
    public static bool IsFirstLaunchForCurrentVersion { get; }
    public static string PreviousBuild { get; }
    public static string PreviousVersion { get; }
    public static IEnumerable<string> VersionHistory { get; }
    public static bool IsFirstLaunchForBuild(string build);
    public static bool IsFirstLaunchForVersion(string version);
    public static void Track();
  }
  public static class Vibration {
    public static void Cancel();
    public static void Vibrate();
    public static void Vibrate(double duration);
    public static void Vibrate(TimeSpan duration);
  }
}
```
# Quick Reviews 09/29/2020

## Productize DOM APIs for release.json

**NeedsWork** | [#deployment-tools/35](https://github.com/dotnet/deployment-tools/issues/35#issuecomment-700917853) | [Video](https://www.youtube.com/watch?v=aj5-itpOyvo&t=0h0m0s)

* Provides a public API for the release.json
* Distributed as a NuGet package
* Depends on `Newtonsoft.Json`
* Is `Microsoft.Deployment.Releases` the right namespace? Should we fit into the other namespaces used by the SDK tooling, which AFAIK is in in `Microsoft.DotNet`.
* Currently Windows-only (because it targets .NET Framework) but it can be fully cross-platform
* General notes:
    - Instead of exposing `IEnumerable<T>` use collection types `Collection<T>` and `ReadOnlyCollection<T>` because they avoid leaking the underlying instance that holds the data. It also allows you to derive from to add custom indexers/methods.
    - Using `DateTime` over `DateTimeOffset` for data that has no time component is correct
* `ReleaseIndex`
    - It's a subset of `ReleaseChannel`. It would be better if these types would be merged and the additional data is lazily loaded. For example `Product` would represent the data that is in the root JSON file and `ProductDetails` would contain the entirety of the product that comes from another JSON file. They key is to let the user initiate each separate IO request. Should also return data async.
* `ProductVersion`
    - `ReleaseVersion`
* `ReleaseChannel`
    - `ReleaseChannel` should be named `Product`
    - `SupportPhase` should be an enum
    - It's weird to have `LatestRelease` and `GetLatestRelease()`. Either get rid of the method and make the property return `Release` or remove the properties.
    - Having a nullable `LatestReleaseDate` is odd
    - Shouldn't have public constructor
* `Release`
    - `Runtimes` should be `Components`
    - Having both `Sdk` and `Sdks` is odd. We should only expose `Sdks` because that's well defined
    - `AspNetCoreRuntime` should be `AspNetCoreComponent`
    - `WindowsDesktopRuntime` should be `WindowsDesktopComponent`
* `IRelease`
    - It's odd to have a type called `Release` but not implementing `IRelease`
    - Also, we should try to avoid interfaces and use abstract base types
    - Should be named `ReleaseComponent` and an abstract class
* `SdkRelease`
    - Should be named `SdkReleaseComponent`
* `RuntimeRelease`
    - Should be named `RuntimeReleaseComponent`
* ` AspNetCoreRuntimeRelease`
    - Should be named ` AspNetCoreReleaseComponent`
* ` WindowsDesktopRelease`
    - Should be named ` WindowsDesktopReleaseComponent`

```C#
namespace Microsoft.Deployment.Releases {
    public interface IRelease {
        IEnumerable<ReleaseFile> Files { get; set; }
        string Name { get; }
        ReleaseFlags ReleaseKind { get; }
        ProductVersion Version { get; set; }
    }
    public class AspNetCoreRuntimeRelease : IRelease {
        public AspNetCoreRuntimeRelease();
        public string[] AspNetCoreModuleVersions { get; set; }
        public string DisplayVersion { get; set; }
        public IEnumerable<ReleaseFile> Files { get; set; }
        public string Name { get; }
        public ProductVersion Version { get; set; }
        public string VisualStudioVersion { get; set; }
    }
    public class Cve {
        public Cve();
        public string Id { get; set; }
        public Uri Url { get; set; }
        public override bool Equals(object obj);
        public override int GetHashCode();
    }
    public class ProductVersion : IComparable, IComparable<ProductVersion>, IEquatable<ProductVersion>, ICloneable {
        public ProductVersion(string version);
        public ProductVersion();
        public static readonly string Version2Pattern;
        public string BuildMetadata { get; set; }
        public int Major { get; set; }
        public int Minor { get; set; }
        public int Patch { get; set; }
        public string Prerelease { get; set; }
        public int SdkFeatureBand { get; }
        public int SdkPatchLevel { get; }
        public static int Compare(ProductVersion a, ProductVersion b);
        public static bool Equals(ProductVersion a, ProductVersion b);
        public static bool operator ==(ProductVersion v1, ProductVersion v2);
        public static bool operator >(ProductVersion v1, ProductVersion v2);
        public static bool operator >=(ProductVersion v1, ProductVersion v2);
        public static bool operator !=(ProductVersion v1, ProductVersion v2);
        public static bool operator <(ProductVersion v1, ProductVersion v2);
        public static bool operator <=(ProductVersion v1, ProductVersion v2);
        public object Clone();
        public int ComparePrecedence(ProductVersion value);
        public int CompareTo(object value);
        public int CompareTo(ProductVersion value);
        public bool Equals(ProductVersion obj);
        public bool PrecedenceEquals(ProductVersion value);
        public string ToString(int fieldCount);
        public override bool Equals(object obj);
        public override int GetHashCode();
        public override string ToString();
    }
    public class ProductVersionConverter : JsonConverter<ProductVersion> {
        public ProductVersionConverter();
        public override ProductVersion ReadJson(JsonReader reader, Type objectType, ProductVersion existingValue, bool hasExistingValue, JsonSerializer serializer);
        public override void WriteJson(JsonWriter writer, ProductVersion value, JsonSerializer serializer);
    }
    public class Release {
        public Release();
        public AspNetCoreRuntimeRelease AspNetCoreRuntime { get; set; }
        public IEnumerable<Cve> Cves { get; set; }
        public IEnumerable<ReleaseFile> Files { get; }
        public bool IsPreview { get; }
        public bool IsSecurityUpdate { get; set; }
        public DateTime ReleaseDate { get; set; }
        public Uri ReleaseNotes { get; set; }
        public ProductVersion ReleaseVersion { get; set; }
        public RuntimeRelease Runtime { get; set; }
        public IEnumerable<IRelease> Runtimes { get; }
        public SdkRelease Sdk { get; set; }
        public IEnumerable<SdkRelease> Sdks { get; set; }
        public WindowsDesktopRelease WindowsDesktop { get; set; }
    }
    public class ReleaseChannel {
        public ReleaseChannel();
        public string ChannelVersion { get; set; }
        public DateTime? EolDate { get; set; }
        public bool IsOutOfSupport { get; }
        public ProductVersion LatestRelease { get; set; }
        public DateTime? LatestReleaseDate { get; set; }
        public ProductVersion LatestRuntime { get; set; }
        public ProductVersion LatestSdk { get; set; }
        public Uri LifeCyclePolicyUrl { get; set; }
        public IEnumerable<Release> Releases { get; set; }
        public SupportPhase SupportPhase { get; set; }
        public Release GetLatestRelease();
        public Release GetLatestRelease(bool isSecurityUpdate);
    }
    public class ReleaseFile {
        public ReleaseFile();
        public string FileName { get; }
        public string Hash { get; }
        public string Name { get; }
        public string Rid { get; }
        public Uri Url { get; }
        public void Download(string fileName);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }
    public class ReleaseIndex {
        public ReleaseIndex();
        public string ChannelVersion { get; set; }
        public DateTime? EolDate { get; set; }
        public bool IsOutOfSupport { get; }
        public bool IsSecurity { get; set; }
        public ProductVersion LatestRelease { get; set; }
        public DateTime? LatestReleaseDate { get; set; }
        public ProductVersion LatestRuntime { get; set; }
        public ProductVersion LatestSdk { get; set; }
        public string Product { get; set; }
        public string ReleasesJson { get; set; }
        public SupportPhase SupportPhase { get; set; }
    }
    public class Releases {
        public Releases();
        public static readonly Uri ReleasesIndexJsonUri;
        public int ChannelCount { get; }
        public IEnumerable<ReleaseChannel> Channels { get; }
        public IEnumerable<string> ChannelVersions { get; }
        public IEnumerable<ReleaseIndex> Index { get; set; }
        public ReleaseChannel this[string channelVersion] { get; }
        public static Releases CreateFromDefaultUrl();
        public static Releases CreateFromFile(string releasesIndexPath);
        public static Releases CreateFromFile(string releasesIndexPath, bool useLatest);
        public static Releases CreateFromUrl(Uri uri);
        public bool ContainsChannel(string channelVersion);
        public Release GetRelease(ProductVersion releaseVersion);
        public IEnumerable<Release> GetReleases(string cveId);
    }
    public class ReleasesHelpers {
        public ReleasesHelpers();
        public static T Create<T>(Uri jsonUrl);
        public static T CreateFromFile<T>(string path);
    }
    public class RuntimeRelease : IRelease {
        public RuntimeRelease();
        public ProductVersion DisplayVersion { get; set; }
        public IEnumerable<ReleaseFile> Files { get; set; }
        public string Name { get; }
        public ProductVersion Version { get; set; }
        public string VisualStudioMacVersion { get; set; }
        public string VisualStudioVersion { get; set; }
    }
    public class SdkRelease : IRelease {
        public SdkRelease();
        public string CSharpVersion { get; set; }
        public ProductVersion DisplayVersion { get; set; }
        public IEnumerable<ReleaseFile> Files { get; set; }
        public string FSharpVersion { get; set; }
        public string Name { get; }
        public ProductVersion RuntimeVersion { get; set; }
        public ProductVersion Version { get; set; }
        public string VisualStudioMacSupport { get; set; }
        public string VisualStudioMacVersion { get; set; }
        public string VisualStudioSupport { get; set; }
        public string VisualStudioVersion { get; set; }
    }
    public class WindowsDesktopRelease : IRelease {
        public WindowsDesktopRelease();
        public string DisplayVersion { get; set; }
        public IEnumerable<ReleaseFile> Files { get; set; }
        public string Name { get; }
        public ProductVersion Version { get; set; }
    }
    [Flags]
    public enum ReleaseFlags {
        None = 0,
        Sdk = 1,
        Runtime = 2,
        AspNetCoreRuntime = 4,
        WindowsDesktopRuntime = 8,
    }
    public enum SupportPhase {
        Undefined = 0,
        [EnumMember]
        EOL = 1,
        [EnumMember]
        LTS = 2,
        [EnumMember]
        Maintenance = 3,
        [EnumMember]
        Preview = 4,
    }
}
```


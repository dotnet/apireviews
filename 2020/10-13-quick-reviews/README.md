# Quick Reviews 10/13/2020

## Productize DOM APIs for release.json

**Approved** | [#deployment-tools/35](https://github.com/dotnet/deployment-tools/issues/35#issuecomment-707946719) | [Video](https://www.youtube.com/watch?v=G2ohWFyKnVc&t=0h0m0s)

* Pick a strategy: either expose `IReadOnlyXxx` or use `Collection`/`ReadOnlyCollection<T>` types but don't do both
* Some types of the object model have (public) constructors while others don't. Pick one model.
* Is it possible that consumers of this API can observe inconsistencies when files are updated independently? Should this be done via a versioning scheme or simply by serving the files from a GitHub repo (so long we make sure that commits are consistent).
* `ProductCollection`
    - `ReleaseIndexDefaultUrl` should be a get-only property
    - `CreateAsync` is odd. How about `GetAsync()`?
    - The parameters shouldn't use name `Url` or `Json`.
    - Don't overload between file paths and URLs (because we often also add string based overloads for URI based APIs)
    - Add a string-based overload for URI
    - Add `GetFromFile` for the file-based one
* `Product`
    - `IsSecurityUpdate`. Should at least be renamed `LatestReleaseIsSecurityUpdate` but we should consider removing it because the SDK has feature trains `5.0.100` and `5.0.200` that are for different VS releases. In which case only 200 is latest, which produce confusing/misleading results. It also seems the property is prone to errors for folks that want to check whether they are missing a security update (because this property is only true when the very latest release is a security update).
    - The static `GetReleasesAsync(Uri releasesJsonUrl)` method should move to `ProductRelease`
* `SupportPhase`
    - We should probably not surface serialization attributes
    - What does `Maintenance` mean?
    - `Current` is missing
* `ProductRelease`
    - Whatever policy you have on the `Product`, such as whether something is go-live `SupportPhase` it seems the releases should be able to answer any questions as well, because releases are logically a point in time for a product. For example, we expose `IsPreview` but not `IsGoLive`.
    - Considering a reference back to `Product`
* `ReleaseComponent`
    - `DisplayVersion` should be a `string`
    - `Runtimes` should be named `AllRuntimes` and return `IReadOnlyCollection<T>`
* `ReleaseVersion`
    - This type is mutable, which we probably don't want. We should also remove `Clone()` and `ICloneable`
    - You have `ComparePrecedence` and `PrecedenceEquals`. It seems more consistent to use `PrecedenceCompareTo` and `PrecedenceEquals`
* `ReleaseFile`
    - We should make sure that hash validation is the default.
    - When the validation fails we should delete the file
    - We should negate the parameter to `skipHashValidation`
    - Should we consider dropping the option of skipping entirely?
    - Consider using `System.IO.InvalidDataException`
    - The `fileName` parameter is a bit ambiguous because the instance also has one. Maybe `path` or `destinationPath`?
    - Should probably implement `IEquatable<>`
* `ReleaseVersionConverter` and `SupportPhaseConverter`
    - Should probably be internal

```C#
namespace Microsoft.Deployment.DotNet.Releases {
    public class AspNetCoreReleaseComponent : ReleaseComponent {
        public IReadOnlyCollection<string> AspNetCoreModuleVersions { get; }
        public string VisualStudioVersion { get; }
    }
    public class Cve {
        public Cve();
        public string Id { get; }
        public Uri Url { get; }
        public override bool Equals(object obj);
        public override int GetHashCode();
    }
    public class Product {
        public Product();
        public DateTime? EndOfLifeDate { get; }
        public bool IsSecurityUpdate { get; }
        public DateTime? LatestReleaseDate { get; }
        public ReleaseVersion LatestReleaseVersion { get; }
        public ReleaseVersion LatestRuntimeVersion { get; }
        public ReleaseVersion LatestSdkVersion { get; }
        public string ProductName { get; }
        public string ProductVersion { get; }
        public Uri ReleasesJson { get; }
        public SupportPhase SupportPhase { get; }
        public static Task<ReadOnlyCollection<ProductRelease>> GetReleasesAsync(Uri releasesJsonUrl);
        public Task<ReadOnlyCollection<ProductRelease>> GetReleasesAsync();
        public Task<ReadOnlyCollection<ProductRelease>> GetReleasesAsync(string releasesIndexJsonPath, bool downloadLatest);
        public bool IsOutOfSupport();
    }
    public sealed class ProductCollection : ReadOnlyCollection<Product> {
        public static readonly Uri ReleaseIndexDefaultUrl;
        public static Task<ProductCollection> CreateAsync();
        public static Task<ProductCollection> CreateAsync(string releasesIndexJsonPath, bool downloadLatest);
        public static Task<ProductCollection> CreateAsync(Uri releasesIndexUrl);
        public IEnumerable<SupportPhase> GetSupportPhases();
    }
    public class ProductRelease {
        public AspNetCoreReleaseComponent AspNetCoreRuntime { get; }
        public IReadOnlyCollection<ReleaseComponent> Components { get; }
        public IReadOnlyCollection<Cve> Cves { get; }
        public IReadOnlyCollection<ReleaseFile> Files { get; }
        public bool IsPreview { get; }
        public bool IsSecurityUpdate { get; }
        public DateTime ReleaseDate { get; }
        public Uri ReleaseNotes { get; }
        public RuntimeReleaseComponent Runtime { get; }
        public IEnumerable<ReleaseComponent> Runtimes { get; }
        public IReadOnlyCollection<SdkReleaseComponent> Sdks { get; }
        public ReleaseVersion Version { get; }
        public WindowsDesktopReleaseComponent WindowsDesktopRuntime { get; }
    }
    public abstract class ReleaseComponent {
        public ReleaseVersion DisplayVersion { get; }
        public IReadOnlyCollection<ReleaseFile> Files { get; }
        public string Name { get; protected set; }
        public ProductRelease Release { get; }
        public ReleaseVersion Version { get; }
    }
    public class ReleaseFile {
        public ReleaseFile();
        public string FileName { get; }
        public string Hash { get; }
        public string Name { get; }
        public string Rid { get; }
        public Uri Url { get; }
        public Task DownloadAsync(string fileName);
        public Task DownloadAsync(string fileName, bool verifyHash);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }
    public class ReleaseVersion : IComparable, IComparable<ReleaseVersion>, IEquatable<ReleaseVersion>, ICloneable {
        public ReleaseVersion(string version);
        public ReleaseVersion();
        public static readonly string Version2Pattern;
        public string BuildMetadata { get; set; }
        public int Major { get; set; }
        public int Minor { get; set; }
        public int Patch { get; set; }
        public string Prerelease { get; set; }
        public int SdkFeatureBand { get; }
        public int SdkPatchLevel { get; }
        public static int Compare(ReleaseVersion a, ReleaseVersion b);
        public static bool Equals(ReleaseVersion a, ReleaseVersion b);
        public static bool operator ==(ReleaseVersion a, ReleaseVersion b);
        public static bool operator >(ReleaseVersion a, ReleaseVersion b);
        public static bool operator >=(ReleaseVersion a, ReleaseVersion b);
        public static bool operator !=(ReleaseVersion a, ReleaseVersion b);
        public static bool operator <(ReleaseVersion a, ReleaseVersion b);
        public static bool operator <=(ReleaseVersion a, ReleaseVersion b);
        public object Clone();
        public int ComparePrecedence(ReleaseVersion value);
        public int CompareTo(object value);
        public int CompareTo(ReleaseVersion value);
        public bool Equals(ReleaseVersion obj);
        public bool PrecedenceEquals(ReleaseVersion value);
        public string ToString(int fieldCount);
        public override bool Equals(object obj);
        public override int GetHashCode();
        public override string ToString();
    }
    public class ReleaseVersionConverter : JsonConverter<ReleaseVersion> {
        public ReleaseVersionConverter();
        public override ReleaseVersion ReadJson(JsonReader reader, Type objectType, ReleaseVersion existingValue, bool hasExistingValue, JsonSerializer serializer);
        public override void WriteJson(JsonWriter writer, ReleaseVersion value, JsonSerializer serializer);
    }
    public class RuntimeReleaseComponent : ReleaseComponent {
        public string VisualStudioMacVersion { get; }
        public string VisualStudioVersion { get; }
    }
    public class SdkReleaseComponent : ReleaseComponent {
        public string CSharpVersion { get; }
        public string FSharpVersion { get; }
        public ReleaseVersion RuntimeVersion { get; }
        public string VisualBasicVersion { get; }
        public string VisualStudioMacSupport { get; }
        public string VisualStudioMacVersion { get; }
        public string VisualStudioSupport { get; }
        public string VisualStudioVersion { get; }
    }
    public class SupportPhaseConverter : StringEnumConverter {
        public SupportPhaseConverter();
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    }
    public class WindowsDesktopReleaseComponent : ReleaseComponent {
    }
    public enum SupportPhase {
        Unknown = 0,
        [EnumMember]
        EndOfLife = 1,
        Maintenance = 2,
        [EnumMember]
        LongTermSupport = 3,
        Preview = 4,
        RC = 5,
    }
}
```



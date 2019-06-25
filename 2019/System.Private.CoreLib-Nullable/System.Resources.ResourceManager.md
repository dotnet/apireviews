# System.Resources.ResourceManager

```diff
---D:\Temp\nullable\before
+++D:\Temp\nullable\after
 assembly System.Resources.ResourceManager {
  namespace System.Resources {
    public interface IResourceReader : IDisposable, IEnumerable {
      void Close();
      IDictionaryEnumerator GetEnumerator();
    }
    public class MissingManifestResourceException : SystemException {
      public MissingManifestResourceException();
      protected MissingManifestResourceException(SerializationInfo info, StreamingContext context);
      public MissingManifestResourceException(string message);
      public MissingManifestResourceException(string message, Exception inner);
    }
    public class MissingSatelliteAssemblyException : SystemException {
      public MissingSatelliteAssemblyException();
      protected MissingSatelliteAssemblyException(SerializationInfo info, StreamingContext context);
      public MissingSatelliteAssemblyException(string message);
      public MissingSatelliteAssemblyException(string message, Exception inner);
      public MissingSatelliteAssemblyException(string message, string cultureName);
      public string CultureName { get; }
    }
    public sealed class NeutralResourcesLanguageAttribute : Attribute {
      public NeutralResourcesLanguageAttribute(string cultureName);
      public NeutralResourcesLanguageAttribute(string cultureName, UltimateResourceFallbackLocation location);
      public string CultureName { get; }
      public UltimateResourceFallbackLocation Location { get; }
    }
    public class ResourceManager {
      public static readonly int HeaderVersionNumber;
      public static readonly int MagicNumber;
      protected Assembly MainAssembly;
      protected ResourceManager();
      public ResourceManager(string baseName, Assembly assembly);
      public ResourceManager(string baseName, Assembly assembly, Type usingResourceSet);
      public ResourceManager(Type resourceSource);
      public virtual string BaseName { get; }
      protected UltimateResourceFallbackLocation FallbackLocation { get; set; }
      public virtual bool IgnoreCase { get; set; }
      public virtual Type ResourceSetType { get; }
      public static ResourceManager CreateFileBasedResourceManager(string baseName, string resourceDir, Type usingResourceSet);
      protected static CultureInfo GetNeutralResourcesLanguage(Assembly a);
      public virtual object GetObject(string name);
      public virtual object GetObject(string name, CultureInfo culture);
      protected virtual string GetResourceFileName(CultureInfo culture);
      public virtual ResourceSet GetResourceSet(CultureInfo culture, bool createIfNotExists, bool tryParents);
      protected static Version GetSatelliteContractVersion(Assembly a);
      public UnmanagedMemoryStream GetStream(string name);
      public UnmanagedMemoryStream GetStream(string name, CultureInfo culture);
      public virtual string GetString(string name);
      public virtual string GetString(string name, CultureInfo culture);
      protected virtual ResourceSet InternalGetResourceSet(CultureInfo culture, bool createIfNotExists, bool tryParents);
      public virtual void ReleaseAllResources();
    }
    public sealed class ResourceReader : IDisposable, IEnumerable, IResourceReader {
      public ResourceReader(Stream stream);
      public ResourceReader(string fileName);
      public void Close();
      public void Dispose();
      public IDictionaryEnumerator GetEnumerator();
      public void GetResourceData(string resourceName, out string resourceType, out byte[] resourceData);
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
    public class ResourceSet : IDisposable, IEnumerable {
      protected ResourceSet();
      public ResourceSet(Stream stream);
      public ResourceSet(IResourceReader reader);
      public ResourceSet(string fileName);
      public virtual void Close();
      public void Dispose();
      protected virtual void Dispose(bool disposing);
      public virtual Type GetDefaultReader();
      public virtual Type GetDefaultWriter();
      public virtual IDictionaryEnumerator GetEnumerator();
      public virtual object GetObject(string name);
      public virtual object GetObject(string name, bool ignoreCase);
      public virtual string GetString(string name);
      public virtual string GetString(string name, bool ignoreCase);
      protected virtual void ReadResources();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
    public sealed class SatelliteContractVersionAttribute : Attribute {
      public SatelliteContractVersionAttribute(string version);
      public string Version { get; }
    }
    public enum UltimateResourceFallbackLocation {
      MainAssembly = 0,
      Satellite = 1,
    }
  }
 }
```

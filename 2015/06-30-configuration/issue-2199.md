```diff
---C:\Users\divega\Desktop\Configuration and Options assemblies\dotnet
   assembly Microsoft.Framework.Configuration {
  namespace Microsoft.Framework.Configuration {
    public class ConfigurationBuilder : IConfigurationBuilder {
      public ConfigurationBuilder(params IConfigurationSource[] sources);
      public ConfigurationBuilder(string basePath, params IConfigurationSource[] sources);
      public string BasePath { get; }
      public IEnumerable<IConfigurationSource> Sources { get; }
      public IConfigurationBuilder Add(IConfigurationSource configurationSource);
      public IConfigurationBuilder Add(IConfigurationSource configurationSource, bool load);
      public IConfiguration Build();
    }
    public class ConfigurationKeyComparer : IComparer<string> {
      public ConfigurationKeyComparer();
      public static ConfigurationKeyComparer Instance { get; }
      public int Compare(string x, string y);
    }
    public class ConfigurationSection : IConfiguration {
      public ConfigurationSection(IList<IConfigurationSource> sources);
      public IEnumerable<IConfigurationSource> Sources { get; }
      public string this[string key] { get; set; }
      public string Get(string key);
      public IConfiguration GetConfigurationSection(string key);
      public IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections();
      public IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections(string key);
      public void Reload();
      public void Set(string key, string value);
      public bool TryGet(string key, out string value);
    }
    public abstract class ConfigurationSource : IConfigurationSource {
      protected ConfigurationSource();
      protected IDictionary<string, string> Data { get; set; }
      public virtual void Load();
      public virtual IEnumerable<string> ProduceConfigurationSections(IEnumerable<string> earlierKeys, string prefix, string delimiter);
      public virtual void Set(string key, string value);
      public virtual bool TryGet(string key, out string value);
    }
    public static class Constants {
      public static readonly string KeyDelimiter;
    }
    public class MemoryConfigurationSource : ConfigurationSource, IEnumerable, IEnumerable<KeyValuePair<string, string>> {
      public MemoryConfigurationSource();
      public MemoryConfigurationSource(IEnumerable<KeyValuePair<string, string>> initialData);
      public void Add(string key, string value);
      public IEnumerator<KeyValuePair<string, string>> GetEnumerator();
      IEnumerator System.Collections.IEnumerable.GetEnumerator();
    }
  }
  namespace Microsoft.Framework.Configuration.Helper {
    public static class ConfigurationHelper {
      public static string ResolveConfigurationFilePath(IConfigurationBuilder configuration, string path);
    }
  }
  namespace Microsoft.Framework.Configuration.Internal {
    public class ConfigurationFocus : IConfiguration {
      public ConfigurationFocus(IConfiguration root, string prefix);
      public string this[string key] { get; set; }
      public string Get(string key);
      public IConfiguration GetConfigurationSection(string key);
      public IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections();
      public IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections(string key);
      public void Reload();
      public void Set(string key, string value);
      public bool TryGet(string key, out string value);
    }
  }
 }
 assembly Microsoft.Framework.Configuration.Abstractions {
  namespace Microsoft.Framework.Configuration {
    public interface IConfiguration {
      ^ Immo Landwerth: Should this implement IE<KV<string, string>>? This would help in the debugger and also allow foreach/Linq queries.
      ^ Immo Landwerth: How does one get all the keys? A: You'd use GetChildren().
      string this[string key] { get; set; }
      ^ Immo Landwerth: Returns null if the key cannot be found. This might have an issue where the store contains a key, but the value is actually null.
      string Get(string key);
      ^ Immo Landwerth: Remove
      IConfiguration GetConfigurationSection(string key);
      IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections();
      ^ Immo Landwerth: GetChildren
      IEnumerable<KeyValuePair<string, IConfiguration>> GetConfigurationSections(string key);
      ^ Immo Landwerth: GetChildren
      void Reload();
      ^ Immo Landwerth: Should this return a new IConfiguration object? There is already a setter, so technically it's not immutable. But it should it be?
      void Set(string key, string value);
      ^ Immo Landwerth: Remove
      bool TryGet(string key, out string value);
    }
    public interface IConfigurationBuilder {
      string BasePath { get; }
      IEnumerable<IConfigurationSource> Sources { get; }
      IConfigurationBuilder Add(IConfigurationSource configurationSource);
      IConfiguration Build();
    }
    public interface IConfigurationSource {
      void Load();
      IEnumerable<string> ProduceConfigurationSections(IEnumerable<string> earlierKeys, string prefix, string delimiter);
      void Set(string key, string value);
      bool TryGet(string key, out string value);
    }
  }
 }
 assembly Microsoft.Framework.Configuration.Binder {
  namespace Microsoft.Framework.Configuration {
    public static class ConfigurationBinder {
      public static void Bind(object model, IConfiguration configuration);
      public static TModel Bind<TModel>(IConfiguration configuration) where TModel : new();
    }
  }
 }
 assembly Microsoft.Framework.Configuration.CommandLine {
  namespace Microsoft.Framework.Configuration {
    public static class CommandLineConfigurationExtension {
      public static IConfigurationBuilder AddCommandLine(this IConfigurationBuilder configuration, string[] args);
      public static IConfigurationBuilder AddCommandLine(this IConfigurationBuilder configuration, string[] args, IDictionary<string, string> switchMappings);
    }
  }
  namespace Microsoft.Framework.Configuration.CommandLine {
    public class CommandLineConfigurationSource : ConfigurationSource {
      public CommandLineConfigurationSource(IEnumerable<string> args, IDictionary<string, string> switchMappings=null);
      protected IEnumerable<string> Args { get; }
      public override void Load();
    }
  }
 }
 assembly Microsoft.Framework.Configuration.EnvironmentVariables {
  namespace Microsoft.Framework.Configuration {
    public static class EnvironmentVariablesExtension {
      public static IConfigurationBuilder AddEnvironmentVariables(this IConfigurationBuilder configuration);
      public static IConfigurationBuilder AddEnvironmentVariables(this IConfigurationBuilder configuration, string prefix);
    }
  }
  namespace Microsoft.Framework.Configuration.EnvironmentVariables {
    public class EnvironmentVariablesConfigurationSource : ConfigurationSource {
      public EnvironmentVariablesConfigurationSource();
      public EnvironmentVariablesConfigurationSource(string prefix);
      public override void Load();
    }
  }
 }
 assembly Microsoft.Framework.Configuration.Ini {
  namespace Microsoft.Framework.Configuration {
    public static class IniConfigurationExtension {
      public static IConfigurationBuilder AddIniFile(this IConfigurationBuilder configuration, string path);
      public static IConfigurationBuilder AddIniFile(this IConfigurationBuilder configuration, string path, bool optional);
    }
  }
  namespace Microsoft.Framework.Configuration.Ini {
    public class IniFileConfigurationSource : ConfigurationSource {
      public IniFileConfigurationSource(string path);
      public IniFileConfigurationSource(string path, bool optional);
      public bool Optional { get; }
      public string Path { get; }
      public override void Load();
    }
  }
 }
 assembly Microsoft.Framework.Configuration.Json {
  namespace Microsoft.Framework.Configuration {
    public static class JsonConfigurationExtension {
      public static IConfigurationBuilder AddJsonFile(this IConfigurationBuilder configuration, string path);
      public static IConfigurationBuilder AddJsonFile(this IConfigurationBuilder configuration, string path, bool optional);
    }
    public class JsonConfigurationSource : ConfigurationSource {
      public JsonConfigurationSource(string path);
      public JsonConfigurationSource(string path, bool optional);
      public bool Optional { get; }
      public string Path { get; }
      public override void Load();
    }
  }
 }
 assembly Microsoft.Framework.Configuration.Xml {
  namespace Microsoft.Framework.Configuration {
    public static class XmlConfigurationExtension {
      public static IConfigurationBuilder AddXmlFile(this IConfigurationBuilder configuration, string path);
      public static IConfigurationBuilder AddXmlFile(this IConfigurationBuilder configuration, string path, bool optional);
    }
    public class XmlConfigurationSource : ConfigurationSource {
      public XmlConfigurationSource(string path);
      public bool Optional { get; }
      public string Path { get; }
      public override void Load();
    }
  }
 }
 assembly Microsoft.Framework.OptionsModel {
  namespace Microsoft.Framework.DependencyInjection {
    public static class OptionsServiceCollectionExtensions {
      public static IServiceCollection AddOptions(this IServiceCollection services);
      public static IServiceCollection Configure<TOptions>(this IServiceCollection services, IConfiguration config, int order=-1000, string optionsName="");
      public static IServiceCollection Configure<TOptions>(this IServiceCollection services, IConfiguration config, string optionsName);
      public static IServiceCollection Configure<TOptions>(this IServiceCollection services, Action<TOptions> setupAction, int order=0, string optionsName="");
      public static IServiceCollection Configure<TOptions>(this IServiceCollection services, Action<TOptions> setupAction, string optionsName);
      public static IServiceCollection ConfigureOptions(this IServiceCollection services, object configureInstance);
      public static IServiceCollection ConfigureOptions(this IServiceCollection services, Type configureType);
      public static IServiceCollection ConfigureOptions<TSetup>(this IServiceCollection services);
    }
  }
  namespace Microsoft.Framework.OptionsModel {
    public class ConfigureFromConfigurationOptions<TOptions> : ConfigureOptions<TOptions> {
      public ConfigureFromConfigurationOptions(IConfiguration config);
    }
    public class ConfigureOptions<TOptions> : IConfigureOptions<TOptions> {
      public ConfigureOptions(Action<TOptions> action);
      public Action<TOptions> Action { get; }
      public string Name { get; set; }
      public virtual int Order { get; set; }
      public virtual void Configure(TOptions options, string name="");
    }
    public interface IConfigureOptions<in TOptions> {
      int Order { get; }
      void Configure(TOptions options, string name="");
    }
    public interface IOptions<out TOptions> where TOptions : class, new() {
      TOptions Options { get; }
      TOptions GetNamedOptions(string name);
    }
    public class OptionsManager<TOptions> : IOptions<TOptions> where TOptions : class, new() {
      public OptionsManager(IEnumerable<IConfigureOptions<TOptions>> setups);
      public virtual TOptions Options { get; }
      public virtual TOptions Configure(string optionsName="");
      public virtual TOptions GetNamedOptions(string name);
    }
  }
 }
```
# Sample Usages

For related issues see the [ASP.NET repo](https://github.com/aspnet/Configuration/issues?utf8=%E2%9C%93&q=is%3Aissue+bcl+).

## Configuration sample usage

```CSharp
// providers setup
var configuration =
   new ConfigurationBuilder(appEnv.ApplicationBasePath)
       .AddJsonFile("config.json")
       .AddEnvironmentVariables()
       .Build();

// reading a value
var connectionString = configuration["Data:DefaultConnection:ConnectionString"];

// writing a value (to memory)
configuration.Set("AppSettings:BackgroundColor", "Blue");

// accessing a group of values
var applicationSettings = configuration.GetSection("AppSettings");
```

## Options sample usage

```CSharp
// setup with configuration binding
services.Configure<AppSettings>(Configuration.GetSection("AppSettings"));

// setup with an Action<T>
services.Configure<AppSettings>(as => as.BackgrounColorName = "Blue");

// access
public class Window
{
    private string _backgroundColorName;

    public Window(IOptions<AppSettings> appSettings)
    {
        _backgroundColorName = appSettings.Options.BackgroundColorName;
    }
...
```

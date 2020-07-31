# Quick Reviews 07/31/2020

## ConsoleLogger output is garbled with color codes when redirected

**Approved** | [#runtime/37421](https://github.com/dotnet/runtime/issues/37421#issuecomment-667250129) | [Video](https://www.youtube.com/watch?v=wOytg5ukJ2Y&t=0h0m0s)

* Looks good as proposed
* We considered exposing a setting on `System.Console` instead but we want a setting that can be configured via the configuration system of `Microsoft.Extensions`.

```diff
 namespace Microsoft.Extensions.Logging.Console
 {
+    public enum LoggerColorBehavior
+    {
+        Default = 0,
+        Enabled = 1,
+        Disabled = 2,
+    }
     public class SimpleConsoleFormatterOptions : ConsoleFormatterOptions
     {
         public SimpleConsoleFormatterOptions();
-        public bool DisableColors { get; set; }
+        public LoggerColorBehavior ColorBehavior { get; set; }
         public bool SingleLine { get; set; }
     }
 }
```


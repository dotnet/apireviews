# System.Runtime.InteropServices.WindowsRuntime

```diff
--- D:\Temp\nullable\before\System.Runtime.InteropServices.WindowsRuntime.dll.cs	2019-06-26 13:40:22.000000000 -0700
+++ D:\Temp\nullable\after\System.Runtime.InteropServices.WindowsRuntime.dll.cs	2019-06-26 13:40:41.000000000 -0700
@@ -15,12 +15,13 @@
         public static bool operator ==(System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken left, System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken right) { throw null; }
         public static bool operator !=(System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken left, System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken right) { throw null; }
     }
     public sealed partial class EventRegistrationTokenTable<T> where T : class
     {
         public EventRegistrationTokenTable() { }
+        [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]
         public T InvocationList { get { throw null; } set { } }
         public System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken AddEventHandler(T handler) { throw null; }
         public static System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable<T> GetOrCreateEventRegistrationTokenTable(ref System.Runtime.InteropServices.WindowsRuntime.EventRegistrationTokenTable<T> refEventTable) { throw null; }
         public void RemoveEventHandler(System.Runtime.InteropServices.WindowsRuntime.EventRegistrationToken token) { }
         public void RemoveEventHandler(T handler) { }
     }
```

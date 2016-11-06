```diff
---D:\ProjectK\binaries\x86chk\Contracts\System.Diagnostics.Process\4.0.0.0\System.Diagnostics.Process.dll
+++D:\ProjectK\binaries\x86chk\Contracts\System.Diagnostics.Process\4.1.0.0\System.Diagnostics.Process.dll
 namespace System.Diagnostics {
  public class Process : IDisposable {
+   public static Process Start(string fileName, string userName, string password, string domain);
    ^ Immo Landwerth: Remove
+   public static Process Start(string fileName, string arguments, string userName, string password, string domain);
    ^ Immo Landwerth: Remove
  }
  public sealed class ProcessStartInfo {
+   public string Domain { get; set; }
+   public bool LoadUserProfile { get; set; }
+   public string PlainTextPassword { get; set; }
    ^ Immo Landwerth: For desktop, we should hide this property from designers using DesignerSerializationVisibilityAttribute.
    ^ Immo Landwerth: PasswordInClearText
+   public string UserName { get; set; }
  }
 }
```
# API Review 07/14/2022

## add a setter to TransactionManager.DefaultTimeout and MaxTimeout

**Approved** | [#runtime/59282](https://github.com/dotnet/runtime/issues/59282#issuecomment-1184714971) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=0h0m0s)

* We would prefer an option without a static setter, like using AppContext to allow the setting to be set (and loaded into a `Lazy<T>` so it only gets read once)
* As the next fallback, it might be helpful to have the setter only allow set-once (throw on a second call to set)

```diff
namespace System.Transactions
{
    public static class TransactionManager
    {
        public static TimeSpan DefaultTimeout 
        {
            get; 
+           set;
         } 
        public static TimeSpan MaximumTimeout 
        {
            get; 
+           set;
         } 
    }
}
```
## Improve RateLimiter metrics

**Approved** | [#runtime/71804](https://github.com/dotnet/runtime/issues/71804#issuecomment-1184731995) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=0h19m51s)

Looks good as proposed. (Changed resourceID to resource to match an in-flight rename request)

```diff
namespace System.Threading.RateLimiting;

public abstract class RateLimiter : IAsyncDisposable, IDisposable
{
-    public abstract int GetAvailablePermits();
+    public abstract RateLimiterStatistics? GetStatistics();
}

public abstract class PartitionedRateLimiter<TResource> : IAsyncDisposable, IDisposable
{
-    public abstract int GetAvailablePermits(TResource resourceID);
+    public abstract RateLimiterStatistics? GetStatistics(TResource resource);
}

+ public class RateLimiterStatistics
+ {
+    public RateLimiterStatistics();

+    public long CurrentAvailablePermits { get; init; }

+    public long CurrentQueuedCount { get; init; }

+    public long TotalFailedLeases { get; init; }

+    public long TotalSuccessfulLeases { get; init; }
+ }
```
## Rename resourceID in PartitionedRateLimiter

**Approved** | [#runtime/72121](https://github.com/dotnet/runtime/issues/72121#issuecomment-1184732824) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=0h39m33s)

Looks good as proposed

```diff
namespace System.Threading.RateLimiting;

public abstract class PartitionedRateLimiter<TResource> : IAsyncDisposable, IDisposable
{
-    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(TResource resourceID, int permitCount, CancellationToken cancellationToken);
+    protected abstract ValueTask<RateLimitLease> WaitAndAcquireAsyncCore(TResource resource, int permitCount, CancellationToken cancellationToken);
-    public ValueTask<RateLimitLease> WaitAndAcquireAsync(TResource resourceID, int permitCount = 1, CancellationToken cancellationToken = default);
+    public ValueTask<RateLimitLease> WaitAndAcquireAsync(TResource resource, int permitCount = 1, CancellationToken cancellationToken = default);
-    protected abstract RateLimitLease AcquireCore(TResource resourceID, int permitCount);
+    protected abstract RateLimitLease AcquireCore(TResource resource, int permitCount);
-    public RateLimitLease Acquire(TResource resourceID, int permitCount = 1);
+    public RateLimitLease Acquire(TResource resource, int permitCount = 1);
-    public abstract int GetAvailablePermits(TResource resourceID);
+    public abstract int GetAvailablePermits(TResource resource);
}
```
## Introduce Commands and DataContext to WinForms to adapt modern Binding scenarios

**NeedsWork** | [#winforms/4895](https://github.com/dotnet/winforms/issues/4895#issuecomment-1184777610) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=0h40m42s)

* The ICommandProvider interface is rather large, which gives off a feeling that it will end up growing over the next couple of releases.  Instead of a large interface the behavior should be pulled off onto a different type.
  * One possible hand-waved example here is included (changed `ICommandProvider` to `CommandControl : Control` and changed the base class of `ButtonBase`)
    * Most of the Raise* members can probably easily be eliminated and just be the On* version.
  * The alternative design (ICommandBindingTarget + class CommandProvider) may also be fruitful
* We also discussed the DataContext(Changed) aspect and it looks reasonable

```C#
namespace System.Windows.Forms
{
    public class CommandControl : Control
    {
        event EventHandler? CommandChanged;
        event EventHandler? CommandCanExecuteChanged;

        /// <summary>
        /// Gets or sets the Command to invoke, when the implementing 
        /// Component or Control is triggered by the user.
        /// </summary>
        System.Windows.Input.ICommand? Command { get; set; }

        /// <summary>
        /// Gets or sets the CommandParameter for the Component or Control.
        /// </summary>
        object? CommandParameter { get; set; }

        /// <summary>
        /// Gets or sets a value indicating whether the control 
        /// can respond to user interaction.
        /// </summary>
        bool Enabled { get; set; }

        // Needed to restore the previous Enabled status
        // when a new Command gets assigned.
        protected bool? PreviousEnabledStatus { get; set; }

        // Needed to call OnCommandChanged to meet
        // WinForms property implementation conventions for Data Binding.
        protected void RaiseCommandChanged(EventArgs e);

        // Needed to call OnCommandCanExecuteChanged to meet
        // WinForms property implementation conventions for Data Binding.
        protected void RaiseCommandCanExecuteChanged(object sender, EventArgs e);

        // Needed to call OnCommandExecuting to meet
        // WinForms property implementation conventions for Data Binding.
        protected void RaiseCommandExecuting(CancelEventArgs e);

        /// <summary>
        /// Handles the assignment of a new Command to the Command property.
        /// </summary>
        /// <param name="commandComponent">Instance of the component which 
        /// provides the Command property.</param>
        /// <param name="newCommand">New Command value to be assign.</param>
        /// <param name="commandBackingField">The backing field of the component 
        /// which holds the command for the property.</param>
        protected static void CommandSetter(
            ICommandProvider commandComponent,
            ICommand newCommand,
            ref ICommand commandBackingField)
        { }

        /// <summary>
        /// Handels the invoke of the command, e.g. called by a button's click event.
        /// </summary>
        /// <param name="commandComponent"></param>
        protected static void RequestCommandExecute(ICommandProvider commandComponent) { }
    }
}
```

```diff
namespace System.Windows.Forms
{
-    public abstract partial class ButtonBase : Control
+    public abstract partial class ButtonBase : CommandControl
}
```
## Add X509SubjectAlternativeName as a rich type

**Approved** | [#runtime/22699](https://github.com/dotnet/runtime/issues/22699#issuecomment-1184787969) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=1h32m31s)

Looks good as proposed

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public partial class X509Certificate2
    {
        // Performs RFC 6125 matching of a hostname against a certificate and the
        // Subject Alternative Names.
        //
        // Uses IPAddress.TryParse to determine DNS match vs IP match
        public bool MatchesHostname(string hostname) { throw null; }
        public bool MatchesHostname(string hostname, bool allowWildcards = true, bool allowCommonName = true) { throw null; }
    }

    public sealed class X509SubjectAlternativeNameExtension : X509Extension
    {
        public X509SubjectAlternativeNameExtension() { }
        public X509SubjectAlternativeNameExtension(byte[] rawData, bool critical = false) { }
        public X509SubjectAlternativeNameExtension(ReadOnlySpan<byte> rawData, bool critical = false) { }

        public IEnumerable<string> EnumerateDnsNames() { throw null; }
        public IEnumerable<IPAddress> EnumerateIPAddresses() { throw null; }
    }
}
```
## Make it possible to access specific elements of an X500DistinguishedName

**Approved** | [#runtime/33914](https://github.com/dotnet/runtime/issues/33914#issuecomment-1184800563) | [Video](https://www.youtube.com/watch?v=PJh2esB0Efw&t=1h45m16s)

* Changed "Value" to "Element"
* Made SingleElementType a method instead of a null-returning property

```C#
namespace System.Security.Cryptography.X509Certificates
{
    public partial class X500DistinguishedName
    {
        // Reversed defaults to `true` to match the display order from the Name property.
        // It is the reverse of their encoded order, and named to match
        // X500DistinguishedNameFlags.Reversed
        IEnumerable<X500RelativeDistinguishedName> EnumerateRelativeDistinguishedNames(bool reversed = true);
    }

    public sealed class X500RelativeDistinguishedName
    {
        internal X500RelativeDistinguishedName() { }
        
        // If all else fails, send this to AsnReader.
        public ReadOnlyMemory<byte> RawData { get; }
        
        // true is rare, and means all else has failed.
        public bool HasMultipleElements { get; }

        // throws if HasMultipleValues is true
        public Oid GetSingleElementType();

        // throws if HasMultipleValues is true, returns null if string is
        // not an appropriate type per the payload.
        public string? GetSingleElementValue() { throw null; }
    }
}
```

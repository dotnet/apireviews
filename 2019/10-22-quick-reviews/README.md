# Quick Reviews 10/22/2019

## Attribute for minimal runtime impact during an unmanaged call

**Approved** | [#corefx/40740](https://github.com/dotnet/corefx/issues/40740#issuecomment-545076797) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=0h0m0s)

* Would probably make sense on `DllImportAttribute` but this attribute is specially encoded in metadata, so we can't easily do that
* We believe this can't be internal and library authors will likely want this
* This concept should work function pointers, however, it's unclear how this can be expressed. There is currently no way to express this.
* The more generic API names (`TrivialUnmanagedMethodAttribute`, `LimitedUnmanagedMethodAttribute`) seem problematic because we're probably not likely to turn more runtime optimizations on when a method is attributed as *trivial*, which makes specific naming like this more useful.
* We should probably make this attribute work for `UnmanagedFunctionPointerAttribute` which names we should include `Delegate` in the attribute targets.
	- @AaronRobinsonMSFT please confirm whether this feature will work for the existing umanaged function pointer delegates.
* How would this feature work with function pointers?
	- @333fred @jaredpar what are you thoughts on this?
## Provide an `Unsafe.SkipInit` method to allow bypassing definite assignment rules.

**Approved** | [#corefx/38585](https://github.com/dotnet/corefx/issues/38585#issuecomment-545079783) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=0h34m14s)

Looks good as proposed, ~~but we should constrain the `T` to be unamanged.~~

After discussion with @jkotas we concluded that we don't want *any* constraint. The JIT will do the right thing. Not even the `struct` constraint should be there.

```C#
public static partial class Unsafe
{
    public static void SkipInit<T>(out T value);
}
```
## Make Type implement IEquatable<Type>

**Needs Work** | [#corefx/41784](https://github.com/dotnet/corefx/issues/41784#issuecomment-545080947) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=0h41m27s)

What about other types in the `MemberInfo` hierarchy?

@steveharter what are you thoughts?
## Mark Assembly.CodeBase as obsolete

**Approved** | [#corefx/41695](https://github.com/dotnet/corefx/issues/41695#issuecomment-545084027) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=0h44m18s)

Looks good. We should also mark it as `[EditorBrowsable(Never)]`.

```C#
namespace System.Reflection
{
    public partial class Assembly
    {
        [Obsolete("Use Location instead.")]
        public virtual string CodeBase { get; }
    }
}
```
## Add HttpContent.SerializeToStreamAsync overload with cancellation

**Approved** | [#corefx/9071](https://github.com/dotnet/corefx/issues/9071#issuecomment-545093260) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=0h52m7s)

~~We should consider making the existing one non-abstract so that new types inheriting it don't have to overide the *bad* one. We obviously can't call each other because if a person didn't override either method, we shouldn't crash with a stack overflow.~~

**Edit** We changed our mind. We'll leave it abstract and implementers should override both. We should think about a *soft abstract* attribute where an analyzer could warn people if they inherited but didn't override.

```C#
partial class HttpContent
{
    protected virtual Task SerializeToStreamAsync(Stream stream, TransportContext context, CancellationToken token);
    protected abstract Task SerializeToStreamAsync(Stream stream, TransportContext context);
}
```
## SmtpClient.SendMailAsync overload with cancellation

**Approved** | [#corefx/24475](https://github.com/dotnet/corefx/issues/24475#issuecomment-545097103) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=1h15m2s)

Looks good as proposed.

```C#
namespace System.Net.Mail
{
    public partial class SmtpClient
    {
        public Task SendMailAsync(MailMessage message, CancellationToken cancellationToken);
        public Task SendMailAsync(string from, string recipients, string subject, string body, CancellationToken cancellationToken);
    }
}
```
## TryParse for email addresses 

**Needs Work** | [#corefx/25295](https://github.com/dotnet/corefx/issues/25295#issuecomment-545099733) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=1h25m16s)

* If we were to expose the `TryParse` ones, we should also define corresponding static `Parse` methods.
* The bigger question is whether we'd recommend `MailAddress` at all. The [data annotations implementation for `IsValid`](https://source.dot.net/#System.ComponentModel.Annotations/System/ComponentModel/DataAnnotations/EmailAddressAttribute.cs,1dcffef3eace5290,references) is much simpler, probably because email addresses are complex and in practice most validators reject rare, but valid email addreses.
## Provide a zero-cost mechanism for going between System.Numerics.Vector and System.Runtime.Intrinsics.Vector types

**Needs Work** | [#corefx/36379](https://github.com/dotnet/corefx/issues/36379#issuecomment-545102767) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=1h31m59s)

Can we move `Vector2`, `Vector3`, `Vector4` to corlib? It seems weird to add one more dumping ground for extension methods. Another benefit is that we'd end up with all intrinsically understood types to be corlib then.
## Add new overloads with ReadOnlySpan to System.Net.NetworkInformation.PhysicalAddress 

**Approved** | [#corefx/29780](https://github.com/dotnet/corefx/issues/29780#issuecomment-545105286) | [Video](https://www.youtube.com/watch?v=VQuFvX2k-HA&t=1h39m11s)

We should also have `string`-based `TryParse` method. We should also name the `out` parameter `value`. Other than that, looks good as proposed.

```C#
namespace System.Net.NetworkInformation
{
    public class PhysicalAddress
    {
        public static bool TryParse(string address, out PhysicalAddress value);
        public static bool TryParse(ReadOnlySpan<char> address, out PhysicalAddress value);
        public static PhysicalAddress Parse(ReadOnlySpan<char> address);
        // Existing
        // public static PhysicalAddress Parse(string address);
    }
}
```

# Quick Reviews 04/29/2021

## Low level API support for Objective-C scenarios

**Approved** | [#runtime/44659](https://github.com/dotnet/runtime/issues/44659#issuecomment-829495419) | [Video](https://www.youtube.com/watch?v=ctxGYi6HDxw&t=0h0m0s)

* Is the ObjectiveC namespace justified? It may end up with a low number of types.
  * We decided yes, for symmetry with the others.
* Bridge.CreateReferenceTrackingHandle should out Span<IntPtr> instead of just IntPtr so it can communicate that you get two pointers worth of memory (and avoids the famous last words of "it'll always be two").
* Rename Bridge.CreateReferenceTrackingHandle@scratchMemory to taggedMemory
* Remove the UnhandledExceptionPropagation event and make it be only one delegate, bound in the Initialize call
* TrackedNativeReferencesAttribute should be sealed, Inherited=true, AllowMultiple=false
* Is "Bridge" too general name even for an out of the way namespace?
  * We suggested ObjectiveCMarshal
* We renamed TrackedNativeReferenceAttribute to ObjectiveCTrackedTypeAttribute
* We renamed SetMessageSendPendingExceptionForThread to SetMessageSendPendingException (dropped "ForThread")

```C#
namespace System.Runtime.InteropServices.ObjectiveC
{
    /// <summary>
    /// Attribute used to indicate a class is tracked from the native environment.
    /// </summary>
    [AttributeUsage(AttributeTargets.Class, Inherited=true, AllowMultiple=false)]
    public sealed class ObjectiveCTrackedTypeAttribute : Attribute
    {
        /// <summary>
        /// Instantiate a <see cref="ObjectiveCTrackedTypeAttribute"/> instance.
        /// </summary>
        public ObjectiveCTrackedTypeAttribute() { }
    }

    /// <summary>
    /// API to enable an Objective-C bridge.
    /// </summary>
    public static class ObjectiveCMarshal
    {
        /// <summary>
        /// Initialize reference tracking for the Objective-C bridge API.
        /// </summary>
        /// <param name="beginEndCallback">Called when tracking begins and ends.</param>
        /// <param name="isReferencedCallback">Called to determine if a managed object instance is referenced elsewhere, and must not be collected by the GC.</param>
        /// <param name="trackedObjectEnteredFinalization">Called when a tracked object enters the finalization queue.</param>
        /// <param name="unhandledExceptionPropagationHandler">Handler for the propagation of unhandled Exceptions across a managed -> native boundary (that is, Reverse P/Invoke).</param>
        /// <exception cref="InvalidOperationException">Thrown if this API has already been called.</exception>
        /// <remarks>
        /// All callbacks must be written in native code since they will be called by the GC and
        /// managed code is not able to run at that time.
        ///
        /// The <paramref name="beginEndCallback"/> will be called when reference tracking begins and ends.
        /// The associated begin/end pair will never be nested.
        ///
        /// The <paramref name="isReferencedCallback"/> should return 0 for not reference or 1 for
        /// referenced. Any other value has undefined behavior.
        /// </remarks>
        public static unsafe void InitializeReferenceTracking(
            delegate* unmanaged<void> beginEndCallback,
            delegate* unmanaged<IntPtr, int> isReferencedCallback,
            delegate* unmanaged<IntPtr, void> trackedObjectEnteredFinalization,
            UnhandledExceptionPropagationHandler unhandledExceptionPropagationHandler);

        /// <summary>
        /// Request native reference tracking for the supplied object.
        /// </summary>
        /// <param name="obj">The object to track.</param>
        /// <param name="scratchMemory">A pointer to scratch memory.</param>
        /// <returns>Reference tracking GC handle.</returns>
        /// <exception cref="InvalidOperationException">Thrown if the Bridge API has not been initialized.</exception>
        /// <remarks>
        /// Reference tracking in the <see cref="Bridge"/> must be initialized prior to calling
        /// this function.
        ///
        /// The <paramref name="obj"/> must have a type in its hierarchy marked with
        /// <see cref="TrackedNativeReferenceAttribute"/>.
        ///
        /// The "Is Referenced" callback passed to InitializeReferenceTracking
        /// will be passed the <paramref name="scratchMemory"/> returned from this function.
        /// The memory it points at is 2 pointer's worth (for example, 16 bytes on a 64-bit platform) and
        /// will be zeroed out and available until <paramref name="obj"/> is collected by the GC.
        /// The memory pointed to by <paramref name="scratchMemory"/> can be used for any purpose by the
        /// caller of this function and usable during the "Is Referenced" callback.
        ///
        /// Calling this function multiple times with the same <paramref name="obj"/> will
        /// return a new handle each time but the same scratch memory will be returned. The
        /// scratch memory is only guaranteed to be zero initialized on the first call.
        ///
        /// The caller is responsible for freeing the returned <see cref="GCHandle"/>.
        /// </remarks>
        public static GCHandle CreateReferenceTrackingHandle(
            object obj,
            out Span<IntPtr> taggedMemory);

        /// <summary>
        /// Objective-C msgSend function override options.
        /// </summary>
        /// <see href="https://developer.apple.com/documentation/objectivec/1456712-objc_msgsend"/>
        public enum MsgSendFunction
        {
            ObjCMsgSend,
            ObjCMsgSendFpret,
            ObjCMsgSendStret,
            ObjCMsgSendSuper,
            ObjCMsgSendSuperStret,
        }

        /// <summary>
        /// Set function pointer override for an Objective-C runtime message passing export.
        /// </summary>
        /// <param name="msgSendFunction">The export to override.</param>
        /// <param name="func">The function override.</param>
        /// <exception cref="InvalidOperationException">Thrown if the msgSend function has already been overridden.</exception>
        /// <remarks>
        /// Providing an override can enable support for Objective-C
        /// exception propagation and variadic argument support.
        /// </remarks>
        public static void SetMessageSendCallback(MsgSendFunction msgSendFunction, IntPtr func);

        /// <summary>
        /// Sets a pending exception for this thread to be thrown
        /// the next time the runtime is entered from an overridden
        /// msgSend P/Invoke.
        /// </summary>
        /// <param name="exception">The exception.</param>
        /// <remarks>
        /// If <c>null</c> is supplied any pending exception is discarded.
        /// </remarks>
        public static void SetMessageSendPendingException(Exception? exception);

        /// <summary>
        /// Handler for unhandled Exceptions crossing the managed -> native boundary (that is, Reverse P/Invoke).
        /// </summary>
        /// <param name="exception">Unhandled exception.</param>
        /// <param name="lastMethod">Last managed method.</param>
        /// <param name="context">Context provided to the returned function pointer.</param>
        /// <returns>Exception propagation callback.</returns>
        /// <remarks>
        /// If the handler is able to propagate the managed Exception properly to the native environment an
        /// unmanaged callback can be returned, otherwise <c>null</c>. The RuntimeMethodHandle is to the
        /// last managed method that was executed prior to leaving the runtime. Along with the returned callback
        /// the handler can return a context that will be passed to the callback during dispatch.
        ///
        /// The returned handler will be passed the context when called and is the responsibility of the callback
        /// to managed. The handler must not return and is expected to propagate the exception into the native
        /// environment or fail fast.
        /// </remarks>
        public unsafe delegate delegate* unmanaged<IntPtr, void> UnhandledExceptionPropagationHandler(
            Exception exception,
            RuntimeMethodHandle lastMethod,
            out IntPtr context);
    }
}
```
## Proposed API for symbolic links

**Approved** | [#runtime/24271](https://github.com/dotnet/runtime/issues/24271#issuecomment-829537170) | [Video](https://www.youtube.com/watch?v=ctxGYi6HDxw&t=1h31m38s)

* File.CreateSymbolicLink should return FileSystemInfo instead of FileInfo, to avoid the type problem when the target was a directory
* Same for GetSymbolicLinkTarget
* The FileSystemInfo.CreateAsSymbolicLink seems problematic, since it could cause the existing instance to change whether it "believes" it's a file or a directory, depending on the target.
  * What does new FileSystemInfo(@"C:\Windows") do?  What happens when use members on it later?
  * Ultimately we ended up not needing to know, but it did leave us in a confused state in the middle.
* In review we were concerned with how this might affect existing behaviors of things like DirectoryInfo.EnumerateDirectories (does that currently examine if symbolic links are directories?) and since we weren't sure what the answers were we had trouble continuing.
* The File and Directory GetSymbolicLinkTarget want the returnFinalTarget boolean, for symmetry.
* We defaulted all the returnFinalTarget values to false
* @GrabYourPitchforks  has lots of opinions on documentation for when returnFinalTarget is true.

```C#
namespace System.IO
{
    public abstract partial class FileSystemInfo
    {
        public void CreateAsSymbolicLink(string pathToTarget);
        public FileSystemInfo? GetSymbolicLinkTarget(bool returnFinalTarget = false); 
    }
    public static class File
    {
        public static FileSystemInfo CreateSymbolicLink(string path, string pathToTarget);
        public static FileSystemInfo? GetSymbolicLinkTarget(string linkPath, bool returnFinalTarget = false);
    }
    public static class Directory
    {
        public static FileSystemInfo CreateSymbolicLink(string path, string pathToTarget);
        public static FileSystemInfo? GetSymbolicLinkTarget(string linkPath, bool returnFinalTarget = false);
    }
}
```

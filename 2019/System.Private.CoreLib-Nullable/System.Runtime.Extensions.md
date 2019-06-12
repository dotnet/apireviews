# System.Runtime.Extensions

```diff
---D:\Temp\nullable\before\System.Runtime.Extensions.dll
+++D:\Temp\nullable\after\System.Runtime.Extensions.dll
 namespace System {
  public sealed class AppDomain : MarshalByRefObject {
    public string? BaseDirectory { get; }
    public static AppDomain CurrentDomain { get; }
    public string? DynamicDirectory { get; }
    public string FriendlyName { get; }
    public int Id { get; }
    public bool IsFullyTrusted { get; }
    public bool IsHomogenous { get; }
    public static bool MonitoringIsEnabled { get; set; }
    public long MonitoringSurvivedMemorySize { get; }
    public static long MonitoringSurvivedProcessMemorySize { get; }
    public long MonitoringTotalAllocatedMemorySize { get; }
    public TimeSpan MonitoringTotalProcessorTime { get; }
    public PermissionSet PermissionSet { get; }
    public string? RelativeSearchPath { get; }
    public AppDomainSetup SetupInformation { get; }
    public bool ShadowCopyFiles { get; }
    public event AssemblyLoadEventHandler AssemblyLoad;
    public event ResolveEventHandler AssemblyResolve;
    public event EventHandler DomainUnload;
    public event EventHandler<FirstChanceExceptionEventArgs> FirstChanceException;
    public event EventHandler ProcessExit;
    public event ResolveEventHandler ReflectionOnlyAssemblyResolve;
    public event ResolveEventHandler ResourceResolve;
    public event ResolveEventHandler TypeResolve;
    public event UnhandledExceptionEventHandler UnhandledException;
    public void AppendPrivatePath(string? path);
    public string ApplyPolicy(string assemblyName);
    public void ClearPrivatePath();
    public void ClearShadowCopyPath();
    public static AppDomain CreateDomain(string friendlyName);
    public ObjectHandle? CreateInstance(string assemblyName, string typeName);
    public ObjectHandle? CreateInstance(string assemblyName, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
    public ObjectHandle? CreateInstance(string assemblyName, string typeName, object?[]? activationAttributes);
    public object? CreateInstanceAndUnwrap(string assemblyName, string typeName);
    public object? CreateInstanceAndUnwrap(string assemblyName, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
    public object? CreateInstanceAndUnwrap(string assemblyName, string typeName, object?[]? activationAttributes);
    public ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName);
    public ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
    public ObjectHandle? CreateInstanceFrom(string assemblyFile, string typeName, object?[]? activationAttributes);
    public object? CreateInstanceFromAndUnwrap(string assemblyFile, string typeName);
    public object? CreateInstanceFromAndUnwrap(string assemblyFile, string typeName, bool ignoreCase, BindingFlags bindingAttr, Binder? binder, object?[]? args, CultureInfo? culture, object?[]? activationAttributes);
    public object? CreateInstanceFromAndUnwrap(string assemblyFile, string typeName, object?[]? activationAttributes);
    public int ExecuteAssembly(string assemblyFile);
    public int ExecuteAssembly(string assemblyFile, string?[]? args);
    public int ExecuteAssembly(string assemblyFile, string?[]? args, byte[]? hashValue, AssemblyHashAlgorithm hashAlgorithm);
    public int ExecuteAssemblyByName(AssemblyName assemblyName, params string?[]? args);
    public int ExecuteAssemblyByName(string assemblyName);
    public int ExecuteAssemblyByName(string assemblyName, params string?[]? args);
    public Assembly[] GetAssemblies();
    public static int GetCurrentThreadId();
    public object? GetData(string name);
    public bool? IsCompatibilitySwitchSet(string value);
    public bool IsDefaultAppDomain();
    public bool IsFinalizingForUnload();
    public Assembly Load(byte[] rawAssembly);
    public Assembly Load(byte[] rawAssembly, byte[]? rawSymbolStore);
    public Assembly Load(AssemblyName assemblyRef);
    public Assembly Load(string assemblyString);
    public Assembly[] ReflectionOnlyGetAssemblies();
    public void SetCachePath(string? path);
    public void SetData(string name, object? data);
    public void SetDynamicBase(string? path);
    public void SetPrincipalPolicy(PrincipalPolicy policy);
    public void SetShadowCopyFiles();
    public void SetShadowCopyPath(string? path);
    public void SetThreadPrincipal(IPrincipal principal);
    public override string ToString();
    public static void Unload(AppDomain domain);
  }
  public sealed class AppDomainSetup {
    public string? ApplicationBase { get; }
    public string? TargetFrameworkName { get; }
  }
  public class AppDomainUnloadedException : SystemException {
    public AppDomainUnloadedException();
    protected AppDomainUnloadedException(SerializationInfo info, StreamingContext context);
    public AppDomainUnloadedException(string? message);
    public AppDomainUnloadedException(string? message, Exception? innerException);
  }
  public sealed class ApplicationId {
    public ApplicationId(byte[] publicKeyToken, string name, Version version, string? processorArchitecture, string? culture);
    public string? Culture { get; }
    public string Name { get; }
    public string? ProcessorArchitecture { get; }
    public byte[] PublicKeyToken { get; }
    public Version Version { get; }
    public ApplicationId Copy();
    public override bool Equals(object? o);
    public override int GetHashCode();
    public override string ToString();
  }
  public class AssemblyLoadEventArgs : EventArgs {
    public AssemblyLoadEventArgs(Assembly loadedAssembly);
    public Assembly LoadedAssembly { get; }
  }
  public delegate void AssemblyLoadEventHandler(object? sender, AssemblyLoadEventArgs args);
  public enum Base64FormattingOptions {
    InsertLineBreaks = 1,
    None = 0,
  }
  public static class BitConverter {
    public static readonly bool IsLittleEndian;
    public static long DoubleToInt64Bits(double value);
    public static byte[] GetBytes(bool value);
    public static byte[] GetBytes(char value);
    public static byte[] GetBytes(double value);
    public static byte[] GetBytes(short value);
    public static byte[] GetBytes(int value);
    public static byte[] GetBytes(long value);
    public static byte[] GetBytes(float value);
    public static byte[] GetBytes(ushort value);
    public static byte[] GetBytes(uint value);
    public static byte[] GetBytes(ulong value);
    public static float Int32BitsToSingle(int value);
    public static double Int64BitsToDouble(long value);
    public static int SingleToInt32Bits(float value);
    public static bool ToBoolean(byte[] value, int startIndex);
    public static bool ToBoolean(ReadOnlySpan<byte> value);
    public static char ToChar(byte[] value, int startIndex);
    public static char ToChar(ReadOnlySpan<byte> value);
    public static double ToDouble(byte[] value, int startIndex);
    public static double ToDouble(ReadOnlySpan<byte> value);
    public static short ToInt16(byte[] value, int startIndex);
    public static short ToInt16(ReadOnlySpan<byte> value);
    public static int ToInt32(byte[] value, int startIndex);
    public static int ToInt32(ReadOnlySpan<byte> value);
    public static long ToInt64(byte[] value, int startIndex);
    public static long ToInt64(ReadOnlySpan<byte> value);
    public static float ToSingle(byte[] value, int startIndex);
    public static float ToSingle(ReadOnlySpan<byte> value);
    public static string ToString(byte[] value);
    public static string ToString(byte[] value, int startIndex);
    public static string ToString(byte[] value, int startIndex, int length);
    public static ushort ToUInt16(byte[] value, int startIndex);
    public static ushort ToUInt16(ReadOnlySpan<byte> value);
    public static uint ToUInt32(byte[] value, int startIndex);
    public static uint ToUInt32(ReadOnlySpan<byte> value);
    public static ulong ToUInt64(byte[] value, int startIndex);
    public static ulong ToUInt64(ReadOnlySpan<byte> value);
    public static bool TryWriteBytes(Span<byte> destination, bool value);
    public static bool TryWriteBytes(Span<byte> destination, char value);
    public static bool TryWriteBytes(Span<byte> destination, double value);
    public static bool TryWriteBytes(Span<byte> destination, short value);
    public static bool TryWriteBytes(Span<byte> destination, int value);
    public static bool TryWriteBytes(Span<byte> destination, long value);
    public static bool TryWriteBytes(Span<byte> destination, float value);
    public static bool TryWriteBytes(Span<byte> destination, ushort value);
    public static bool TryWriteBytes(Span<byte> destination, uint value);
    public static bool TryWriteBytes(Span<byte> destination, ulong value);
  }
  public class CannotUnloadAppDomainException : SystemException {
    public CannotUnloadAppDomainException();
    protected CannotUnloadAppDomainException(SerializationInfo info, StreamingContext context);
    public CannotUnloadAppDomainException(string? message);
    public CannotUnloadAppDomainException(string? message, Exception? innerException);
  }
  public abstract class ContextBoundObject : MarshalByRefObject {
    protected ContextBoundObject();
  }
  public class ContextMarshalException : SystemException {
    public ContextMarshalException();
    protected ContextMarshalException(SerializationInfo info, StreamingContext context);
    public ContextMarshalException(string? message);
    public ContextMarshalException(string? message, Exception? inner);
  }
  public class ContextStaticAttribute : Attribute {
    public ContextStaticAttribute();
  }
  public static class Convert {
    public static readonly object DBNull;
    public static object? ChangeType(object? value, Type conversionType);
    public static object? ChangeType(object? value, Type conversionType, IFormatProvider? provider);
    public static object? ChangeType(object? value, TypeCode typeCode);
    public static object? ChangeType(object? value, TypeCode typeCode, IFormatProvider? provider);
    public static byte[] FromBase64CharArray(char[] inArray, int offset, int length);
    public static byte[] FromBase64String(string s);
    public static TypeCode GetTypeCode(object? value);
    public static bool IsDBNull(object? value);
    public static int ToBase64CharArray(byte[] inArray, int offsetIn, int length, char[] outArray, int offsetOut);
    public static int ToBase64CharArray(byte[] inArray, int offsetIn, int length, char[] outArray, int offsetOut, Base64FormattingOptions options);
    public static string ToBase64String(byte[] inArray);
    public static string ToBase64String(byte[] inArray, Base64FormattingOptions options);
    public static string ToBase64String(byte[] inArray, int offset, int length);
    public static string ToBase64String(byte[] inArray, int offset, int length, Base64FormattingOptions options);
    public static string ToBase64String(ReadOnlySpan<byte> bytes, Base64FormattingOptions options = Base64FormattingOptions.None);
    public static bool ToBoolean(bool value);
    public static bool ToBoolean(byte value);
    public static bool ToBoolean(char value);
    public static bool ToBoolean(DateTime value);
    public static bool ToBoolean(decimal value);
    public static bool ToBoolean(double value);
    public static bool ToBoolean(short value);
    public static bool ToBoolean(int value);
    public static bool ToBoolean(long value);
    public static bool ToBoolean(object? value);
    public static bool ToBoolean(object? value, IFormatProvider? provider);
    public static bool ToBoolean(sbyte value);
    public static bool ToBoolean(float value);
    public static bool ToBoolean(string? value);
    public static bool ToBoolean(string? value, IFormatProvider? provider);
    public static bool ToBoolean(ushort value);
    public static bool ToBoolean(uint value);
    public static bool ToBoolean(ulong value);
    public static byte ToByte(bool value);
    public static byte ToByte(byte value);
    public static byte ToByte(char value);
    public static byte ToByte(DateTime value);
    public static byte ToByte(decimal value);
    public static byte ToByte(double value);
    public static byte ToByte(short value);
    public static byte ToByte(int value);
    public static byte ToByte(long value);
    public static byte ToByte(object? value);
    public static byte ToByte(object? value, IFormatProvider? provider);
    public static byte ToByte(sbyte value);
    public static byte ToByte(float value);
    public static byte ToByte(string? value);
    public static byte ToByte(string? value, IFormatProvider? provider);
    public static byte ToByte(string? value, int fromBase);
    public static byte ToByte(ushort value);
    public static byte ToByte(uint value);
    public static byte ToByte(ulong value);
    public static char ToChar(bool value);
    public static char ToChar(byte value);
    public static char ToChar(char value);
    public static char ToChar(DateTime value);
    public static char ToChar(decimal value);
    public static char ToChar(double value);
    public static char ToChar(short value);
    public static char ToChar(int value);
    public static char ToChar(long value);
    public static char ToChar(object? value);
    public static char ToChar(object? value, IFormatProvider? provider);
    public static char ToChar(sbyte value);
    public static char ToChar(float value);
    public static char ToChar(string value);
    public static char ToChar(string value, IFormatProvider? provider);
    public static char ToChar(ushort value);
    public static char ToChar(uint value);
    public static char ToChar(ulong value);
    public static DateTime ToDateTime(bool value);
    public static DateTime ToDateTime(byte value);
    public static DateTime ToDateTime(char value);
    public static DateTime ToDateTime(DateTime value);
    public static DateTime ToDateTime(decimal value);
    public static DateTime ToDateTime(double value);
    public static DateTime ToDateTime(short value);
    public static DateTime ToDateTime(int value);
    public static DateTime ToDateTime(long value);
    public static DateTime ToDateTime(object? value);
    public static DateTime ToDateTime(object? value, IFormatProvider? provider);
    public static DateTime ToDateTime(sbyte value);
    public static DateTime ToDateTime(float value);
    public static DateTime ToDateTime(string? value);
    public static DateTime ToDateTime(string? value, IFormatProvider? provider);
    public static DateTime ToDateTime(ushort value);
    public static DateTime ToDateTime(uint value);
    public static DateTime ToDateTime(ulong value);
    public static decimal ToDecimal(bool value);
    public static decimal ToDecimal(byte value);
    public static decimal ToDecimal(char value);
    public static decimal ToDecimal(DateTime value);
    public static decimal ToDecimal(decimal value);
    public static decimal ToDecimal(double value);
    public static decimal ToDecimal(short value);
    public static decimal ToDecimal(int value);
    public static decimal ToDecimal(long value);
    public static decimal ToDecimal(object? value);
    public static decimal ToDecimal(object? value, IFormatProvider? provider);
    public static decimal ToDecimal(sbyte value);
    public static decimal ToDecimal(float value);
    public static decimal ToDecimal(string? value);
    public static decimal ToDecimal(string? value, IFormatProvider? provider);
    public static decimal ToDecimal(ushort value);
    public static decimal ToDecimal(uint value);
    public static decimal ToDecimal(ulong value);
    public static double ToDouble(bool value);
    public static double ToDouble(byte value);
    public static double ToDouble(char value);
    public static double ToDouble(DateTime value);
    public static double ToDouble(decimal value);
    public static double ToDouble(double value);
    public static double ToDouble(short value);
    public static double ToDouble(int value);
    public static double ToDouble(long value);
    public static double ToDouble(object? value);
    public static double ToDouble(object? value, IFormatProvider? provider);
    public static double ToDouble(sbyte value);
    public static double ToDouble(float value);
    public static double ToDouble(string? value);
    public static double ToDouble(string? value, IFormatProvider? provider);
    public static double ToDouble(ushort value);
    public static double ToDouble(uint value);
    public static double ToDouble(ulong value);
    public static short ToInt16(bool value);
    public static short ToInt16(byte value);
    public static short ToInt16(char value);
    public static short ToInt16(DateTime value);
    public static short ToInt16(decimal value);
    public static short ToInt16(double value);
    public static short ToInt16(short value);
    public static short ToInt16(int value);
    public static short ToInt16(long value);
    public static short ToInt16(object? value);
    public static short ToInt16(object? value, IFormatProvider? provider);
    public static short ToInt16(sbyte value);
    public static short ToInt16(float value);
    public static short ToInt16(string? value);
    public static short ToInt16(string? value, IFormatProvider? provider);
    public static short ToInt16(string? value, int fromBase);
    public static short ToInt16(ushort value);
    public static short ToInt16(uint value);
    public static short ToInt16(ulong value);
    public static int ToInt32(bool value);
    public static int ToInt32(byte value);
    public static int ToInt32(char value);
    public static int ToInt32(DateTime value);
    public static int ToInt32(decimal value);
    public static int ToInt32(double value);
    public static int ToInt32(short value);
    public static int ToInt32(int value);
    public static int ToInt32(long value);
    public static int ToInt32(object? value);
    public static int ToInt32(object? value, IFormatProvider? provider);
    public static int ToInt32(sbyte value);
    public static int ToInt32(float value);
    public static int ToInt32(string? value);
    public static int ToInt32(string? value, IFormatProvider? provider);
    public static int ToInt32(string? value, int fromBase);
    public static int ToInt32(ushort value);
    public static int ToInt32(uint value);
    public static int ToInt32(ulong value);
    public static long ToInt64(bool value);
    public static long ToInt64(byte value);
    public static long ToInt64(char value);
    public static long ToInt64(DateTime value);
    public static long ToInt64(decimal value);
    public static long ToInt64(double value);
    public static long ToInt64(short value);
    public static long ToInt64(int value);
    public static long ToInt64(long value);
    public static long ToInt64(object? value);
    public static long ToInt64(object? value, IFormatProvider? provider);
    public static long ToInt64(sbyte value);
    public static long ToInt64(float value);
    public static long ToInt64(string? value);
    public static long ToInt64(string? value, IFormatProvider? provider);
    public static long ToInt64(string? value, int fromBase);
    public static long ToInt64(ushort value);
    public static long ToInt64(uint value);
    public static long ToInt64(ulong value);
    public static sbyte ToSByte(bool value);
    public static sbyte ToSByte(byte value);
    public static sbyte ToSByte(char value);
    public static sbyte ToSByte(DateTime value);
    public static sbyte ToSByte(decimal value);
    public static sbyte ToSByte(double value);
    public static sbyte ToSByte(short value);
    public static sbyte ToSByte(int value);
    public static sbyte ToSByte(long value);
    public static sbyte ToSByte(object? value);
    public static sbyte ToSByte(object? value, IFormatProvider? provider);
    public static sbyte ToSByte(sbyte value);
    public static sbyte ToSByte(float value);
    public static sbyte ToSByte(string? value);
    public static sbyte ToSByte(string value, IFormatProvider? provider);
    public static sbyte ToSByte(string? value, int fromBase);
    public static sbyte ToSByte(ushort value);
    public static sbyte ToSByte(uint value);
    public static sbyte ToSByte(ulong value);
    public static float ToSingle(bool value);
    public static float ToSingle(byte value);
    public static float ToSingle(char value);
    public static float ToSingle(DateTime value);
    public static float ToSingle(decimal value);
    public static float ToSingle(double value);
    public static float ToSingle(short value);
    public static float ToSingle(int value);
    public static float ToSingle(long value);
    public static float ToSingle(object? value);
    public static float ToSingle(object? value, IFormatProvider? provider);
    public static float ToSingle(sbyte value);
    public static float ToSingle(float value);
    public static float ToSingle(string? value);
    public static float ToSingle(string? value, IFormatProvider? provider);
    public static float ToSingle(ushort value);
    public static float ToSingle(uint value);
    public static float ToSingle(ulong value);
    public static string ToString(bool value);
    public static string ToString(bool value, IFormatProvider? provider);
    public static string ToString(byte value);
    public static string ToString(byte value, IFormatProvider? provider);
    public static string ToString(byte value, int toBase);
    public static string ToString(char value);
    public static string ToString(char value, IFormatProvider? provider);
    public static string ToString(DateTime value);
    public static string ToString(DateTime value, IFormatProvider? provider);
    public static string ToString(decimal value);
    public static string ToString(decimal value, IFormatProvider? provider);
    public static string ToString(double value);
    public static string ToString(double value, IFormatProvider? provider);
    public static string ToString(short value);
    public static string ToString(short value, IFormatProvider? provider);
    public static string ToString(short value, int toBase);
    public static string ToString(int value);
    public static string ToString(int value, IFormatProvider? provider);
    public static string ToString(int value, int toBase);
    public static string ToString(long value);
    public static string ToString(long value, IFormatProvider? provider);
    public static string ToString(long value, int toBase);
    public static string? ToString(object? value);
    public static string? ToString(object? value, IFormatProvider? provider);
    public static string ToString(sbyte value);
    public static string ToString(sbyte value, IFormatProvider? provider);
    public static string ToString(float value);
    public static string ToString(float value, IFormatProvider? provider);
    public static string? ToString(string? value);
    public static string? ToString(string? value, IFormatProvider? provider);
    public static string ToString(ushort value);
    public static string ToString(ushort value, IFormatProvider? provider);
    public static string ToString(uint value);
    public static string ToString(uint value, IFormatProvider? provider);
    public static string ToString(ulong value);
    public static string ToString(ulong value, IFormatProvider? provider);
    public static ushort ToUInt16(bool value);
    public static ushort ToUInt16(byte value);
    public static ushort ToUInt16(char value);
    public static ushort ToUInt16(DateTime value);
    public static ushort ToUInt16(decimal value);
    public static ushort ToUInt16(double value);
    public static ushort ToUInt16(short value);
    public static ushort ToUInt16(int value);
    public static ushort ToUInt16(long value);
    public static ushort ToUInt16(object? value);
    public static ushort ToUInt16(object? value, IFormatProvider? provider);
    public static ushort ToUInt16(sbyte value);
    public static ushort ToUInt16(float value);
    public static ushort ToUInt16(string? value);
    public static ushort ToUInt16(string? value, IFormatProvider? provider);
    public static ushort ToUInt16(string? value, int fromBase);
    public static ushort ToUInt16(ushort value);
    public static ushort ToUInt16(uint value);
    public static ushort ToUInt16(ulong value);
    public static uint ToUInt32(bool value);
    public static uint ToUInt32(byte value);
    public static uint ToUInt32(char value);
    public static uint ToUInt32(DateTime value);
    public static uint ToUInt32(decimal value);
    public static uint ToUInt32(double value);
    public static uint ToUInt32(short value);
    public static uint ToUInt32(int value);
    public static uint ToUInt32(long value);
    public static uint ToUInt32(object? value);
    public static uint ToUInt32(object? value, IFormatProvider? provider);
    public static uint ToUInt32(sbyte value);
    public static uint ToUInt32(float value);
    public static uint ToUInt32(string? value);
    public static uint ToUInt32(string? value, IFormatProvider? provider);
    public static uint ToUInt32(string? value, int fromBase);
    public static uint ToUInt32(ushort value);
    public static uint ToUInt32(uint value);
    public static uint ToUInt32(ulong value);
    public static ulong ToUInt64(bool value);
    public static ulong ToUInt64(byte value);
    public static ulong ToUInt64(char value);
    public static ulong ToUInt64(DateTime value);
    public static ulong ToUInt64(decimal value);
    public static ulong ToUInt64(double value);
    public static ulong ToUInt64(short value);
    public static ulong ToUInt64(int value);
    public static ulong ToUInt64(long value);
    public static ulong ToUInt64(object? value);
    public static ulong ToUInt64(object? value, IFormatProvider? provider);
    public static ulong ToUInt64(sbyte value);
    public static ulong ToUInt64(float value);
    public static ulong ToUInt64(string? value);
    public static ulong ToUInt64(string? value, IFormatProvider? provider);
    public static ulong ToUInt64(string? value, int fromBase);
    public static ulong ToUInt64(ushort value);
    public static ulong ToUInt64(uint value);
    public static ulong ToUInt64(ulong value);
    public static bool TryFromBase64Chars(ReadOnlySpan<char> chars, Span<byte> bytes, out int bytesWritten);
    public static bool TryFromBase64String(string s, Span<byte> bytes, out int bytesWritten);
    public static bool TryToBase64Chars(ReadOnlySpan<byte> bytes, Span<char> chars, out int charsWritten, Base64FormattingOptions options = Base64FormattingOptions.None);
  }
  public static class Environment {
    public static string CommandLine { get; }
    public static string CurrentDirectory { get; set; }
    public static int CurrentManagedThreadId { get; }
    public static int ExitCode { get; set; }
    public static bool HasShutdownStarted { get; }
    public static bool Is64BitOperatingSystem { get; }
    public static bool Is64BitProcess { get; }
    public static string MachineName { get; }
    public static string NewLine { get; }
    public static OperatingSystem OSVersion { get; }
    public static int ProcessorCount { get; }
    public static string StackTrace { get; }
    public static string SystemDirectory { get; }
    public static int SystemPageSize { get; }
    public static int TickCount { get; }
+   public static long TickCount64 { get; }
    public static string UserDomainName { get; }
    public static bool UserInteractive { get; }
    public static string UserName { get; }
    public static Version Version { get; }
    public static long WorkingSet { get; }
    public static void Exit(int exitCode);
    public static string ExpandEnvironmentVariables(string name);
    public static void FailFast(string? message);
    public static void FailFast(string? message, Exception? exception);
    public static string[] GetCommandLineArgs();
    public static string? GetEnvironmentVariable(string variable);
    public static string? GetEnvironmentVariable(string variable, EnvironmentVariableTarget target);
    public static IDictionary GetEnvironmentVariables();
    public static IDictionary GetEnvironmentVariables(EnvironmentVariableTarget target);
    public static string GetFolderPath(Environment.SpecialFolder folder);
    public static string GetFolderPath(Environment.SpecialFolder folder, Environment.SpecialFolderOption option);
    public static string[] GetLogicalDrives();
    public static void SetEnvironmentVariable(string variable, string? value);
    public static void SetEnvironmentVariable(string variable, string? value, EnvironmentVariableTarget target);
    public enum SpecialFolder {
      AdminTools = 48,
      ApplicationData = 26,
      CDBurning = 59,
      CommonAdminTools = 47,
      CommonApplicationData = 35,
      CommonDesktopDirectory = 25,
      CommonDocuments = 46,
      CommonMusic = 53,
      CommonOemLinks = 58,
      CommonPictures = 54,
      CommonProgramFiles = 43,
      CommonProgramFilesX86 = 44,
      CommonPrograms = 23,
      CommonStartMenu = 22,
      CommonStartup = 24,
      CommonTemplates = 45,
      CommonVideos = 55,
      Cookies = 33,
      Desktop = 0,
      DesktopDirectory = 16,
      Favorites = 6,
      Fonts = 20,
      History = 34,
      InternetCache = 32,
      LocalApplicationData = 28,
      LocalizedResources = 57,
      MyComputer = 17,
      MyDocuments = 5,
      MyMusic = 13,
      MyPictures = 39,
      MyVideos = 14,
      NetworkShortcuts = 19,
      Personal = 5,
      PrinterShortcuts = 27,
      ProgramFiles = 38,
      ProgramFilesX86 = 42,
      Programs = 2,
      Recent = 8,
      Resources = 56,
      SendTo = 9,
      StartMenu = 11,
      Startup = 7,
      System = 37,
      SystemX86 = 41,
      Templates = 21,
      UserProfile = 40,
      Windows = 36,
    }
    public enum SpecialFolderOption {
      Create = 32768,
      DoNotVerify = 16384,
      None = 0,
    }
  }
  public enum EnvironmentVariableTarget {
    Machine = 2,
    Process = 0,
    User = 1,
  }
  public enum LoaderOptimization {
    DisallowBindings = 4,
    DomainMask = 3,
    MultiDomain = 2,
    MultiDomainHost = 3,
    NotSpecified = 0,
    SingleDomain = 1,
  }
  public sealed class LoaderOptimizationAttribute : Attribute {
    public LoaderOptimizationAttribute(byte value);
    public LoaderOptimizationAttribute(LoaderOptimization value);
    public LoaderOptimization Value { get; }
  }
  public static class Math {
    public const double E = 2.7182818284590451;
    public const double PI = 3.1415926535897931;
    public static decimal Abs(decimal value);
    public static double Abs(double value);
    public static short Abs(short value);
    public static int Abs(int value);
    public static long Abs(long value);
    public static sbyte Abs(sbyte value);
    public static float Abs(float value);
    public static double Acos(double d);
    public static double Acosh(double d);
    public static double Asin(double d);
    public static double Asinh(double d);
    public static double Atan(double d);
    public static double Atan2(double y, double x);
    public static double Atanh(double d);
    public static long BigMul(int a, int b);
    public static double BitDecrement(double x);
    public static double BitIncrement(double x);
    public static double Cbrt(double d);
    public static decimal Ceiling(decimal d);
    public static double Ceiling(double a);
    public static byte Clamp(byte value, byte min, byte max);
    public static decimal Clamp(decimal value, decimal min, decimal max);
    public static double Clamp(double value, double min, double max);
    public static short Clamp(short value, short min, short max);
    public static int Clamp(int value, int min, int max);
    public static long Clamp(long value, long min, long max);
    public static sbyte Clamp(sbyte value, sbyte min, sbyte max);
    public static float Clamp(float value, float min, float max);
    public static ushort Clamp(ushort value, ushort min, ushort max);
    public static uint Clamp(uint value, uint min, uint max);
    public static ulong Clamp(ulong value, ulong min, ulong max);
    public static double CopySign(double x, double y);
    public static double Cos(double d);
    public static double Cosh(double value);
    public static int DivRem(int a, int b, out int result);
    public static long DivRem(long a, long b, out long result);
    public static double Exp(double d);
    public static decimal Floor(decimal d);
    public static double Floor(double d);
    public static double FusedMultiplyAdd(double x, double y, double z);
    public static double IEEERemainder(double x, double y);
    public static int ILogB(double x);
    public static double Log(double d);
    public static double Log(double a, double newBase);
    public static double Log10(double d);
    public static double Log2(double x);
    public static byte Max(byte val1, byte val2);
    public static decimal Max(decimal val1, decimal val2);
    public static double Max(double val1, double val2);
    public static short Max(short val1, short val2);
    public static int Max(int val1, int val2);
    public static long Max(long val1, long val2);
    public static sbyte Max(sbyte val1, sbyte val2);
    public static float Max(float val1, float val2);
    public static ushort Max(ushort val1, ushort val2);
    public static uint Max(uint val1, uint val2);
    public static ulong Max(ulong val1, ulong val2);
    public static double MaxMagnitude(double x, double y);
    public static byte Min(byte val1, byte val2);
    public static decimal Min(decimal val1, decimal val2);
    public static double Min(double val1, double val2);
    public static short Min(short val1, short val2);
    public static int Min(int val1, int val2);
    public static long Min(long val1, long val2);
    public static sbyte Min(sbyte val1, sbyte val2);
    public static float Min(float val1, float val2);
    public static ushort Min(ushort val1, ushort val2);
    public static uint Min(uint val1, uint val2);
    public static ulong Min(ulong val1, ulong val2);
    public static double MinMagnitude(double x, double y);
    public static double Pow(double x, double y);
    public static decimal Round(decimal d);
    public static decimal Round(decimal d, int decimals);
    public static decimal Round(decimal d, int decimals, MidpointRounding mode);
    public static decimal Round(decimal d, MidpointRounding mode);
    public static double Round(double a);
    public static double Round(double value, int digits);
    public static double Round(double value, int digits, MidpointRounding mode);
    public static double Round(double value, MidpointRounding mode);
    public static double ScaleB(double x, int n);
    public static int Sign(decimal value);
    public static int Sign(double value);
    public static int Sign(short value);
    public static int Sign(int value);
    public static int Sign(long value);
    public static int Sign(sbyte value);
    public static int Sign(float value);
    public static double Sin(double a);
    public static double Sinh(double value);
    public static double Sqrt(double d);
    public static double Tan(double a);
    public static double Tanh(double value);
    public static decimal Truncate(decimal d);
    public static double Truncate(double d);
  }
  public static class MathF {
    public const float E = 2.71828175f;
    public const float PI = 3.14159274f;
    public static float Abs(float x);
    public static float Acos(float x);
    public static float Acosh(float x);
    public static float Asin(float x);
    public static float Asinh(float x);
    public static float Atan(float x);
    public static float Atan2(float y, float x);
    public static float Atanh(float x);
    public static float BitDecrement(float x);
    public static float BitIncrement(float x);
    public static float Cbrt(float x);
    public static float Ceiling(float x);
    public static float CopySign(float x, float y);
    public static float Cos(float x);
    public static float Cosh(float x);
    public static float Exp(float x);
    public static float Floor(float x);
    public static float FusedMultiplyAdd(float x, float y, float z);
    public static float IEEERemainder(float x, float y);
    public static int ILogB(float x);
    public static float Log(float x);
    public static float Log(float x, float y);
    public static float Log10(float x);
    public static float Log2(float x);
    public static float Max(float x, float y);
    public static float MaxMagnitude(float x, float y);
    public static float Min(float x, float y);
    public static float MinMagnitude(float x, float y);
    public static float Pow(float x, float y);
    public static float Round(float x);
    public static float Round(float x, int digits);
    public static float Round(float x, int digits, MidpointRounding mode);
    public static float Round(float x, MidpointRounding mode);
    public static float ScaleB(float x, int n);
    public static int Sign(float x);
    public static float Sin(float x);
    public static float Sinh(float x);
    public static float Sqrt(float x);
    public static float Tan(float x);
    public static float Tanh(float x);
    public static float Truncate(float x);
  }
  public sealed class OperatingSystem : ICloneable, ISerializable {
    public OperatingSystem(PlatformID platform, Version version);
    public PlatformID Platform { get; }
    public string ServicePack { get; }
    public Version Version { get; }
    public string VersionString { get; }
    public object Clone();
    public void GetObjectData(SerializationInfo info, StreamingContext context);
    public override string ToString();
  }
  public enum PlatformID {
    MacOSX = 6,
    Unix = 4,
    Win32NT = 2,
    Win32S = 0,
    Win32Windows = 1,
    WinCE = 3,
    Xbox = 5,
  }
  public class Progress<T> : IProgress<T> {
    public Progress();
    public Progress(Action<T> handler);
    public event EventHandler<T> ProgressChanged;
    protected virtual void OnReport(T value);
    void System.IProgress<T>.ReportIProgress<T>.Report(T value);
  }
  public class Random {
    public Random();
    public Random(int Seed);
    public virtual int Next();
    public virtual int Next(int maxValue);
    public virtual int Next(int minValue, int maxValue);
    public virtual void NextBytes(byte[] buffer);
    public virtual void NextBytes(Span<byte> buffer);
    public virtual double NextDouble();
    protected virtual double Sample();
  }
  public delegate Assembly ResolveEventHandler(object? sender, ResolveEventArgs args);
  public abstract class StringComparer : IComparer, IComparer<string?>, IEqualityComparer, IEqualityComparer<string?> {
    protected StringComparer();
    public static StringComparer CurrentCulture { get; }
    public static StringComparer CurrentCultureIgnoreCase { get; }
    public static StringComparer InvariantCulture { get; }
    public static StringComparer InvariantCultureIgnoreCase { get; }
    public static StringComparer Ordinal { get; }
    public static StringComparer OrdinalIgnoreCase { get; }
    public int Compare(object? x, object? y);
    public abstract int Compare(string? x, string? y);
    public static StringComparer Create(CultureInfo culture, bool ignoreCase);
    public static StringComparer Create(CultureInfo culture, CompareOptions options);
    public bool Equals(object? x, object? y);
    public abstract bool Equals(string? x, string? y);
    public static StringComparer FromComparison(StringComparison comparisonType);
    public int GetHashCode(object obj);
    public abstract int GetHashCode(string? obj);
  }
  public static class StringNormalizationExtensions {
    public static bool IsNormalized(this string value);
    public static bool IsNormalized(this string value, NormalizationForm normalizationForm);
    public static string Normalize(this string value);
    public static string Normalize(this string value, NormalizationForm normalizationForm);
  }
  public class UriBuilder {
    public UriBuilder();
    public UriBuilder(string uri);
    public UriBuilder(string schemeName, string hostName);
    public UriBuilder(string scheme, string host, int portNumber);
    public UriBuilder(string scheme, string host, int port, string pathValue);
    public UriBuilder(string scheme, string host, int port, string path, string extraValue);
    public UriBuilder(Uri uri);
    public string Fragment { get; set; }
    public string Host { get; set; }
    public string Password { get; set; }
    public string Path { get; set; }
    public int Port { get; set; }
    public string Query { get; set; }
    public string Scheme { get; set; }
    public Uri Uri { get; }
    public string UserName { get; set; }
    public override bool Equals(object rparam);
    public override int GetHashCode();
    public override string ToString();
  }
 }
 namespace System.CodeDom.Compiler {
  public class IndentedTextWriter : TextWriter {
    public const string DefaultTabString = "    ";
    public IndentedTextWriter(TextWriter writer);
    public IndentedTextWriter(TextWriter writer, string tabString);
    public override Encoding Encoding { get; }
    public int Indent { get; set; }
    public TextWriter InnerWriter { get; }
    public override string NewLine { get; set; }
    public override void Close();
    public override void Flush();
    protected virtual void OutputTabs();
    public override void Write(bool value);
    public override void Write(char value);
    public override void Write(char[]? buffer);
    public override void Write(char[] buffer, int index, int count);
    public override void Write(double value);
    public override void Write(int value);
    public override void Write(long value);
    public override void Write(object? value);
    public override void Write(float value);
    public override void Write(string? s);
    public override void Write(string format, object? arg0);
    public override void Write(string format, object? arg0, object? arg1);
    public override void Write(string format, params object?[] arg);
    public override void WriteLine();
    public override void WriteLine(bool value);
    public override void WriteLine(char value);
    public override void WriteLine(char[]? buffer);
    public override void WriteLine(char[] buffer, int index, int count);
    public override void WriteLine(double value);
    public override void WriteLine(int value);
    public override void WriteLine(long value);
    public override void WriteLine(object? value);
    public override void WriteLine(float value);
    public override void WriteLine(string? s);
    public override void WriteLine(string format, object? arg0);
    public override void WriteLine(string format, object? arg0, object? arg1);
    public override void WriteLine(string format, params object?[] arg);
    public override void WriteLine(uint value);
    public void WriteLineNoTabs(string? s);
  }
 }
 namespace System.Collections {
  public class ArrayList : ICloneable, ICollection, IEnumerable, IList {
    public ArrayList();
    public ArrayList(ICollection c);
    public ArrayList(int capacity);
    public virtual int Capacity { get; set; }
    public virtual int Count { get; }
    public virtual bool IsFixedSize { get; }
    public virtual bool IsReadOnly { get; }
    public virtual bool IsSynchronized { get; }
    public virtual object SyncRoot { get; }
    public virtual object? this[int index] { get; set; }
    public static ArrayList Adapter(IList list);
    public virtual int Add(object? value);
    public virtual void AddRange(ICollection c);
    public virtual int BinarySearch(int index, int count, object? value, IComparer? comparer);
    public virtual int BinarySearch(object? value);
    public virtual int BinarySearch(object? value, IComparer? comparer);
    public virtual void Clear();
    public virtual object Clone();
    public virtual bool Contains(object? item);
    public virtual void CopyTo(Array array);
    public virtual void CopyTo(Array array, int arrayIndex);
    public virtual void CopyTo(int index, Array array, int arrayIndex, int count);
    public static ArrayList FixedSize(ArrayList list);
    public static IList FixedSize(IList list);
    public virtual IEnumerator GetEnumerator();
    public virtual IEnumerator GetEnumerator(int index, int count);
    public virtual ArrayList GetRange(int index, int count);
    public virtual int IndexOf(object? value);
    public virtual int IndexOf(object? value, int startIndex);
    public virtual int IndexOf(object? value, int startIndex, int count);
    public virtual void Insert(int index, object? value);
    public virtual void InsertRange(int index, ICollection c);
    public virtual int LastIndexOf(object? value);
    public virtual int LastIndexOf(object? value, int startIndex);
    public virtual int LastIndexOf(object? value, int startIndex, int count);
    public static ArrayList ReadOnly(ArrayList list);
    public static IList ReadOnly(IList list);
    public virtual void Remove(object? obj);
    public virtual void RemoveAt(int index);
    public virtual void RemoveRange(int index, int count);
    public static ArrayList Repeat(object? value, int count);
    public virtual void Reverse();
    public virtual void Reverse(int index, int count);
    public virtual void SetRange(int index, ICollection c);
    public virtual void Sort();
    public virtual void Sort(IComparer? comparer);
    public virtual void Sort(int index, int count, IComparer? comparer);
    public static ArrayList Synchronized(ArrayList list);
    public static IList Synchronized(IList list);
    public virtual object?[] ToArray();
    public virtual Array ToArray(Type type);
    public virtual void TrimToSize();
  }
  public sealed class Comparer : IComparer, ISerializable {
    public static readonly Comparer Default;
    public static readonly Comparer DefaultInvariant;
    public Comparer(CultureInfo culture);
    public int Compare(object? a, object? b);
    public void GetObjectData(SerializationInfo info, StreamingContext context);
  }
  public class Hashtable : ICloneable, ICollection, IDeserializationCallback, IDictionary, IEnumerable, ISerializable {
    public Hashtable();
    public Hashtable(IDictionary d);
    public Hashtable(IDictionary d, IEqualityComparer? equalityComparer);
    public Hashtable(IDictionary d, IHashCodeProvider? hcp, IComparer? comparer);
    public Hashtable(IDictionary d, float loadFactor);
    public Hashtable(IDictionary d, float loadFactor, IEqualityComparer? equalityComparer);
    public Hashtable(IDictionary d, float loadFactor, IHashCodeProvider? hcp, IComparer? comparer);
    public Hashtable(IEqualityComparer? equalityComparer);
    public Hashtable(IHashCodeProvider? hcp, IComparer? comparer);
    public Hashtable(int capacity);
    public Hashtable(int capacity, IEqualityComparer? equalityComparer);
    public Hashtable(int capacity, IHashCodeProvider? hcp, IComparer? comparer);
    public Hashtable(int capacity, float loadFactor);
    public Hashtable(int capacity, float loadFactor, IEqualityComparer? equalityComparer);
    public Hashtable(int capacity, float loadFactor, IHashCodeProvider? hcp, IComparer? comparer);
    protected Hashtable(SerializationInfo info, StreamingContext context);
    protected IComparer? comparer { get; set; }
    public virtual int Count { get; }
    protected IEqualityComparer? EqualityComparer { get; }
    protected IHashCodeProvider? hcp { get; set; }
    public virtual bool IsFixedSize { get; }
    public virtual bool IsReadOnly { get; }
    public virtual bool IsSynchronized { get; }
    public virtual ICollection Keys { get; }
    public virtual object SyncRoot { get; }
    public virtual object? this[object key] { get; set; }
    public virtual ICollection Values { get; }
    public virtual void Add(object key, object? value);
    public virtual void Clear();
    public virtual object Clone();
    public virtual bool Contains(object key);
    public virtual bool ContainsKey(object key);
    public virtual bool ContainsValue(object? value);
    public virtual void CopyTo(Array array, int arrayIndex);
    public virtual IDictionaryEnumerator GetEnumerator();
    protected virtual int GetHash(object key);
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    protected virtual bool KeyEquals(object? item, object key);
    public virtual void OnDeserialization(object sender);
    public virtual void Remove(object key);
    public static Hashtable Synchronized(Hashtable table);
    IEnumerator System.Collections.IEnumerable.GetEnumerator();
  }
  public interface IHashCodeProvider {
    int GetHashCode(object obj);
  }
 }
 namespace System.Diagnostics {
  public class Stopwatch {
    public static readonly bool IsHighResolution;
    public static readonly long Frequency;
    public Stopwatch();
    public TimeSpan Elapsed { get; }
    public long ElapsedMilliseconds { get; }
    public long ElapsedTicks { get; }
    public bool IsRunning { get; }
    public static long GetTimestamp();
    public void Reset();
    public void Restart();
    public void Start();
    public static Stopwatch StartNew();
    public void Stop();
  }
 }
 namespace System.Globalization {
  public static class GlobalizationExtensions {
    public static StringComparer GetStringComparer(this CompareInfo compareInfo, CompareOptions options);
  }
 }
 namespace System.IO {
  public class BinaryReader : IDisposable {
    public BinaryReader(Stream input);
    public BinaryReader(Stream input, Encoding encoding);
    public BinaryReader(Stream input, Encoding encoding, bool leaveOpen);
    public virtual Stream BaseStream { get; }
    public virtual void Close();
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    protected virtual void FillBuffer(int numBytes);
    public virtual int PeekChar();
    public virtual int Read();
    public virtual int Read(byte[] buffer, int index, int count);
    public virtual int Read(char[] buffer, int index, int count);
    public virtual int Read(Span<byte> buffer);
    public virtual int Read(Span<char> buffer);
    protected internal int Read7BitEncodedInt();
    public virtual bool ReadBoolean();
    public virtual byte ReadByte();
    public virtual byte[] ReadBytes(int count);
    public virtual char ReadChar();
    public virtual char[] ReadChars(int count);
    public virtual decimal ReadDecimal();
    public virtual double ReadDouble();
    public virtual short ReadInt16();
    public virtual int ReadInt32();
    public virtual long ReadInt64();
    public virtual sbyte ReadSByte();
    public virtual float ReadSingle();
    public virtual string ReadString();
    public virtual ushort ReadUInt16();
    public virtual uint ReadUInt32();
    public virtual ulong ReadUInt64();
  }
  public class BinaryWriter : IAsyncDisposable, IDisposable {
    public static readonly BinaryWriter Null;
    protected Stream OutStream;
    protected BinaryWriter();
    public BinaryWriter(Stream output);
    public BinaryWriter(Stream output, Encoding encoding);
    public BinaryWriter(Stream output, Encoding encoding, bool leaveOpen);
    public virtual Stream BaseStream { get; }
    public virtual void Close();
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public virtual ValueTask DisposeAsync();
    public virtual void Flush();
    public virtual long Seek(int offset, SeekOrigin origin);
    public virtual void Write(bool value);
    public virtual void Write(byte value);
    public virtual void Write(byte[] buffer);
    public virtual void Write(byte[] buffer, int index, int count);
    public virtual void Write(char ch);
    public virtual void Write(char[] chars);
    public virtual void Write(char[] chars, int index, int count);
    public virtual void Write(decimal value);
    public virtual void Write(double value);
    public virtual void Write(short value);
    public virtual void Write(int value);
    public virtual void Write(long value);
    public virtual void Write(ReadOnlySpan<byte> buffer);
    public virtual void Write(ReadOnlySpan<char> chars);
    public virtual void Write(sbyte value);
    public virtual void Write(float value);
    public virtual void Write(string value);
    public virtual void Write(ushort value);
    public virtual void Write(uint value);
    public virtual void Write(ulong value);
    protected void Write7BitEncodedInt(int value);
  }
  public sealed class BufferedStream : Stream {
    public BufferedStream(Stream stream);
    public BufferedStream(Stream stream, int bufferSize);
    public int BufferSize { get; }
    public override bool CanRead { get; }
    public override bool CanSeek { get; }
    public override bool CanWrite { get; }
    public override long Length { get; }
    public override long Position { get; set; }
    public Stream? UnderlyingStream { get; }
    public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback callback, object? state);
    public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback callback, object? state);
    public override void CopyTo(Stream destination, int bufferSize);
    public override Task CopyToAsync(Stream destination, int bufferSize, CancellationToken cancellationToken);
    protected override void Dispose(bool disposing);
    public override ValueTask DisposeAsync();
    public override int EndRead(IAsyncResult asyncResult);
    public override void EndWrite(IAsyncResult asyncResult);
    public override void Flush();
    public override Task FlushAsync(CancellationToken cancellationToken);
    public override int Read(byte[] array, int offset, int count);
    public override int Read(Span<byte> destination);
    public override Task<int> ReadAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
    public override ValueTask<int> ReadAsync(Memory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override int ReadByte();
    public override long Seek(long offset, SeekOrigin origin);
    public override void SetLength(long value);
    public override void Write(byte[] array, int offset, int count);
    public override void Write(ReadOnlySpan<byte> buffer);
    public override Task WriteAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
    public override ValueTask WriteAsync(ReadOnlyMemory<byte> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override void WriteByte(byte value);
  }
  public class EndOfStreamException : IOException {
    public EndOfStreamException();
    protected EndOfStreamException(SerializationInfo info, StreamingContext context);
    public EndOfStreamException(string? message);
    public EndOfStreamException(string? message, Exception? innerException);
  }
  public sealed class InvalidDataException : SystemException {
    public InvalidDataException();
    public InvalidDataException(string? message);
    public InvalidDataException(string? message, Exception? innerException);
  }
  public class MemoryStream : Stream {
    public MemoryStream();
    public MemoryStream(byte[] buffer);
    public MemoryStream(byte[] buffer, bool writable);
    public MemoryStream(byte[] buffer, int index, int count);
    public MemoryStream(byte[] buffer, int index, int count, bool writable);
    public MemoryStream(byte[] buffer, int index, int count, bool writable, bool publiclyVisible);
    public MemoryStream(int capacity);
    public override bool CanRead { get; }
    public override bool CanSeek { get; }
    public override bool CanWrite { get; }
    public virtual int Capacity { get; set; }
    public override long Length { get; }
    public override long Position { get; set; }
    public override IAsyncResult BeginRead(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
    public override IAsyncResult BeginWrite(byte[] buffer, int offset, int count, AsyncCallback callback, object state);
    public override void CopyTo(Stream destination, int bufferSize);
    public override Task CopyToAsync(Stream destination, int bufferSize, CancellationToken cancellationToken);
    protected override void Dispose(bool disposing);
    public override int EndRead(IAsyncResult asyncResult);
    public override void EndWrite(IAsyncResult asyncResult);
    public override void Flush();
    public override Task FlushAsync(CancellationToken cancellationToken);
    public virtual byte[] GetBuffer();
    public override int Read(byte[] buffer, int offset, int count);
    public override int Read(Span<byte> destination);
    public override Task<int> ReadAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
    public override ValueTask<int> ReadAsync(Memory<byte> destination, CancellationToken cancellationToken = default(CancellationToken));
    public override int ReadByte();
    public override long Seek(long offset, SeekOrigin loc);
    public override void SetLength(long value);
    public virtual byte[] ToArray();
    public virtual bool TryGetBuffer(out ArraySegment<byte> buffer);
    public override void Write(byte[] buffer, int offset, int count);
    public override void Write(ReadOnlySpan<byte> source);
    public override Task WriteAsync(byte[] buffer, int offset, int count, CancellationToken cancellationToken);
    public override ValueTask WriteAsync(ReadOnlyMemory<byte> source, CancellationToken cancellationToken = default(CancellationToken));
    public override void WriteByte(byte value);
    public virtual void WriteTo(Stream stream);
  }
  public static class Path {
    public static readonly char AltDirectorySeparatorChar;
    public static readonly char DirectorySeparatorChar;
    public static readonly char PathSeparator;
    public static readonly char VolumeSeparatorChar;
    public static readonly char[] InvalidPathChars;
    public static string? ChangeExtension(string? path, string? extension);
    public static string Combine(string path1, string path2);
    public static string Combine(string path1, string path2, string path3);
    public static string Combine(string path1, string path2, string path3, string path4);
    public static string Combine(params string[] paths);
+   public static bool EndsInDirectorySeparator(ReadOnlySpan<char> path);
+   public static bool EndsInDirectorySeparator(string path);
    public static ReadOnlySpan<char> GetDirectoryName(ReadOnlySpan<char> path);
    public static string? GetDirectoryName(string? path);
    public static ReadOnlySpan<char> GetExtension(ReadOnlySpan<char> path);
    public static string? GetExtension(string? path);
    public static ReadOnlySpan<char> GetFileName(ReadOnlySpan<char> path);
    public static string? GetFileName(string? path);
    public static ReadOnlySpan<char> GetFileNameWithoutExtension(ReadOnlySpan<char> path);
    public static string? GetFileNameWithoutExtension(string? path);
    public static string GetFullPath(string path);
    public static string GetFullPath(string path, string basePath);
    public static char[] GetInvalidFileNameChars();
    public static char[] GetInvalidPathChars();
    public static ReadOnlySpan<char> GetPathRoot(ReadOnlySpan<char> path);
    public static string? GetPathRoot(string? path);
    public static string GetRandomFileName();
    public static string GetRelativePath(string relativeTo, string path);
    public static string GetTempFileName();
    public static string GetTempPath();
    public static bool HasExtension(ReadOnlySpan<char> path);
    public static bool HasExtension(string? path);
    public static bool IsPathFullyQualified(ReadOnlySpan<char> path);
    public static bool IsPathFullyQualified(string path);
    public static bool IsPathRooted(ReadOnlySpan<char> path);
    public static bool IsPathRooted(string? path);
    public static string Join(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2);
    public static string Join(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, ReadOnlySpan<char> path3);
    public static string Join(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, ReadOnlySpan<char> path3, ReadOnlySpan<char> path4);
    public static string Join(string? path1, string? path2);
    public static string Join(string? path1, string? path2, string? path3);
    public static string Join(string? path1, string? path2, string? path3, string? path4);
    public static string Join(params string?[] paths);
+   public static ReadOnlySpan<char> TrimEndingDirectorySeparator(ReadOnlySpan<char> path);
+   public static string TrimEndingDirectorySeparator(string path);
    public static bool TryJoin(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, ReadOnlySpan<char> path3, Span<char> destination, out int charsWritten);
    public static bool TryJoin(ReadOnlySpan<char> path1, ReadOnlySpan<char> path2, Span<char> destination, out int charsWritten);
  }
  public class StreamReader : TextReader {
    public static readonly new StreamReader Null;
    public StreamReader(Stream stream);
    public StreamReader(Stream stream, bool detectEncodingFromByteOrderMarks);
    public StreamReader(Stream stream, Encoding encoding);
    public StreamReader(Stream stream, Encoding encoding, bool detectEncodingFromByteOrderMarks);
    public StreamReader(Stream stream, Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize);
    public StreamReader(Stream stream, Encoding? encoding = null, bool detectEncodingFromByteOrderMarks = true, int bufferSize = -1, bool leaveOpen = false);
    public StreamReader(string path);
    public StreamReader(string path, bool detectEncodingFromByteOrderMarks);
    public StreamReader(string path, Encoding encoding);
    public StreamReader(string path, Encoding encoding, bool detectEncodingFromByteOrderMarks);
    public StreamReader(string path, Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize);
    public virtual Stream BaseStream { get; }
    public virtual Encoding CurrentEncoding { get; }
    public bool EndOfStream { get; }
    public override void Close();
    public void DiscardBufferedData();
    protected override void Dispose(bool disposing);
    public override int Peek();
    public override int Read();
    public override int Read(char[] buffer, int index, int count);
    public override int Read(Span<char> buffer);
    public override Task<int> ReadAsync(char[] buffer, int index, int count);
    public override ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override int ReadBlock(char[] buffer, int index, int count);
    public override int ReadBlock(Span<char> buffer);
    public override Task<int> ReadBlockAsync(char[] buffer, int index, int count);
    public override ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override string? ReadLine();
    public override Task<string?> ReadLineAsync();
    public override string ReadToEnd();
    public override Task<string> ReadToEndAsync();
  }
  public class StreamWriter : TextWriter {
    public static readonly new StreamWriter Null;
    public StreamWriter(Stream stream);
    public StreamWriter(Stream stream, Encoding encoding);
    public StreamWriter(Stream stream, Encoding encoding, int bufferSize);
    public StreamWriter(Stream stream, Encoding? encoding = null, int bufferSize = -1, bool leaveOpen = false);
    public StreamWriter(string path);
    public StreamWriter(string path, bool append);
    public StreamWriter(string path, bool append, Encoding encoding);
    public StreamWriter(string path, bool append, Encoding encoding, int bufferSize);
    public virtual bool AutoFlush { get; set; }
    public virtual Stream BaseStream { get; }
    public override Encoding Encoding { get; }
    public override void Close();
    protected override void Dispose(bool disposing);
    public override ValueTask DisposeAsync();
    public override void Flush();
    public override Task FlushAsync();
    public override void Write(char value);
    public override void Write(char[]? buffer);
    public override void Write(char[] buffer, int index, int count);
    public override void Write(ReadOnlySpan<char> buffer);
    public override void Write(string? value);
    public override void Write(string format, object? arg0);
    public override void Write(string format, object? arg0, object? arg1);
    public override void Write(string format, object? arg0, object? arg1, object? arg2);
    public override void Write(string format, params object?[] arg);
    public override Task WriteAsync(char value);
    public override Task WriteAsync(char[] buffer, int index, int count);
    public override Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override Task WriteAsync(string? value);
    public override void WriteLine(ReadOnlySpan<char> buffer);
    public override void WriteLine(string? value);
    public override void WriteLine(string format, object? arg0);
    public override void WriteLine(string format, object? arg0, object? arg1);
    public override void WriteLine(string format, object? arg0, object? arg1, object? arg2);
    public override void WriteLine(string format, params object?[] arg);
    public override Task WriteLineAsync();
    public override Task WriteLineAsync(char value);
    public override Task WriteLineAsync(char[] buffer, int index, int count);
    public override Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override Task WriteLineAsync(string? value);
  }
  public class StringReader : TextReader {
    public StringReader(string s);
    public override void Close();
    protected override void Dispose(bool disposing);
    public override int Peek();
    public override int Read();
    public override int Read(char[] buffer, int index, int count);
    public override int Read(Span<char> buffer);
    public override Task<int> ReadAsync(char[] buffer, int index, int count);
    public override ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override int ReadBlock(Span<char> buffer);
    public override Task<int> ReadBlockAsync(char[] buffer, int index, int count);
    public override ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override string? ReadLine();
    public override Task<string?> ReadLineAsync();
    public override string ReadToEnd();
    public override Task<string> ReadToEndAsync();
  }
  public class StringWriter : TextWriter {
    public StringWriter();
    public StringWriter(IFormatProvider? formatProvider);
    public StringWriter(StringBuilder sb);
    public StringWriter(StringBuilder sb, IFormatProvider? formatProvider);
    public override Encoding Encoding { get; }
    public override void Close();
    protected override void Dispose(bool disposing);
    public override Task FlushAsync();
    public virtual StringBuilder GetStringBuilder();
    public override string ToString();
    public override void Write(char value);
    public override void Write(char[] buffer, int index, int count);
    public override void Write(ReadOnlySpan<char> buffer);
    public override void Write(string? value);
    public override void Write(StringBuilder? value);
    public override Task WriteAsync(char value);
    public override Task WriteAsync(char[] buffer, int index, int count);
    public override Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override Task WriteAsync(string? value);
    public override Task WriteAsync(StringBuilder? value, CancellationToken cancellationToken = default(CancellationToken));
    public override void WriteLine(ReadOnlySpan<char> buffer);
    public override void WriteLine(StringBuilder? value);
    public override Task WriteLineAsync(char value);
    public override Task WriteLineAsync(char[] buffer, int index, int count);
    public override Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public override Task WriteLineAsync(string? value);
    public override Task WriteLineAsync(StringBuilder? value, CancellationToken cancellationToken = default(CancellationToken));
  }
  public abstract class TextReader : MarshalByRefObject, IDisposable {
    public static readonly TextReader Null;
    protected TextReader();
    public virtual void Close();
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public virtual int Peek();
    public virtual int Read();
    public virtual int Read(char[] buffer, int index, int count);
    public virtual int Read(Span<char> buffer);
    public virtual Task<int> ReadAsync(char[] buffer, int index, int count);
    public virtual ValueTask<int> ReadAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public virtual int ReadBlock(char[] buffer, int index, int count);
    public virtual int ReadBlock(Span<char> buffer);
    public virtual Task<int> ReadBlockAsync(char[] buffer, int index, int count);
    public virtual ValueTask<int> ReadBlockAsync(Memory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public virtual string? ReadLine();
    public virtual Task<string?> ReadLineAsync();
    public virtual string ReadToEnd();
    public virtual Task<string> ReadToEndAsync();
    public static TextReader Synchronized(TextReader reader);
  }
  public abstract class TextWriter : MarshalByRefObject, IAsyncDisposable, IDisposable {
    protected char[] CoreNewLine;
    public static readonly TextWriter Null;
    protected TextWriter();
    protected TextWriter(IFormatProvider? formatProvider);
    public abstract Encoding Encoding { get; }
    public virtual IFormatProvider FormatProvider { get; }
    public virtual string NewLine { get; set; }
    public virtual void Close();
    public void Dispose();
    protected virtual void Dispose(bool disposing);
    public virtual ValueTask DisposeAsync();
    public virtual void Flush();
    public virtual Task FlushAsync();
    public static TextWriter Synchronized(TextWriter writer);
    public virtual void Write(bool value);
    public virtual void Write(char value);
    public virtual void Write(char[]? buffer);
    public virtual void Write(char[] buffer, int index, int count);
    public virtual void Write(decimal value);
    public virtual void Write(double value);
    public virtual void Write(int value);
    public virtual void Write(long value);
    public virtual void Write(object? value);
    public virtual void Write(ReadOnlySpan<char> buffer);
    public virtual void Write(float value);
    public virtual void Write(string? value);
    public virtual void Write(string format, object? arg0);
    public virtual void Write(string format, object? arg0, object? arg1);
    public virtual void Write(string format, object? arg0, object? arg1, object? arg2);
    public virtual void Write(string format, params object?[] arg);
    public virtual void Write(StringBuilder? value);
    public virtual void Write(uint value);
    public virtual void Write(ulong value);
    public virtual Task WriteAsync(char value);
    public Task WriteAsync(char[]? buffer);
    public virtual Task WriteAsync(char[] buffer, int index, int count);
    public virtual Task WriteAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public virtual Task WriteAsync(string? value);
    public virtual Task WriteAsync(StringBuilder? value, CancellationToken cancellationToken = default(CancellationToken));
    public virtual void WriteLine();
    public virtual void WriteLine(bool value);
    public virtual void WriteLine(char value);
    public virtual void WriteLine(char[]? buffer);
    public virtual void WriteLine(char[] buffer, int index, int count);
    public virtual void WriteLine(decimal value);
    public virtual void WriteLine(double value);
    public virtual void WriteLine(int value);
    public virtual void WriteLine(long value);
    public virtual void WriteLine(object? value);
    public virtual void WriteLine(ReadOnlySpan<char> buffer);
    public virtual void WriteLine(float value);
    public virtual void WriteLine(string? value);
    public virtual void WriteLine(string format, object? arg0);
    public virtual void WriteLine(string format, object? arg0, object? arg1);
    public virtual void WriteLine(string format, object? arg0, object? arg1, object? arg2);
    public virtual void WriteLine(string format, params object?[] arg);
    public virtual void WriteLine(StringBuilder? value);
    public virtual void WriteLine(uint value);
    public virtual void WriteLine(ulong value);
    public virtual Task WriteLineAsync();
    public virtual Task WriteLineAsync(char value);
    public Task WriteLineAsync(char[]? buffer);
    public virtual Task WriteLineAsync(char[] buffer, int index, int count);
    public virtual Task WriteLineAsync(ReadOnlyMemory<char> buffer, CancellationToken cancellationToken = default(CancellationToken));
    public virtual Task WriteLineAsync(string? value);
    public virtual Task WriteLineAsync(StringBuilder? value, CancellationToken cancellationToken = default(CancellationToken));
  }
 }
 namespace System.Net {
  public static class WebUtility {
    public static string? HtmlDecode(string? value);
    public static void HtmlDecode(string? value, TextWriter output);
    public static string? HtmlEncode(string? value);
    public static void HtmlEncode(string? value, TextWriter output);
    public static string? UrlDecode(string? encodedValue);
    public static byte[]? UrlDecodeToBytes(byte[]? encodedValue, int offset, int count);
    public static string? UrlEncode(string? value);
    public static byte[]? UrlEncodeToBytes(byte[]? value, int offset, int count);
  }
 }
 namespace System.Numerics {
  public static class BitOperations {
    public static int LeadingZeroCount(uint value);
    public static int LeadingZeroCount(ulong value);
    public static int Log2(uint value);
    public static int Log2(ulong value);
    public static int PopCount(uint value);
    public static int PopCount(ulong value);
    public static uint RotateLeft(uint value, int offset);
    public static ulong RotateLeft(ulong value, int offset);
    public static uint RotateRight(uint value, int offset);
    public static ulong RotateRight(ulong value, int offset);
    public static int TrailingZeroCount(int value);
    public static int TrailingZeroCount(long value);
    public static int TrailingZeroCount(uint value);
    public static int TrailingZeroCount(ulong value);
  }
 }
 namespace System.Reflection {
  public class AssemblyNameProxy : MarshalByRefObject {
    public AssemblyNameProxy();
    public AssemblyName GetAssemblyName(string assemblyFile);
  }
 }
 namespace System.Runtime {
  public static class ProfileOptimization {
    public static void SetProfileRoot(string directoryPath);
    public static void StartProfile(string profile);
  }
 }
 namespace System.Runtime.CompilerServices {
  public sealed class SwitchExpressionException : InvalidOperationException {
    public SwitchExpressionException();
    public SwitchExpressionException(Exception? innerException);
    public SwitchExpressionException(object? unmatchedValue);
    public SwitchExpressionException(string? message);
    public SwitchExpressionException(string? message, Exception? innerException);
    public override string Message { get; }
    public object? UnmatchedValue { get; }
    public override void GetObjectData(SerializationInfo info, StreamingContext context);
  }
 }
 namespace System.Runtime.Versioning {
  public sealed class ComponentGuaranteesAttribute : Attribute {
    public ComponentGuaranteesAttribute(ComponentGuaranteesOptions guarantees);
    public ComponentGuaranteesOptions Guarantees { get; }
  }
  public enum ComponentGuaranteesOptions {
    Exchange = 1,
    None = 0,
    SideBySide = 4,
    Stable = 2,
  }
  public sealed class FrameworkName : IEquatable<FrameworkName?> {
    public FrameworkName(string frameworkName);
    public FrameworkName(string identifier, Version version);
    public FrameworkName(string identifier, Version version, string? profile);
    public string FullName { get; }
    public string Identifier { get; }
    public string Profile { get; }
    public Version Version { get; }
    public override bool Equals(object? obj);
    public bool Equals(FrameworkName? other);
    public override int GetHashCode();
    public static bool operator ==(FrameworkName? left, FrameworkName? right);
    public static bool operator !=(FrameworkName? left, FrameworkName? right);
    public override string ToString();
  }
  public sealed class ResourceConsumptionAttribute : Attribute {
    public ResourceConsumptionAttribute(ResourceScope resourceScope);
    public ResourceConsumptionAttribute(ResourceScope resourceScope, ResourceScope consumptionScope);
    public ResourceScope ConsumptionScope { get; }
    public ResourceScope ResourceScope { get; }
  }
  public sealed class ResourceExposureAttribute : Attribute {
    public ResourceExposureAttribute(ResourceScope exposureLevel);
    public ResourceScope ResourceExposureLevel { get; }
  }
  public enum ResourceScope {
    AppDomain = 4,
    Assembly = 32,
    Library = 8,
    Machine = 1,
    None = 0,
    Private = 16,
    Process = 2,
  }
  public static class VersioningHelper {
    public static string MakeVersionSafeName(string? name, ResourceScope from, ResourceScope to);
    public static string MakeVersionSafeName(string? name, ResourceScope from, ResourceScope to, Type? type);
  }
 }
 namespace System.Security {
  public interface IPermission : ISecurityEncodable {
    IPermission Copy();
    void Demand();
    IPermission? Intersect(IPermission? target);
    bool IsSubsetOf(IPermission? target);
    IPermission? Union(IPermission? target);
  }
  public interface ISecurityEncodable {
    void FromXml(SecurityElement e);
    SecurityElement? ToXml();
  }
  public interface IStackWalk {
    void Assert();
    void Demand();
    void Deny();
    void PermitOnly();
  }
  public class PermissionSet : ICollection, IDeserializationCallback, IEnumerable, ISecurityEncodable, IStackWalk {
    public PermissionSet(PermissionState state);
    public PermissionSet(PermissionSet? permSet);
    public virtual int Count { get; }
    public virtual bool IsReadOnly { get; }
    public virtual bool IsSynchronized { get; }
    public virtual object SyncRoot { get; }
    public IPermission? AddPermission(IPermission? perm);
    protected virtual IPermission? AddPermissionImpl(IPermission? perm);
    public void Assert();
    public bool ContainsNonCodeAccessPermissions();
    public static byte[] ConvertPermissionSet(string inFormat, byte[] inData, string outFormat);
    public virtual PermissionSet Copy();
    public virtual void CopyTo(Array array, int index);
    public void Demand();
    public void Deny();
    public override bool Equals(object? o);
    public virtual void FromXml(SecurityElement et);
    public IEnumerator GetEnumerator();
    protected virtual IEnumerator GetEnumeratorImpl();
    public override int GetHashCode();
    public IPermission? GetPermission(Type? permClass);
    protected virtual IPermission? GetPermissionImpl(Type? permClass);
    public PermissionSet? Intersect(PermissionSet? other);
    public bool IsEmpty();
    public bool IsSubsetOf(PermissionSet? target);
    public bool IsUnrestricted();
    public void PermitOnly();
    public IPermission? RemovePermission(Type? permClass);
    protected virtual IPermission? RemovePermissionImpl(Type? permClass);
    public static void RevertAssert();
    public IPermission? SetPermission(IPermission? perm);
    protected virtual IPermission? SetPermissionImpl(IPermission? perm);
    void System.Runtime.Serialization.IDeserializationCallback.OnDeserialization(object sender);
    public override string ToString();
    public virtual SecurityElement? ToXml();
    public PermissionSet? Union(PermissionSet? other);
  }
  public sealed class SecurityElement {
    public SecurityElement(string tag);
    public SecurityElement(string tag, string? text);
    public Hashtable? Attributes { get; set; }
    public ArrayList? Children { get; set; }
    public string Tag { get; set; }
    public string? Text { get; set; }
    public void AddAttribute(string name, string value);
    public void AddChild(SecurityElement child);
    public string? Attribute(string name);
    public SecurityElement Copy();
    public bool Equal(SecurityElement? other);
    public static string? Escape(string? str);
    public static SecurityElement? FromString(string xml);
    public static bool IsValidAttributeName(string? name);
    public static bool IsValidAttributeValue(string? value);
    public static bool IsValidTag(string? tag);
    public static bool IsValidText(string? text);
    public SecurityElement? SearchForChildByTag(string tag);
    public string? SearchForTextOfTag(string tag);
    public override string ToString();
  }
 }
 namespace System.Security.Permissions {
  public abstract class CodeAccessSecurityAttribute : SecurityAttribute {
    protected CodeAccessSecurityAttribute(SecurityAction action);
  }
  public enum PermissionState {
    None = 0,
    Unrestricted = 1,
  }
  public enum SecurityAction {
    Assert = 3,
    Demand = 2,
    Deny = 4,
    InheritanceDemand = 7,
    LinkDemand = 6,
    PermitOnly = 5,
    RequestMinimum = 8,
    RequestOptional = 9,
    RequestRefuse = 10,
  }
  public abstract class SecurityAttribute : Attribute {
    protected SecurityAttribute(SecurityAction action);
    public SecurityAction Action { get; set; }
    public bool Unrestricted { get; set; }
    public abstract IPermission? CreatePermission();
  }
  public sealed class SecurityPermissionAttribute : CodeAccessSecurityAttribute {
    public SecurityPermissionAttribute(SecurityAction action);
    public bool Assertion { get; set; }
    public bool BindingRedirects { get; set; }
    public bool ControlAppDomain { get; set; }
    public bool ControlDomainPolicy { get; set; }
    public bool ControlEvidence { get; set; }
    public bool ControlPolicy { get; set; }
    public bool ControlPrincipal { get; set; }
    public bool ControlThread { get; set; }
    public bool Execution { get; set; }
    public SecurityPermissionFlag Flags { get; set; }
    public bool Infrastructure { get; set; }
    public bool RemotingConfiguration { get; set; }
    public bool SerializationFormatter { get; set; }
    public bool SkipVerification { get; set; }
    public bool UnmanagedCode { get; set; }
    public override IPermission? CreatePermission();
  }
  public enum SecurityPermissionFlag {
    AllFlags = 16383,
    Assertion = 1,
    BindingRedirects = 8192,
    ControlAppDomain = 1024,
    ControlDomainPolicy = 256,
    ControlEvidence = 32,
    ControlPolicy = 64,
    ControlPrincipal = 512,
    ControlThread = 16,
    Execution = 8,
    Infrastructure = 4096,
    NoFlags = 0,
    RemotingConfiguration = 2048,
    SerializationFormatter = 128,
    SkipVerification = 4,
    UnmanagedCode = 2,
  }
 }
```
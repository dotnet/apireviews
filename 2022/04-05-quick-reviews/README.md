# API Review 04/05/2022

## Add Microseconds and Nanoseconds to TimeStamp, DateTime, DateTimeOffset, and TimeOnly

**Approved** | [#runtime/23799](https://github.com/dotnet/runtime/issues/23799#issuecomment-1089085382) | [Video](https://www.youtube.com/watch?v=WuIdjTRbD50&t=0h0m0s)

We changed the AddMicroseconds methods to take `double value` for consistency, flipped TicksPerNanoseconds to NanosecondsPerTick to get rid of an imprecise `double`, and cleaned up some singular/plural mismatches.

Otherwise, looks good as proposed.

```C#
namespace System {
    public struct DateTime {
    	/// <exception cref="ArgumentOutOfRangeException">When microsecond is not between 0 and 999.</exception>
        public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int microsecond);
    	/// <exception cref="ArgumentOutOfRangeException">When microsecond is not between 0 and 999.</exception>
        public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int microsecond, System.DateTimeKind kind);
    	/// <exception cref="ArgumentOutOfRangeException">When microsecond is not between 0 and 999.</exception>
        public DateTime(int year, int month, int day, int hour, int minute, int second, int millisecond, int microsecond, System.Globalization.Calendar calendar);
        /// <returns>The microseconds component, expressed as a value between 0 and 999.</returns>
        public int Microsecond { get; }
        /// <returns>The nanoseconds component, expressed as a value between 0 and 900.</returns>
        public int Nanosecond { get; }
    	/// <exception cref="ArgumentOutOfRangeException">The resulting DateTime is less than DateTime.MinValue or greater than DateTime.MaxValue.</exception>
        public DateTime AddMicroseconds(double value);
    }
    public struct DateTimeOffset {
    	/// <exception cref="ArgumentOutOfRangeException">When microseconds is not between 0 and 999.</exception>
        public DateTimeOffset(int year, int month, int day, int hour, int minute, int second, int millisecond, int microsecond, System.TimeSpan offset);
    	/// <exception cref="ArgumentOutOfRangeException">When microseconds is not between 0 and 999.</exception>
        public DateTimeOffset(int year, int month, int day, int hour, int minute, int second, int millisecond, int microsecond, System.TimeSpan offset, System.Globalization.Calendar calendar);
        /// <returns>The microseconds component, expressed as a value between 0 and 999.</returns>
        public int Microsecond { get; }
        /// <returns>The nanoseconds component, expressed as a value between 0 and 900.</returns>
        public int Nanosecond { get; }
        /// <exception cref="ArgumentOutOfRangeException">The resulting DateTimeOffset is less than DateTimeOffset.MinValue or greater than DateTimeOffset.MaxValue.</exception>
        public DateTimeOffset AddMicroseconds(double value);
    }
    public struct TimeSpan {
        public const long TicksPerMicrosecond = 10L;
        public const long NanosecondsPerTick = 100L;
    	/// <exception cref="ArgumentOutOfRangeException">When microseconds is not between 0 and 999.</exception>
        public TimeSpan(int days, int hours, int minutes, int seconds, int milliseconds, int microseconds);
        /// <returns>The microseconds component, expressed as a value between 0 and 999.</returns>
        public int Microseconds { get; }
        /// <returns>The nanoseconds component, expressed as a value between 0 and 900.</returns>
        public int Nanoseconds { get; }
        /// <returns>The total number of microseconds represented by this instance.</returns>
        public double TotalMicroseconds { get; }
        /// <returns>The total number of nanoseconds represented by this instance.</returns>
        /// <exception cref="OverflowException">When value starts to approach TimeSpan.MinValue or TimeSpan.MaxValue.</exception>
        public double TotalNanoseconds { get; }
        /// <exception cref="OverflowException">When value is less than TimeSpan.MinValue or greater than TimeSpan.MaxValue.</exception>
        public static TimeSpan FromMicroseconds(double value);
    }
    public struct TimeOnly {
        /// <param name="microsecond">The microsecond (0 through 999).</param>
        /// <exception cref="ArgumentOutOfRangeException">When microsecond is not between 0 and 999.</exception>
        public TimeOnly(int day, int hour, int minute, int second, int millisecond, int microsecond);
        /// <returns>The microsecond component of the time represented by this instant, expressed as a value between 0 and 999.</returns>
        public int Microsecond { get; }
        /// <returns>The nanosecond component of the time represented by this instant, expressed as a value between 0 and 900.</returns>
        public int Nanosecond { get; }
    }
}
```
## Allow access to child providers in ChainedConfigurationProvider

**Approved** | [#runtime/44486](https://github.com/dotnet/runtime/issues/44486#issuecomment-1089101502) | [Video](https://www.youtube.com/watch?v=WuIdjTRbD50&t=0h25m16s)

During discussion it was believed that exposing the IConfiguration directly instead of inventing the IEnumerable for it was a better approach.

```C#
namespace Microsoft.Extensions.Configuration
{
    public partial class ChainedConfigurationProvider : IConfigurationProvider, IDisposable
    {
        public IConfiguration Configuration { get; }
    }
}
```
## Add more string syntax type constants to StringSyntaxAttribute

**Approved** | [#runtime/65634](https://github.com/dotnet/runtime/issues/65634#issuecomment-1089123302) | [Video](https://www.youtube.com/watch?v=WuIdjTRbD50&t=0h38m30s)

We changed "DateFormat" to "DateOnlyFormat" and "TimeFormat" to "TimeOnlyFormat" to make it clear that each of them was excluding the aspects of the other.

```C#
namespace System.Diagnostics.CodeAnalysis
{
    public partial sealed class StringSyntaxAttribute : Attribute
    {
        //public const string DateTimeFormat = nameof(DateTimeFormat);
        //public const string Json = nameof(Json);
        //public const string Regex = nameof(Regex);

        public const string CompositeFormat = nameof(CompositeFormat);
        public const string DateOnlyFormat = nameof(DateOnlyFormat);
        public const string EnumFormat = nameof(EnumFormat);
        public const string GuidFormat = nameof(GuidFormat);
        public const string NumericFormat = nameof(NumericFormat);
        public const string TimeOnlyFormat = nameof(TimeOnlyFormat);
        public const string TimeSpanFormat = nameof(TimeSpanFormat);
        public const string Uri = nameof(Uri);
        public const string Xml = nameof(Xml);
    }
}
```
## APIs to support tar archives

**Approved** | [#runtime/65951](https://github.com/dotnet/runtime/issues/65951#issuecomment-1089225133) | [Video](https://www.youtube.com/watch?v=WuIdjTRbD50&t=0h56m36s)

* We added Stream overloads to the methods in TarFile
* We changed TarEntry* to *TarEntry
* We changed RegularFileV7 to V7RegularFile
* We removed the `Archive` particles from TarFile parameter names
* We did a whole lot of discussion about the technical feasibility of OOB-ness, and some potential API impact, but no final decisions are made (they would come as a separate proposal).
* We moved System.IO.UnixFileMode to System.Formats.Tar.TarFileMode (at least until we can come up with a unified representation).
* We expanded ATime/CTime/MTime to AccessTime/ChangeTime/ModificationTime, and GName/UName to GroupName/UserName.  We left UID/GID alone as those are the general names for those concepts.

```C#
namespace System.Formats.Tar
{
    // Easy to use straightforward archiving and extraction APIs.
    public static class TarFile
    {
        public static void CreateFromDirectory(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory);
        public static Task CreateFromDirectoryAsync(string sourceDirectoryName, string destinationFileName, bool includeBaseDirectory, CancellationToken cancellationToken = default);
        public static void CreateFromDirectory(string sourceDirectoryName, Stream destination, bool includeBaseDirectory);
        public static Task CreateFromDirectoryAsync(string sourceDirectoryName, Stream destination, bool includeBaseDirectory, CancellationToken cancellationToken = default);
        public static void ExtractToDirectory(string sourceFileName, string destinationDirectoryName, bool overwriteFiles);
        public static Task ExtractToDirectoryAsync(string sourceFileName, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);
        public static void ExtractToDirectory(Stream source, string destinationDirectoryName, bool overwriteFiles);
        public static Task ExtractToDirectoryAsync(Stream source, string destinationDirectoryName, bool overwriteFiles, CancellationToken cancellationToken = default);
    }

    // Enum representing the entry types that can be detected from V7, Ustar, PAX and GNU.
    public enum TarEntryType : byte
    {
        V7RegularFile = '\0', // Used exclusively by V7

        // Used  by all formats
        RegularFile = '0',
        HardLink = '1',
        SymbolicLink = '2',
        CharacterDevice = '3',
        BlockDevice = '4',
        Directory = '5',
        Fifo = '6',

        // Exclusively used by PAX
        GlobalExtendedAttributes = 'g',
        ExtendedAttributes = 'x',

        // Exclusively used by GNU
        ContiguousFile = '7',
        DirectoryList = 'D',
        LongLink = 'K',
        LongPath = 'L',
        MultiVolume = 'M',
        RenamedOrSymlinked = 'N',
        SparseFile = 'S',
        TapeVolume = 'T',
    }
    
    // The formats these APIs will be able to read
    public enum TarFormat
    {
        Unknown = 0, // For when an archive that is being read is not recognized
        V7 = 1,
        Ustar = 2,
        Pax = 3,
        Gnu = 4,
    }

    // For traversing entries in an existing tar archive
    public sealed class TarReader : System.IDisposable, System.IAsyncDisposable
    {
        public TarReader(System.IO.Stream archiveStream, bool leaveOpen = false);
        public System.Formats.Tar.TarFormat Format { get; }
        public System.Collections.Generic.IReadOnlyDictionary<string, string>? GlobalExtendedAttributes { get; }
        public void Dispose();
        public ValueTask DisposeAsync();
        public System.Formats.Tar.TarEntry? GetNextEntry(bool copyData = false);
        public ValueTask<TarEntry?> GetNextEntryAsync(bool copyData = false, CancellationToken cancellationToken = default);
    }

    // For creating a tar archive
    public sealed class TarWriter : System.IDisposable, System.IAsyncDisposable
    {
        public TarWriter(System.IO.Stream archiveStream, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<string, string>>? globalExtendedAttributes = null, bool leaveOpen = false);
        public TarWriter(System.IO.Stream archiveStream, System.Formats.Tar.TarFormat archiveFormat, bool leaveOpen = false);
        public System.Formats.Tar.TarFormat Format { get; }
        public void Dispose();
        public ValueTask DisposeAsync();
        public void WriteEntry(string fileName, string? entryName);
        public Task WriteEntryAsync(string fileName, string? entryName, CancellationToken cancellationToken = default);
        public void WriteEntry(System.Formats.Tar.TarEntry entry);
        public Task WriteEntryAsync(TarEntry entry, CancellationToken cancellationToken = default);
    }

    // Abstract type to represent the header record's metadata fields
    // These fields are found in all the tar formats
    public abstract class TarEntry
    {
        internal TarEntry();
        public int Checksum { get; }
        public System.IO.Stream? DataStream { get; set; }
        public System.Formats.Tar.TarEntryType EntryType { get; }
        public int Gid { get; set; }
        public long Length { get; }
        public string LinkName { get; set; }
        public TarFileMode Mode { get; set; }
        public System.DateTimeOffset ModificationTime { get; set; }
        public string Name { get; set; }
        public int Uid { get; set; }
        public void ExtractToFile(string destinationFileName, bool overwrite);
        public Task ExtractToFileAsync(string destinationFileName, bool overwrite, CancellationToken cancellationToken = default);
        public override string ToString();
    }

    // Allows instancing a V7 tar entry
    public sealed class V7TarEntry : TarEntry
    {
        public V7TarEntry(System.Formats.Tar.TarEntryType entryType, string entryName);
    }

    // Abstract type that describe the fields proposed by POSIX and inherited by all the subsequent formats
    // GNU also inherits them, but notice that format is not POSIX, so the name for this abstract type can be different
    public abstract class PosixTarEntry : TarEntry
    {
        internal PosixTarEntry();
        public int DeviceMajor { get; set; }
        public int DeviceMinor { get; set; }
        public string GroupName { get; set; }
        public string UserName { get; set; }
        public override string ToString();
    }

    // Allows instancing a Ustar tar entry, which is the first POSIX format
    public sealed class UstarTarEntry : TarEntryPosix
    {
        public UstarTarEntry(System.Formats.Tar.TarEntryType entryType, string entryName);
    }

    // Allows instancing a PAX tar entry
    // Contains a dictionary that exposes the extended attributes found in the extra metadata entry the Pax format defines
    public sealed class PaxTarEntry : TarEntryPosix
    {
        public PaxTarEntry(System.Formats.Tar.TarEntryType entryType, string entryName, System.Collections.Generic.IEnumerable<System.Collections.Generic.KeyValuePair<string, string>>? extendedAttributes);
        public System.Collections.Generic.IReadOnlyDictionary<string, string> ExtendedAttributes { get; }
    }

    // Allows instancing a GNU tar entry
    // Contains additional metadata fields found in GNU
    public sealed class GnuTarEntry : TarEntryPosix
    {
        public GnuTarEntry(System.Formats.Tar.TarEntryType entryType, string entryName);
        public System.DateTimeOffset AccessTime { get; set; }
        public System.DateTimeOffset ChangeTime { get; set; }
    }

    [System.FlagsAttribute]
    public enum TarFileMode
    {
        None = 0,
        OtherExecute = 1,
        OtherWrite = 2,
        OtherRead = 4,
        GroupExecute = 8,
        GroupWrite = 16,
        GroupRead = 32,
        UserExecute = 64,
        UserWrite = 128,
        UserRead = 256,
        StickyBit = 512,
        GroupSpecial = 1024,
        UserSpecial = 2048,
    }
}
```

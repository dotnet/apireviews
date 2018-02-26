# System.IO.FileSystem

```diff
---System.IO.FileSystem.dll (.NET Core 2.0)
+++System.IO.FileSystem.dll (.NET Core 2.1)
 namespace System.IO {
  public static class Directory {
    public static DirectoryInfo CreateDirectory(string path);
    public static void Delete(string path);
    public static void Delete(string path, bool recursive);
    public static IEnumerable<string> EnumerateDirectories(string path);
    public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern, SearchOption searchOption);
    public static IEnumerable<string> EnumerateFiles(string path);
    public static IEnumerable<string> EnumerateFiles(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateFiles(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static IEnumerable<string> EnumerateFiles(string path, string searchPattern, SearchOption searchOption);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern, SearchOption searchOption);
    public static bool Exists(string path);
    public static DateTime GetCreationTime(string path);
    public static DateTime GetCreationTimeUtc(string path);
    public static string GetCurrentDirectory();
    public static string[] GetDirectories(string path);
    public static string[] GetDirectories(string path, string searchPattern);
+   public static string[] GetDirectories(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static string[] GetDirectories(string path, string searchPattern, SearchOption searchOption);
    public static string GetDirectoryRoot(string path);
    public static string[] GetFiles(string path);
    public static string[] GetFiles(string path, string searchPattern);
+   public static string[] GetFiles(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static string[] GetFiles(string path, string searchPattern, SearchOption searchOption);
    public static string[] GetFileSystemEntries(string path);
    public static string[] GetFileSystemEntries(string path, string searchPattern);
+   public static string[] GetFileSystemEntries(string path, string searchPattern, EnumerationOptions enumerationOptions);
    public static string[] GetFileSystemEntries(string path, string searchPattern, SearchOption searchOption);
    public static DateTime GetLastAccessTime(string path);
    public static DateTime GetLastAccessTimeUtc(string path);
    public static DateTime GetLastWriteTime(string path);
    public static DateTime GetLastWriteTimeUtc(string path);
    public static string[] GetLogicalDrives();
    public static DirectoryInfo GetParent(string path);
    public static void Move(string sourceDirName, string destDirName);
    public static void SetCreationTime(string path, DateTime creationTime);
    public static void SetCreationTimeUtc(string path, DateTime creationTimeUtc);
    public static void SetCurrentDirectory(string path);
    public static void SetLastAccessTime(string path, DateTime lastAccessTime);
    public static void SetLastAccessTimeUtc(string path, DateTime lastAccessTimeUtc);
    public static void SetLastWriteTime(string path, DateTime lastWriteTime);
    public static void SetLastWriteTimeUtc(string path, DateTime lastWriteTimeUtc);
  }
  public sealed class DirectoryInfo : FileSystemInfo {
    public DirectoryInfo(string path);
    public override bool Exists { get; }
    public override string Name { get; }
    public DirectoryInfo Parent { get; }
    public DirectoryInfo Root { get; }
    public void Create();
    public DirectoryInfo CreateSubdirectory(string path);
    public override void Delete();
    public void Delete(bool recursive);
    public IEnumerable<DirectoryInfo> EnumerateDirectories();
    public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern);
+   public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern, EnumerationOptions enumerationOptions);
    public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern, SearchOption searchOption);
    public IEnumerable<FileInfo> EnumerateFiles();
    public IEnumerable<FileInfo> EnumerateFiles(string searchPattern);
+   public IEnumerable<FileInfo> EnumerateFiles(string searchPattern, EnumerationOptions enumerationOptions);
    public IEnumerable<FileInfo> EnumerateFiles(string searchPattern, SearchOption searchOption);
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos();
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern);
+   public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern, EnumerationOptions enumerationOptions);
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern, SearchOption searchOption);
    public DirectoryInfo[] GetDirectories();
    public DirectoryInfo[] GetDirectories(string searchPattern);
+   public DirectoryInfo[] GetDirectories(string searchPattern, EnumerationOptions enumerationOptions);
    public DirectoryInfo[] GetDirectories(string searchPattern, SearchOption searchOption);
    public FileInfo[] GetFiles();
    public FileInfo[] GetFiles(string searchPattern);
+   public FileInfo[] GetFiles(string searchPattern, EnumerationOptions enumerationOptions);
    public FileInfo[] GetFiles(string searchPattern, SearchOption searchOption);
    public FileSystemInfo[] GetFileSystemInfos();
    public FileSystemInfo[] GetFileSystemInfos(string searchPattern);
+   public FileSystemInfo[] GetFileSystemInfos(string searchPattern, EnumerationOptions enumerationOptions);
    public FileSystemInfo[] GetFileSystemInfos(string searchPattern, SearchOption searchOption);
    public void MoveTo(string destDirName);
    public override string ToString();
  }
  public static class File {
    public static void AppendAllLines(string path, IEnumerable<string> contents);
    public static void AppendAllLines(string path, IEnumerable<string> contents, Encoding encoding);
    public static Task AppendAllLinesAsync(string path, IEnumerable<string> contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task AppendAllLinesAsync(string path, IEnumerable<string> contents, CancellationToken cancellationToken=default(CancellationToken));
    public static void AppendAllText(string path, string contents);
    public static void AppendAllText(string path, string contents, Encoding encoding);
    public static Task AppendAllTextAsync(string path, string contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task AppendAllTextAsync(string path, string contents, CancellationToken cancellationToken=default(CancellationToken));
    public static StreamWriter AppendText(string path);
    public static void Copy(string sourceFileName, string destFileName);
    public static void Copy(string sourceFileName, string destFileName, bool overwrite);
    public static FileStream Create(string path);
    public static FileStream Create(string path, int bufferSize);
    public static FileStream Create(string path, int bufferSize, FileOptions options);
    public static StreamWriter CreateText(string path);
    public static void Decrypt(string path);
    public static void Delete(string path);
    public static void Encrypt(string path);
    public static bool Exists(string path);
    public static FileAttributes GetAttributes(string path);
    public static DateTime GetCreationTime(string path);
    public static DateTime GetCreationTimeUtc(string path);
    public static DateTime GetLastAccessTime(string path);
    public static DateTime GetLastAccessTimeUtc(string path);
    public static DateTime GetLastWriteTime(string path);
    public static DateTime GetLastWriteTimeUtc(string path);
    public static void Move(string sourceFileName, string destFileName);
    public static FileStream Open(string path, FileMode mode);
    public static FileStream Open(string path, FileMode mode, FileAccess access);
    public static FileStream Open(string path, FileMode mode, FileAccess access, FileShare share);
    public static FileStream OpenRead(string path);
    public static StreamReader OpenText(string path);
    public static FileStream OpenWrite(string path);
    public static byte[] ReadAllBytes(string path);
    public static Task<byte[]> ReadAllBytesAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static string[] ReadAllLines(string path);
    public static string[] ReadAllLines(string path, Encoding encoding);
    public static Task<string[]> ReadAllLinesAsync(string path, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task<string[]> ReadAllLinesAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static string ReadAllText(string path);
    public static string ReadAllText(string path, Encoding encoding);
    public static Task<string> ReadAllTextAsync(string path, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task<string> ReadAllTextAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static IEnumerable<string> ReadLines(string path);
    public static IEnumerable<string> ReadLines(string path, Encoding encoding);
    public static void Replace(string sourceFileName, string destinationFileName, string destinationBackupFileName);
    public static void Replace(string sourceFileName, string destinationFileName, string destinationBackupFileName, bool ignoreMetadataErrors);
    public static void SetAttributes(string path, FileAttributes fileAttributes);
    public static void SetCreationTime(string path, DateTime creationTime);
    public static void SetCreationTimeUtc(string path, DateTime creationTimeUtc);
    public static void SetLastAccessTime(string path, DateTime lastAccessTime);
    public static void SetLastAccessTimeUtc(string path, DateTime lastAccessTimeUtc);
    public static void SetLastWriteTime(string path, DateTime lastWriteTime);
    public static void SetLastWriteTimeUtc(string path, DateTime lastWriteTimeUtc);
    public static void WriteAllBytes(string path, byte[] bytes);
    public static Task WriteAllBytesAsync(string path, byte[] bytes, CancellationToken cancellationToken=default(CancellationToken));
    public static void WriteAllLines(string path, IEnumerable<string> contents);
    public static void WriteAllLines(string path, IEnumerable<string> contents, Encoding encoding);
    public static void WriteAllLines(string path, string[] contents);
    public static void WriteAllLines(string path, string[] contents, Encoding encoding);
    public static Task WriteAllLinesAsync(string path, IEnumerable<string> contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task WriteAllLinesAsync(string path, IEnumerable<string> contents, CancellationToken cancellationToken=default(CancellationToken));
    public static void WriteAllText(string path, string contents);
    public static void WriteAllText(string path, string contents, Encoding encoding);
    public static Task WriteAllTextAsync(string path, string contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
    public static Task WriteAllTextAsync(string path, string contents, CancellationToken cancellationToken=default(CancellationToken));
  }
  public sealed class FileInfo : FileSystemInfo {
    public FileInfo(string fileName);
    public DirectoryInfo Directory { get; }
    public string DirectoryName { get; }
    public override bool Exists { get; }
    public bool IsReadOnly { get; set; }
    public long Length { get; }
    public override string Name { get; }
    public StreamWriter AppendText();
    public FileInfo CopyTo(string destFileName);
    public FileInfo CopyTo(string destFileName, bool overwrite);
    public FileStream Create();
    public StreamWriter CreateText();
    public void Decrypt();
    public override void Delete();
    public void Encrypt();
    public void MoveTo(string destFileName);
    public FileStream Open(FileMode mode);
    public FileStream Open(FileMode mode, FileAccess access);
    public FileStream Open(FileMode mode, FileAccess access, FileShare share);
    public FileStream OpenRead();
    public StreamReader OpenText();
    public FileStream OpenWrite();
    public FileInfo Replace(string destinationFileName, string destinationBackupFileName);
    public FileInfo Replace(string destinationFileName, string destinationBackupFileName, bool ignoreMetadataErrors);
    public override string ToString();
  }
  public abstract class FileSystemInfo : MarshalByRefObject, ISerializable {
    protected string FullPath;
    protected string OriginalPath;
    protected FileSystemInfo();
    protected FileSystemInfo(SerializationInfo info, StreamingContext context);
    public FileAttributes Attributes { get; set; }
    public DateTime CreationTime { get; set; }
    public DateTime CreationTimeUtc { get; set; }
    public abstract bool Exists { get; }
    public string Extension { get; }
    public virtual string FullName { get; }
    public DateTime LastAccessTime { get; set; }
    public DateTime LastAccessTimeUtc { get; set; }
    public DateTime LastWriteTime { get; set; }
    public DateTime LastWriteTimeUtc { get; set; }
    public abstract string Name { get; }
    public abstract void Delete();
    public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public void Refresh();
  }
  public enum SearchOption {
    AllDirectories = 1,
    TopDirectoryOnly = 0,
  }
 }
+namespace System.IO.Enumeration {
+ public struct EnumerationOptions {
+   public FileAttributes AttributesToSkip { get; set; }
+   public int BufferSize { get; set; }
+   public static EnumerationOptions Default { get; }
+   public bool IgnoreInaccessible { get; set; }
+   public MatchType MatchType { get; set; }
+   public bool Recurse { get; set; }
  }
+ public ref struct FileSystemEntry {
+   public FileAttributes Attributes { get; }
+   public DateTimeOffset CreationTimeUtc { get; }
+   public ReadOnlySpan<char> Directory { get; }
+   public ReadOnlySpan<char> FileName { get; }
+   public bool IsDirectory { get; }
+   public bool IsNameDotOrDotDot { get; }
+   public DateTimeOffset LastAccessTimeUtc { get; }
+   public DateTimeOffset LastWriteTimeUtc { get; }
+   public long Length { get; }
+   public string OriginalRootDirectory { get; }
+   public string RootDirectory { get; }
+   public DirectoryInfo ToDirectoryInfo();
+   public FileInfo ToFileInfo();
+   public FileSystemInfo ToFileSystemInfo();
+   public string ToFullPath();
+   public string ToSpecifiedFullPath();
  }
+ public class FileSystemEnumerable<TResult, TState> : IEnumerable, IEnumerable<TResult> {
+   public FileSystemEnumerable(string directory, FileSystemEnumerable<TResult, TState>.FindTransform transform, FileSystemEnumerable<TResult, TState>.FindPredicate shouldIncludePredicate=null);
+   public FileSystemEnumerable(string directory, FileSystemEnumerable<TResult, TState>.FindTransform transform, FileSystemEnumerable<TResult, TState>.FindPredicate shouldIncludePredicate, EnumerationOptions options);
+   public FileSystemEnumerable<TResult, TState>.FindPredicate ShouldRecursePredicate { get; set; }
+   public TState State { get; set; }
+   public IEnumerator<TResult> GetEnumerator();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
+   public delegate bool FindPredicate(ref FileSystemEntry entry, TState state);
+   public delegate TResult FindTransform(ref FileSystemEntry entry, TState state);
  }
+ public abstract class FileSystemEnumerator<TResult> : CriticalFinalizerObject, IDisposable, IEnumerator, IEnumerator<TResult> {
+   public FileSystemEnumerator(string directory);
+   public FileSystemEnumerator(string directory, EnumerationOptions options);
+   public TResult Current { get; }
+   object System.Collections.IEnumerator.Current { get; }
+   protected virtual bool ContinueOnError(int error);
+   public void Dispose();
+   public bool MoveNext();
+   protected virtual void OnDirectoryFinished(ReadOnlySpan<char> directory);
+   public void Reset();
+   protected virtual bool ShouldIncludeEntry(ref FileSystemEntry entry);
+   protected virtual bool ShouldRecurseIntoEntry(ref FileSystemEntry entry);
+   protected abstract TResult TransformEntry(ref FileSystemEntry entry);
  }
+ public static class FileSystemName {
+   public static bool MatchesDosExpression(string expression, ReadOnlySpan<char> name, bool ignoreCase=true);
+   public static bool MatchesSimpleExpression(string expression, ReadOnlySpan<char> name, bool ignoreCase=true);
+   public static string TranslateDosExpression(string expression);
  }
+ public enum MatchType {
+   Dos = 1,
+   Simple = 0,
  }
 }
```

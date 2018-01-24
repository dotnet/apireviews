# System.IO.FileSystem

```diff
---(shipped) system.io.filesystem\4.3.0\ref\netstandard1.3\System.IO.FileSystem.dll
+++(in progress) System.IO.FileSystem.dll
 namespace System.IO {
  ^ Immo Landwerth: Does this feature have to ship OOB for .NET Framework customers?
  public static class Directory {
    public static DirectoryInfo CreateDirectory(string path);
    public static void Delete(string path);
    public static void Delete(string path, bool recursive);
    public static IEnumerable<string> EnumerateDirectories(string path);
    public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern, FindOptions findOptions);
    public static IEnumerable<string> EnumerateDirectories(string path, string searchPattern, SearchOption searchOption);
    public static IEnumerable<string> EnumerateFiles(string path);
    public static IEnumerable<string> EnumerateFiles(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateFiles(string path, string searchPattern, FindOptions findOptions);
    public static IEnumerable<string> EnumerateFiles(string path, string searchPattern, SearchOption searchOption);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern);
+   public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern, FindOptions findOptions);
    public static IEnumerable<string> EnumerateFileSystemEntries(string path, string searchPattern, SearchOption searchOption);
    public static bool Exists(string path);
    public static DateTime GetCreationTime(string path);
    public static DateTime GetCreationTimeUtc(string path);
    public static string GetCurrentDirectory();
    public static string[] GetDirectories(string path);
    public static string[] GetDirectories(string path, string searchPattern);
+   public static string[] GetDirectories(string path, string searchPattern, FindOptions findOptions);
    public static string[] GetDirectories(string path, string searchPattern, SearchOption searchOption);
    public static string GetDirectoryRoot(string path);
    public static string[] GetFiles(string path);
    public static string[] GetFiles(string path, string searchPattern);
+   public static string[] GetFiles(string path, string searchPattern, FindOptions findOptions);
    public static string[] GetFiles(string path, string searchPattern, SearchOption searchOption);
    public static string[] GetFileSystemEntries(string path);
    public static string[] GetFileSystemEntries(string path, string searchPattern);
+   public static string[] GetFileSystemEntries(string path, string searchPattern, FindOptions findOptions);
    public static string[] GetFileSystemEntries(string path, string searchPattern, SearchOption searchOption);
    public static DateTime GetLastAccessTime(string path);
    public static DateTime GetLastAccessTimeUtc(string path);
    public static DateTime GetLastWriteTime(string path);
    public static DateTime GetLastWriteTimeUtc(string path);
+   public static string[] GetLogicalDrives();
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
+   public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern, FindOptions findOptions);
    public IEnumerable<DirectoryInfo> EnumerateDirectories(string searchPattern, SearchOption searchOption);
    public IEnumerable<FileInfo> EnumerateFiles();
    public IEnumerable<FileInfo> EnumerateFiles(string searchPattern);
+   public IEnumerable<FileInfo> EnumerateFiles(string searchPattern, FindOptions findOptions);
    public IEnumerable<FileInfo> EnumerateFiles(string searchPattern, SearchOption searchOption);
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos();
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern);
+   public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern, FindOptions findOptions);
    public IEnumerable<FileSystemInfo> EnumerateFileSystemInfos(string searchPattern, SearchOption searchOption);
    public DirectoryInfo[] GetDirectories();
    public DirectoryInfo[] GetDirectories(string searchPattern);
+   public DirectoryInfo[] GetDirectories(string searchPattern, FindOptions findOptions);
    public DirectoryInfo[] GetDirectories(string searchPattern, SearchOption searchOption);
    public FileInfo[] GetFiles();
    public FileInfo[] GetFiles(string searchPattern);
+   public FileInfo[] GetFiles(string searchPattern, FindOptions findOptions);
    public FileInfo[] GetFiles(string searchPattern, SearchOption searchOption);
    public FileSystemInfo[] GetFileSystemInfos();
    public FileSystemInfo[] GetFileSystemInfos(string searchPattern);
+   public FileSystemInfo[] GetFileSystemInfos(string searchPattern, FindOptions findOptions);
    public FileSystemInfo[] GetFileSystemInfos(string searchPattern, SearchOption searchOption);
    public void MoveTo(string destDirName);
    public override string ToString();
  }
+ public static class DosMatcher {
    ^ Immo Landwerth: Can we merge this with SimpleMatcher and call it PathMatcher?
+   public static bool MatchPattern(string expression, ReadOnlySpan<char> name, bool ignoreCase=true);
+   public static string TranslateExpression(string expression);
  }
  public static class File {
    public static void AppendAllLines(string path, IEnumerable<string> contents);
    public static void AppendAllLines(string path, IEnumerable<string> contents, Encoding encoding);
+   public static Task AppendAllLinesAsync(string path, IEnumerable<string> contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task AppendAllLinesAsync(string path, IEnumerable<string> contents, CancellationToken cancellationToken=default(CancellationToken));
    public static void AppendAllText(string path, string contents);
    public static void AppendAllText(string path, string contents, Encoding encoding);
+   public static Task AppendAllTextAsync(string path, string contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task AppendAllTextAsync(string path, string contents, CancellationToken cancellationToken=default(CancellationToken));
    public static StreamWriter AppendText(string path);
    public static void Copy(string sourceFileName, string destFileName);
    public static void Copy(string sourceFileName, string destFileName, bool overwrite);
    public static FileStream Create(string path);
    public static FileStream Create(string path, int bufferSize);
    public static FileStream Create(string path, int bufferSize, FileOptions options);
    public static StreamWriter CreateText(string path);
+   public static void Decrypt(string path);
    public static void Delete(string path);
+   public static void Encrypt(string path);
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
+   public static Task<byte[]> ReadAllBytesAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static string[] ReadAllLines(string path);
    public static string[] ReadAllLines(string path, Encoding encoding);
+   public static Task<string[]> ReadAllLinesAsync(string path, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task<string[]> ReadAllLinesAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static string ReadAllText(string path);
    public static string ReadAllText(string path, Encoding encoding);
+   public static Task<string> ReadAllTextAsync(string path, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task<string> ReadAllTextAsync(string path, CancellationToken cancellationToken=default(CancellationToken));
    public static IEnumerable<string> ReadLines(string path);
    public static IEnumerable<string> ReadLines(string path, Encoding encoding);
+   public static void Replace(string sourceFileName, string destinationFileName, string destinationBackupFileName);
+   public static void Replace(string sourceFileName, string destinationFileName, string destinationBackupFileName, bool ignoreMetadataErrors);
    public static void SetAttributes(string path, FileAttributes fileAttributes);
    public static void SetCreationTime(string path, DateTime creationTime);
    public static void SetCreationTimeUtc(string path, DateTime creationTimeUtc);
    public static void SetLastAccessTime(string path, DateTime lastAccessTime);
    public static void SetLastAccessTimeUtc(string path, DateTime lastAccessTimeUtc);
    public static void SetLastWriteTime(string path, DateTime lastWriteTime);
    public static void SetLastWriteTimeUtc(string path, DateTime lastWriteTimeUtc);
    public static void WriteAllBytes(string path, byte[] bytes);
+   public static Task WriteAllBytesAsync(string path, byte[] bytes, CancellationToken cancellationToken=default(CancellationToken));
    public static void WriteAllLines(string path, IEnumerable<string> contents);
    public static void WriteAllLines(string path, IEnumerable<string> contents, Encoding encoding);
+   public static void WriteAllLines(string path, string[] contents);
+   public static void WriteAllLines(string path, string[] contents, Encoding encoding);
+   public static Task WriteAllLinesAsync(string path, IEnumerable<string> contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task WriteAllLinesAsync(string path, IEnumerable<string> contents, CancellationToken cancellationToken=default(CancellationToken));
    public static void WriteAllText(string path, string contents);
    public static void WriteAllText(string path, string contents, Encoding encoding);
+   public static Task WriteAllTextAsync(string path, string contents, Encoding encoding, CancellationToken cancellationToken=default(CancellationToken));
+   public static Task WriteAllTextAsync(string path, string contents, CancellationToken cancellationToken=default(CancellationToken));
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
+   public void Decrypt();
    public override void Delete();
+   public void Encrypt();
    public void MoveTo(string destFileName);
    public FileStream Open(FileMode mode);
    public FileStream Open(FileMode mode, FileAccess access);
    public FileStream Open(FileMode mode, FileAccess access, FileShare share);
    public FileStream OpenRead();
    public StreamReader OpenText();
    public FileStream OpenWrite();
+   public FileInfo Replace(string destinationFileName, string destinationBackupFileName);
+   public FileInfo Replace(string destinationFileName, string destinationBackupFileName, bool ignoreMetadataErrors);
    public override string ToString();
  }
-   public abstract class FileSystemInfo : MarshalByRefObject, ISerializable {
    protected string FullPath;
    protected string OriginalPath;
    protected FileSystemInfo();
+   protected FileSystemInfo(SerializationInfo info, StreamingContext context);
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
+   public virtual void GetObjectData(SerializationInfo info, StreamingContext context);
    public void Refresh();
  }
+ public struct FindData<TState> {
    ^ Immo Landwerth: Does the state have to live on the data? It seems more 
    | Immo Landwerth: logical to change the delegate and accept the state as a 
    | Immo Landwerth: separate argument. This also avoids that users of the 
    | Immo Landwerth: struct have to change when they want to change the state 
    | Immo Landwerth: when they recurse (and the struct is passed by ref).
    ^ Immo Landwerth: Should these types go into a separate namespace? There 
    | Immo Landwerth: more advanced. For example, System.IO.Enumeration.
    ^ Immo Landwerth: That name is super generic. Can we make this more specific 
    | Immo Landwerth: to file system, such as FileSystemEntry<T>?
+   public FileAttributes Attributes { get; }
+   public DateTime CreationTimeUtc { get; }
    ^ Immo Landwerth: Should this be DateTimeOffset
+   public string Directory { get; }
    ^ Immo Landwerth: Should this be ReadOnlySpan<char>?
+   public ReadOnlySpan<char> FileName { get; }
+   public DateTime LastAccessTimeUtc { get; }
+   public DateTime LastWriteTimeUtc { get; }
+   public long Length { get; }
+   public string OriginalDirectory { get; }
    ^ Immo Landwerth: OriginalFullPath
+   public string OriginalUserDirectory { get; }
    ^ Immo Landwerth: OriginalPath
+   public TState State { get; }
  }
+ public class FindEnumerable<TResult, TState> : CriticalFinalizerObject, IDisposable, IEnumerable, IEnumerable<TResult>, IEnumerator, IEnumerator<TResult> {
    ^ Immo Landwerth: We need a way to tell when a directory is complete
+   public FindEnumerable(string directory, FindTransform<TResult, TState> transform, FindPredicate<TState> predicate, FindPredicate<TState> recursePredicate=null, TState state=null, FindOptions options=(FindOptions)(0));
+   public TResult Current { get; }
+   object System.Collections.IEnumerator.Current { get; }
+   public void Dispose();
+   public IEnumerator<TResult> GetEnumerator();
+   public bool MoveNext();
+   public void Reset();
+   IEnumerator System.Collections.IEnumerable.GetEnumerator();
  }
+ public enum FindOptions {
    ^ Immo Landwerth: Should this be a struct? This way, it's easier to extend to
    | Immo Landwerth: non-boolean options, such as buffer sizes.
+   IgnoreInaccessable = 2,
    ^ Immo Landwerth: Typo
+   None = 0,
+   Recurse = 1,
+   UseLargeBuffer = 4,
    ^ Immo Landwerth: This almost cries for the struct as it implies a hard wired policy.
  }
+ public delegate bool FindPredicate<TState>(ref FindData<TState> findData);
+ public static class FindPredicates {
    ^ Immo Landwerth: Can those be instance methods?
+   public static bool IsDirectory<TState>(ref FindData<TState> findData);
+   public static bool NotDotOrDotDot<TState>(ref FindData<TState> findData);
    ^ Immo Landwerth: Consider whether there is a plausible scenario for wanting
    | Immo Landwerth: DotOrDotDot since it is everywhere
  }
+ public delegate TResult FindTransform<TResult, TState>(ref FindData<TState> findData);
+ public static class FindTransforms {
    ^ Immo Landwerth: Can those be instance methods?
+   public static DirectoryInfo AsDirectoryInfo<TState>(ref FindData<TState> findData);
+   public static FileInfo AsFileInfo<TState>(ref FindData<TState> findData);
+   public static FileSystemInfo AsFileSystemInfo<TState>(ref FindData<TState> findData);
+   public static string AsUserFullPath<TState>(ref FindData<TState> findData);
  }
  public enum SearchOption {
    AllDirectories = 1,
    TopDirectoryOnly = 0,
  }
+ public static class SimpleMatcher {
+   public static bool MatchPattern(string expression, ReadOnlySpan<char> name, bool ignoreCase=true);
    ^ Immo Landwerth: Sync with Tarek about casing of filesystem entries
  }
 }
```

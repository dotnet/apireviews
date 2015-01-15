```diff
---C:\git\corefx\bin\Debug\System.Text.RegularExpressions\System.Text.RegularExpressions.dll
+++C:\git\corefx\bin-new\Debug\System.Text.RegularExpressions\System.Text.RegularExpressions.dll
 namespace System.Text.RegularExpressions {
  public class CaptureCollection : ICollection, ICollection<Capture>, IEnumerable, IEnumerable<Capture>, IList, IList<Capture>, IReadOnlyCollection<Capture>, IReadOnlyList<Capture> {
    ^ Immo Landwerth: Throw same exceptions that ReadOnlyCollection<T> throws when calling mutating APIs
    ^ Immo Landwerth: The non-mutating, strongly typed APIs should be publicly implemented.
    ^ Immo Landwerth: They are serializable on desktop, which means that we have to make sure that serailization keeps working.
    | Immo Landwerth:
    | Immo Landwerth: --> If it doesn't, we're OK with removing IList
    public int Count { get; }
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.IsReadOnly { get; }
+   Capture System.Collections.Generic.IList<System.Text.RegularExpressions.Capture>.this[int index] { get; set; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
+   bool System.Collections.IList.IsFixedSize { get; }
+   bool System.Collections.IList.IsReadOnly { get; }
+   object System.Collections.IList.this[int index] { get; set; }
    public Capture this[int i] { get; }
    public IEnumerator GetEnumerator();
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.Add(Capture item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.Clear();
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.Contains(Capture item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.CopyTo(Capture[] array, int arrayIndex);
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Capture>.Remove(Capture item);
+   IEnumerator<Capture> System.Collections.Generic.IEnumerable<System.Text.RegularExpressions.Capture>.GetEnumerator();
+   int System.Collections.Generic.IList<System.Text.RegularExpressions.Capture>.IndexOf(Capture item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Capture>.Insert(int index, Capture item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Capture>.RemoveAt(int index);
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndex);
+   int System.Collections.IList.Add(object value);
+   void System.Collections.IList.Clear();
+   bool System.Collections.IList.Contains(object value);
+   int System.Collections.IList.IndexOf(object value);
+   void System.Collections.IList.Insert(int index, object value);
+   void System.Collections.IList.Remove(object value);
+   void System.Collections.IList.RemoveAt(int index);
  }
  public class GroupCollection : ICollection, ICollection<Group>, IEnumerable, IEnumerable<Group>, IList, IList<Group>, IReadOnlyCollection<Group>, IReadOnlyList<Group> {
    ^ Immo Landwerth: Same comments for CaptureCollection
    public int Count { get; }
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.IsReadOnly { get; }
+   Group System.Collections.Generic.IList<System.Text.RegularExpressions.Group>.this[int index] { get; set; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
+   bool System.Collections.IList.IsFixedSize { get; }
+   bool System.Collections.IList.IsReadOnly { get; }
+   object System.Collections.IList.this[int index] { get; set; }
    public Group this[int groupnum] { get; }
    public Group this[string groupname] { get; }
    public IEnumerator GetEnumerator();
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.Add(Group item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.Clear();
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.Contains(Group item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.CopyTo(Group[] array, int arrayIndex);
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Group>.Remove(Group item);
+   IEnumerator<Group> System.Collections.Generic.IEnumerable<System.Text.RegularExpressions.Group>.GetEnumerator();
+   int System.Collections.Generic.IList<System.Text.RegularExpressions.Group>.IndexOf(Group item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Group>.Insert(int index, Group item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Group>.RemoveAt(int index);
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndex);
+   int System.Collections.IList.Add(object value);
+   void System.Collections.IList.Clear();
+   bool System.Collections.IList.Contains(object value);
+   int System.Collections.IList.IndexOf(object value);
+   void System.Collections.IList.Insert(int index, object value);
+   void System.Collections.IList.Remove(object value);
+   void System.Collections.IList.RemoveAt(int index);
  }
  public class MatchCollection : ICollection, ICollection<Match>, IEnumerable, IEnumerable<Match>, IList, IList<Match>, IReadOnlyCollection<Match>, IReadOnlyList<Match> {
    ^ Immo Landwerth: Same comments for CaptureCollection
    public int Count { get; }
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.IsReadOnly { get; }
+   Match System.Collections.Generic.IList<System.Text.RegularExpressions.Match>.this[int index] { get; set; }
    bool System.Collections.ICollection.IsSynchronized { get; }
    object System.Collections.ICollection.SyncRoot { get; }
+   bool System.Collections.IList.IsFixedSize { get; }
+   bool System.Collections.IList.IsReadOnly { get; }
+   object System.Collections.IList.this[int index] { get; set; }
    public virtual Match this[int i] { get; }
    public IEnumerator GetEnumerator();
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.Add(Match item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.Clear();
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.Contains(Match item);
+   void System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.CopyTo(Match[] array, int arrayIndex);
+   bool System.Collections.Generic.ICollection<System.Text.RegularExpressions.Match>.Remove(Match item);
+   IEnumerator<Match> System.Collections.Generic.IEnumerable<System.Text.RegularExpressions.Match>.GetEnumerator();
+   int System.Collections.Generic.IList<System.Text.RegularExpressions.Match>.IndexOf(Match item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Match>.Insert(int index, Match item);
+   void System.Collections.Generic.IList<System.Text.RegularExpressions.Match>.RemoveAt(int index);
    void System.Collections.ICollection.CopyTo(Array array, int arrayIndex);
+   int System.Collections.IList.Add(object value);
+   void System.Collections.IList.Clear();
+   bool System.Collections.IList.Contains(object value);
+   int System.Collections.IList.IndexOf(object value);
+   void System.Collections.IList.Insert(int index, object value);
+   void System.Collections.IList.Remove(object value);
+   void System.Collections.IList.RemoveAt(int index);
  }
 }

```

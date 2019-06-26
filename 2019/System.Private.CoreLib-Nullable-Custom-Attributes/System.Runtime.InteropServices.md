# System.Runtime.InteropServices

```diff
--- D:\Temp\nullable\before\System.Runtime.InteropServices.dll.cs	2019-06-26 13:40:21.000000000 -0700
+++ D:\Temp\nullable\after\System.Runtime.InteropServices.dll.cs	2019-06-26 13:40:41.000000000 -0700
@@ -515,13 +515,13 @@
         public static void Copy(float[] source, int startIndex, System.IntPtr destination, int length) { }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static System.IntPtr CreateAggregatedObject(System.IntPtr pOuter, object o) { throw null; }
         public static System.IntPtr CreateAggregatedObject<T>(System.IntPtr pOuter, T o) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static object CreateWrapperOfType(object o, System.Type t) { throw null; }
-        public static TWrapper CreateWrapperOfType<T, TWrapper>(T o) { throw null; }
+        public static TWrapper CreateWrapperOfType<T, TWrapper>([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T o) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static void DestroyStructure(System.IntPtr ptr, System.Type structuretype) { }
         public static void DestroyStructure<T>(System.IntPtr ptr) { }
         public static int FinalReleaseComObject(object o) { throw null; }
         public static void FreeBSTR(System.IntPtr ptr) { }
         public static void FreeCoTaskMem(System.IntPtr ptr) { }
@@ -529,13 +529,13 @@
         public static System.Guid GenerateGuidForType(System.Type type) { throw null; }
         public static string GenerateProgIdForType(System.Type type) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static System.IntPtr GetComInterfaceForObject(object o, System.Type T) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static System.IntPtr GetComInterfaceForObject(object o, System.Type T, System.Runtime.InteropServices.CustomQueryInterfaceMode mode) { throw null; }
-        public static System.IntPtr GetComInterfaceForObject<T, TInterface>(T o) { throw null; }
+        public static System.IntPtr GetComInterfaceForObject<T, TInterface>([System.Diagnostics.CodeAnalysis.DisallowNullAttribute]T o) { throw null; }
         public static object GetComObjectData(object obj, object key) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static System.Delegate GetDelegateForFunctionPointer(System.IntPtr ptr, System.Type t) { throw null; }
         public static TDelegate GetDelegateForFunctionPointer<TDelegate>(System.IntPtr ptr) { throw null; }
         public static int GetEndComSlot(System.Type t) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
@@ -553,18 +553,18 @@
         public static System.IntPtr GetIDispatchForObject(object o) { throw null; }
         public static System.IntPtr GetIUnknownForObject(object o) { throw null; }
         public static int GetLastWin32Error() { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static void GetNativeVariantForObject(object obj, System.IntPtr pDstNativeVariant) { }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
-        public static void GetNativeVariantForObject<T>(T obj, System.IntPtr pDstNativeVariant) { }
+        public static void GetNativeVariantForObject<T>([System.Diagnostics.CodeAnalysis.AllowNullAttribute]T obj, System.IntPtr pDstNativeVariant) { }
         public static object GetObjectForIUnknown(System.IntPtr pUnk) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static object GetObjectForNativeVariant(System.IntPtr pSrcNativeVariant) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
-        public static T GetObjectForNativeVariant<T>(System.IntPtr pSrcNativeVariant) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T GetObjectForNativeVariant<T>(System.IntPtr pSrcNativeVariant) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static object[] GetObjectsForNativeVariants(System.IntPtr aSrcNativeVariant, int cVars) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static T[] GetObjectsForNativeVariants<T>(System.IntPtr aSrcNativeVariant, int cVars) { throw null; }
         public static int GetStartComSlot(System.Type t) { throw null; }
         public static object GetTypedObjectForIUnknown(System.IntPtr pUnk, System.Type t) { throw null; }
@@ -588,14 +588,14 @@
         public static string PtrToStringUTF8(System.IntPtr ptr) { throw null; }
         public static string PtrToStringUTF8(System.IntPtr ptr, int byteLen) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static void PtrToStructure(System.IntPtr ptr, object structure) { }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static object PtrToStructure(System.IntPtr ptr, System.Type structureType) { throw null; }
-        public static T PtrToStructure<T>(System.IntPtr ptr) { throw null; }
-        public static void PtrToStructure<T>(System.IntPtr ptr, T structure) { }
+        public static [System.Diagnostics.CodeAnalysis.MaybeNullAttribute]T PtrToStructure<T>(System.IntPtr ptr) { throw null; }
+        public static void PtrToStructure<T>(System.IntPtr ptr, [System.Diagnostics.CodeAnalysis.DisallowNullAttribute]T structure) { }
         public static int QueryInterface(System.IntPtr pUnk, ref System.Guid iid, out System.IntPtr ppv) { throw null; }
         public static byte ReadByte(System.IntPtr ptr) { throw null; }
         public static byte ReadByte(System.IntPtr ptr, int ofs) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         [System.ObsoleteAttribute("ReadByte(Object, Int32) may be unavailable in future releases.")]
         public static byte ReadByte(object ptr, int ofs) { throw null; }
@@ -647,13 +647,13 @@
         public static System.IntPtr StringToCoTaskMemUTF8(string s) { throw null; }
         public static System.IntPtr StringToHGlobalAnsi(string s) { throw null; }
         public static System.IntPtr StringToHGlobalAuto(string s) { throw null; }
         public static System.IntPtr StringToHGlobalUni(string s) { throw null; }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static void StructureToPtr(object structure, System.IntPtr ptr, bool fDeleteOld) { }
-        public static void StructureToPtr<T>(T structure, System.IntPtr ptr, bool fDeleteOld) { }
+        public static void StructureToPtr<T>([System.Diagnostics.CodeAnalysis.DisallowNullAttribute]T structure, System.IntPtr ptr, bool fDeleteOld) { }
         public static void ThrowExceptionForHR(int errorCode) { }
         public static void ThrowExceptionForHR(int errorCode, System.IntPtr errorInfo) { }
         [System.ComponentModel.EditorBrowsableAttribute(1)]
         public static System.IntPtr UnsafeAddrOfPinnedArrayElement(System.Array arr, int index) { throw null; }
         public static System.IntPtr UnsafeAddrOfPinnedArrayElement<T>(T[] arr, int index) { throw null; }
         public static void WriteByte(System.IntPtr ptr, byte val) { }
```

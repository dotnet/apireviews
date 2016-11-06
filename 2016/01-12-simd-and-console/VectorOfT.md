```diff
 namespace System.Numerics {
     public struct Vector<T>
     {
+        public Vector(void* dataPointer);
+        public void CopyTo(void* destination);
     }
 }
```

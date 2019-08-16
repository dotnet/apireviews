# System.Threading.Thread

```diff
--- D:\Temp\nullable\before\System.Threading.Thread.dll.cs	2019-06-26 13:40:27.000000000 -0700
+++ D:\Temp\nullable\after\System.Threading.Thread.dll.cs	2019-06-26 13:40:47.000000000 -0700
@@ -87,13 +87,13 @@
         public static byte VolatileRead(ref byte address) { throw null; }
         public static double VolatileRead(ref double address) { throw null; }
         public static short VolatileRead(ref short address) { throw null; }
         public static int VolatileRead(ref int address) { throw null; }
         public static long VolatileRead(ref long address) { throw null; }
         public static System.IntPtr VolatileRead(ref System.IntPtr address) { throw null; }
-        public static object VolatileRead(ref object address) { throw null; }
+        public static [System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("address")]object VolatileRead(ref object address) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static sbyte VolatileRead(ref sbyte address) { throw null; }
         public static float VolatileRead(ref float address) { throw null; }
         [System.CLSCompliantAttribute(false)]
         public static ushort VolatileRead(ref ushort address) { throw null; }
         [System.CLSCompliantAttribute(false)]
@@ -105,13 +105,13 @@
         public static void VolatileWrite(ref byte address, byte value) { }
         public static void VolatileWrite(ref double address, double value) { }
         public static void VolatileWrite(ref short address, short value) { }
         public static void VolatileWrite(ref int address, int value) { }
         public static void VolatileWrite(ref long address, long value) { }
         public static void VolatileWrite(ref System.IntPtr address, System.IntPtr value) { }
-        public static void VolatileWrite(ref object address, object value) { }
+        public static void VolatileWrite([System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute("value")]ref object address, object value) { }
         [System.CLSCompliantAttribute(false)]
         public static void VolatileWrite(ref sbyte address, sbyte value) { }
         public static void VolatileWrite(ref float address, float value) { }
         [System.CLSCompliantAttribute(false)]
         public static void VolatileWrite(ref ushort address, ushort value) { }
         [System.CLSCompliantAttribute(false)]
```

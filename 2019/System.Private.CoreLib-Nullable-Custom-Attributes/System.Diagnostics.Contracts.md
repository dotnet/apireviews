# System.Diagnostics.Contracts

```diff
--- D:\Temp\nullable\before\System.Diagnostics.Contracts.dll.cs	2019-06-26 13:40:12.000000000 -0700
+++ D:\Temp\nullable\after\System.Diagnostics.Contracts.dll.cs	2019-06-26 13:40:31.000000000 -0700
@@ -2,22 +2,22 @@
 {
     public static partial class Contract
     {
         public static event System.EventHandler<System.Diagnostics.Contracts.ContractFailedEventArgs> ContractFailed { add { } remove { } }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition) { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assert(bool condition, string userMessage) { }
+        public static void Assert([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition, string userMessage) { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assume(bool condition) { }
+        public static void Assume([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition) { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         [System.Diagnostics.ConditionalAttribute("DEBUG")]
-        public static void Assume(bool condition, string userMessage) { }
+        public static void Assume([System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute(false)]bool condition, string userMessage) { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         public static void EndContractBlock() { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         public static void Ensures(bool condition) { }
         [System.Diagnostics.ConditionalAttribute("CONTRACTS_FULL")]
         public static void Ensures(bool condition, string userMessage) { }
```

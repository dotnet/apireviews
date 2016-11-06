```diff
 namespace System {
     public static class Console {

         // Already agreed to add back:

+        public static string Title { get; set; }
+        public static void Beep();
+        public static void Clear();
+        public static bool KeyAvailable { get; }
+        public static int CursorLeft { get; set; }
+        public static int CursorTop { get; set; }
+        public static void SetCursorPosition(int left, int top);
+        public static int WindowHeight { get; set; }
+        public static int WindowWidth { get; set; }
+        public static int WindowLeft { get; set; }
+        public static int WindowTop { get; set; }

         // Discussed in this review:

+        public static int BufferHeight { get; set; }
+        public static int BufferWidth { get; set; }

+        // Linux: Returns WindowHeight
+        public static int LargestWindowHeight { get; }
+        // Linux: Returns WindowWidth
+        public static int LargestWindowWidth { get; }

+        // Linux: Getter would just return 100 and setter will throw
+        public static int CursorSize { get; set; }

+        // Linux: Would essentially add a ctrl-c handler that would add the appropriate characters to the read buffer.
+        public static bool TreatControlCAsInput { get; set; }

+        public static Stream OpenStandardError(int bufferSize);
+        public static Stream OpenStandardInput(int bufferSize);
+        public static Stream OpenStandardOutput(int bufferSize);
+        public static Encoding InputEncoding { get; set; }
+        public static Encoding OutputEncoding { get; set; }

-        // We decided not to expose these:
-        public static void Write(string format, object arg0, object arg1, object arg2, object arg3, __arglist)
-        public static void WriteLine(string format, object arg0, object arg1, object arg2, object arg3, __arglist)


        // Linux: We decided to expose these, but they will always throw.
+       public static void Beep(int frequency, int duration);
+       public static void MoveBufferArea(int sourceLeft, int sourceTop, int sourceWidth, int sourceHeight, int targetLeft, int targetTop);
+       public static void MoveBufferArea(int sourceLeft, int sourceTop, int sourceWidth, int sourceHeight, int targetLeft, int targetTop, char sourceChar, ConsoleColor sourceForeColor, ConsoleColor sourceBackColor);
+       public static void SetBufferSize(int width, int height);
+       public static void SetWindowPosition(int left, int top);
+       public static void SetWindowSize(int width, int height);
+       public static bool CapsLock { get; }
+       public static bool NumberLock { get; }
     }
 }
 ```

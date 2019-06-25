# System.Reflection.Emit.Lightweight

```diff
---D:\Temp\nullable\before
+++D:\Temp\nullable\after
 assembly System.Reflection.Emit.Lightweight {
  namespace System.Reflection.Emit {
    public sealed class DynamicILInfo {
      public DynamicMethod DynamicMethod { get; }
      public int GetTokenFor(byte[] signature);
      public int GetTokenFor(DynamicMethod method);
      public int GetTokenFor(RuntimeFieldHandle field);
      public int GetTokenFor(RuntimeFieldHandle field, RuntimeTypeHandle contextType);
      public int GetTokenFor(RuntimeMethodHandle method);
      public int GetTokenFor(RuntimeMethodHandle method, RuntimeTypeHandle contextType);
      public int GetTokenFor(RuntimeTypeHandle type);
      public int GetTokenFor(string literal);
      public unsafe void SetCode(byte* code, int codeSize, int maxStackSize);
      public void SetCode(byte[] code, int maxStackSize);
      public unsafe void SetExceptions(byte* exceptions, int exceptionsSize);
      public void SetExceptions(byte[] exceptions);
      public unsafe void SetLocalSignature(byte* localSignature, int signatureSize);
      public void SetLocalSignature(byte[] localSignature);
    }
    public sealed class DynamicMethod : MethodInfo {
      public DynamicMethod(string name, MethodAttributes attributes, CallingConventions callingConvention, Type returnType, Type[] parameterTypes, Module m, bool skipVisibility);
      public DynamicMethod(string name, MethodAttributes attributes, CallingConventions callingConvention, Type returnType, Type[] parameterTypes, Type owner, bool skipVisibility);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes, bool restrictedSkipVisibility);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes, Module m);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes, Module m, bool skipVisibility);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes, Type owner);
      public DynamicMethod(string name, Type returnType, Type[] parameterTypes, Type owner, bool skipVisibility);
      public override MethodAttributes Attributes { get; }
      public override CallingConventions CallingConvention { get; }
      public override Type DeclaringType { get; }
      public bool InitLocals { get; set; }
+     public override bool IsSecurityCritical { get; }
+     public override bool IsSecuritySafeCritical { get; }
+     public override bool IsSecurityTransparent { get; }
      public override RuntimeMethodHandle MethodHandle { get; }
+     public override Module Module { get; }
      public override string Name { get; }
      public override Type ReflectedType { get; }
      public override ParameterInfo ReturnParameter { get; }
      public override Type ReturnType { get; }
      public override ICustomAttributeProvider ReturnTypeCustomAttributes { get; }
      public sealed override Delegate CreateDelegate(Type delegateType);
      public sealed override Delegate CreateDelegate(Type delegateType, object target);
      public ParameterBuilder DefineParameter(int position, ParameterAttributes attributes, string parameterName);
      public override MethodInfo GetBaseDefinition();
      public override object[] GetCustomAttributes(bool inherit);
      public override object[] GetCustomAttributes(Type attributeType, bool inherit);
      public DynamicILInfo GetDynamicILInfo();
      public ILGenerator GetILGenerator();
      public ILGenerator GetILGenerator(int streamSize);
      public override MethodImplAttributes GetMethodImplementationFlags();
      public override ParameterInfo[] GetParameters();
      public override object Invoke(object obj, BindingFlags invokeAttr, Binder binder, object[] parameters, CultureInfo culture);
      public override bool IsDefined(Type attributeType, bool inherit);
      public override string ToString();
    }
  }
 }
```

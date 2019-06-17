# System.Reflection.Emit.ILGeneration

```diff
---D:\Temp\nullable\before\System.Reflection.Emit.ILGeneration.dll
+++D:\Temp\nullable\after\System.Reflection.Emit.ILGeneration.dll
 namespace System.Reflection.Emit {
  public class CustomAttributeBuilder {
    public CustomAttributeBuilder(ConstructorInfo con, object[] constructorArgs);
    public CustomAttributeBuilder(ConstructorInfo con, object[] constructorArgs, FieldInfo[] namedFields, object[] fieldValues);
    public CustomAttributeBuilder(ConstructorInfo con, object[] constructorArgs, PropertyInfo[] namedProperties, object[] propertyValues);
    public CustomAttributeBuilder(ConstructorInfo con, object[] constructorArgs, PropertyInfo[] namedProperties, object[] propertyValues, FieldInfo[] namedFields, object[] fieldValues);
  }
  public class ILGenerator {
    public virtual int ILOffset { get; }
    public virtual void BeginCatchBlock(Type exceptionType);
    public virtual void BeginExceptFilterBlock();
    public virtual Label BeginExceptionBlock();
    public virtual void BeginFaultBlock();
    public virtual void BeginFinallyBlock();
    public virtual void BeginScope();
    public virtual LocalBuilder DeclareLocal(Type localType);
    public virtual LocalBuilder DeclareLocal(Type localType, bool pinned);
    public virtual Label DefineLabel();
    public virtual void Emit(OpCode opcode);
    public virtual void Emit(OpCode opcode, byte arg);
    public virtual void Emit(OpCode opcode, double arg);
    public virtual void Emit(OpCode opcode, short arg);
    public virtual void Emit(OpCode opcode, int arg);
    public virtual void Emit(OpCode opcode, long arg);
    public virtual void Emit(OpCode opcode, ConstructorInfo con);
    public virtual void Emit(OpCode opcode, Label label);
    public virtual void Emit(OpCode opcode, Label[] labels);
    public virtual void Emit(OpCode opcode, LocalBuilder local);
    public virtual void Emit(OpCode opcode, SignatureHelper signature);
    public virtual void Emit(OpCode opcode, FieldInfo field);
    public virtual void Emit(OpCode opcode, MethodInfo meth);
    public void Emit(OpCode opcode, sbyte arg);
    public virtual void Emit(OpCode opcode, float arg);
    public virtual void Emit(OpCode opcode, string str);
    public virtual void Emit(OpCode opcode, Type cls);
    public virtual void EmitCall(OpCode opcode, MethodInfo methodInfo, Type[] optionalParameterTypes);
    public virtual void EmitCalli(OpCode opcode, CallingConventions callingConvention, Type returnType, Type[] parameterTypes, Type[] optionalParameterTypes);
    public virtual void EmitCalli(OpCode opcode, CallingConvention unmanagedCallConv, Type returnType, Type[] parameterTypes);
    public virtual void EmitWriteLine(LocalBuilder localBuilder);
    public virtual void EmitWriteLine(FieldInfo fld);
    public virtual void EmitWriteLine(string value);
    public virtual void EndExceptionBlock();
    public virtual void EndScope();
    public virtual void MarkLabel(Label loc);
    public virtual void ThrowException(Type excType);
    public virtual void UsingNamespace(string usingNamespace);
  }
  public struct Label : IEquatable<Label> {
    public override bool Equals(object obj);
    public bool Equals(Label obj);
    public override int GetHashCode();
    public static bool operator ==(Label a, Label b);
    public static bool operator !=(Label a, Label b);
  }
  public sealed class LocalBuilder : LocalVariableInfo {
    public override bool IsPinned { get; }
    public override int LocalIndex { get; }
    public override Type LocalType { get; }
  }
  public class ParameterBuilder {
    public virtual int Attributes { get; }
    public bool IsIn { get; }
    public bool IsOptional { get; }
    public bool IsOut { get; }
    public virtual string Name { get; }
    public virtual int Position { get; }
    public virtual void SetConstant(object defaultValue);
    public void SetCustomAttribute(ConstructorInfo con, byte[] binaryAttribute);
    public void SetCustomAttribute(CustomAttributeBuilder customBuilder);
  }
  public sealed class SignatureHelper {
    public void AddArgument(Type clsArgument);
    public void AddArgument(Type argument, bool pinned);
    public void AddArgument(Type argument, Type[] requiredCustomModifiers, Type[] optionalCustomModifiers);
    public void AddArguments(Type[] arguments, Type[][] requiredCustomModifiers, Type[][] optionalCustomModifiers);
    public void AddSentinel();
    public override bool Equals(object obj);
    public static SignatureHelper GetFieldSigHelper(Module mod);
    public override int GetHashCode();
    public static SignatureHelper GetLocalVarSigHelper();
    public static SignatureHelper GetLocalVarSigHelper(Module mod);
    public static SignatureHelper GetMethodSigHelper(CallingConventions callingConvention, Type returnType);
    public static SignatureHelper GetMethodSigHelper(Module mod, CallingConventions callingConvention, Type returnType);
    public static SignatureHelper GetMethodSigHelper(Module mod, Type returnType, Type[] parameterTypes);
    public static SignatureHelper GetPropertySigHelper(Module mod, CallingConventions callingConvention, Type returnType, Type[] requiredReturnTypeCustomModifiers, Type[] optionalReturnTypeCustomModifiers, Type[] parameterTypes, Type[][] requiredParameterTypeCustomModifiers, Type[][] optionalParameterTypeCustomModifiers);
    public static SignatureHelper GetPropertySigHelper(Module mod, Type returnType, Type[] parameterTypes);
    public static SignatureHelper GetPropertySigHelper(Module mod, Type returnType, Type[] requiredReturnTypeCustomModifiers, Type[] optionalReturnTypeCustomModifiers, Type[] parameterTypes, Type[][] requiredParameterTypeCustomModifiers, Type[][] optionalParameterTypeCustomModifiers);
    public byte[] GetSignature();
    public override string ToString();
  }
 }
```

# API Review 06/17/2025

## Expose a new Configuration API to help in Null values binding

**Approved** | [#runtime/116700](https://github.com/dotnet/runtime/issues/116700#issuecomment-2981276757) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=0h0m0s)

* There was a lot of discussion of the need, and how compatibility will be impacted by the overall scenario.
* We'll leave it non-virtual for now, and can make it virtual later.

```c#
namespace Microsoft.Extensions.Configuration;

public partial class ConfigurationSection : IConfigurationSection
{
    public bool TryGetValue(string? key, out string? value);
}
```
## Add APIs to read the `AssemblyNameInfo` of an assembly file.

**Approved** | [#runtime/113749](https://github.com/dotnet/runtime/issues/113749#issuecomment-2981313808) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=0h36m50s)

* AssemblyNameInfo.GetAssemblyNameInfo was cut because it complicates the security model of the AssemblyNameInfo type.
* MetadataReader.GetAssemblyNameInfo was cut because it is simplifying an operation that is already simple given AssemblyDefinition.GetAssemblyNameInfo()

```c#
namespace System.Reflection.Metadata;

public partial struct AssemblyDefinition
{
     public AssemblyNameInfo GetAssemblyNameInfo();
}

public partial struct AssemblyReference
{
     public AssemblyNameInfo GetAssemblyNameInfo();
}
```
## SVE: Rename arguments for GatherVector APIs

**Approved** | [#runtime/115246](https://github.com/dotnet/runtime/issues/115246#issuecomment-2981327489) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=0h50m30s)

* `base` => `address` and `bases` => `addresses` seems like a good changed.
* Looks good as proposed

```C#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract partial class Sve : AdvSimd /// Feature: FEAT_SVE 
{
        public static unsafe Vector<double> GatherVector(Vector<double> mask, double* address, Vector<long> indices) => GatherVector(mask, address, indices);
        public static Vector<double> GatherVector(Vector<double> mask, Vector<ulong> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<double> GatherVector(Vector<double> mask, double* address, Vector<ulong> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<int> GatherVector(Vector<int> mask, int* address, Vector<int> indices) => GatherVector(mask, address, indices);
        // public static Vector<int> GatherVector(Vector<int> mask, Vector<uint> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<int> GatherVector(Vector<int> mask, int* address, Vector<uint> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<long> GatherVector(Vector<long> mask, long* address, Vector<long> indices) => GatherVector(mask, address, indices);
        public static Vector<long> GatherVector(Vector<long> mask, Vector<ulong> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<long> GatherVector(Vector<long> mask, long* address, Vector<ulong> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<float> GatherVector(Vector<float> mask, float* address, Vector<int> indices) => GatherVector(mask, address, indices);
        // public static Vector<float> GatherVector(Vector<float> mask, Vector<uint> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<float> GatherVector(Vector<float> mask, float* address, Vector<uint> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<uint> GatherVector(Vector<uint> mask, uint* address, Vector<int> indices) => GatherVector(mask, address, indices);
        // public static Vector<uint> GatherVector(Vector<uint> mask, Vector<uint> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<uint> GatherVector(Vector<uint> mask, uint* address, Vector<uint> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<ulong> GatherVector(Vector<ulong> mask, ulong* address, Vector<long> indices) => GatherVector(mask, address, indices);
        public static Vector<ulong> GatherVector(Vector<ulong> mask, Vector<ulong> addresses) => GatherVector(mask, addresses);
        public static unsafe Vector<ulong> GatherVector(Vector<ulong> mask, ulong* address, Vector<ulong> indices) => GatherVector(mask, address, indices);
        public static unsafe Vector<int> GatherVectorByteZeroExtend(Vector<int> mask, byte* address, Vector<int> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        // public static Vector<int> GatherVectorByteZeroExtend(Vector<int> mask, Vector<uint> addresses) => GatherVectorByteZeroExtend(mask, addresses);
        public static unsafe Vector<int> GatherVectorByteZeroExtend(Vector<int> mask, byte* address, Vector<uint> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorByteZeroExtend(Vector<long> mask, byte* address, Vector<long> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static Vector<long> GatherVectorByteZeroExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorByteZeroExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorByteZeroExtend(Vector<long> mask, byte* address, Vector<ulong> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorByteZeroExtend(Vector<uint> mask, byte* address, Vector<int> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        // public static Vector<uint> GatherVectorByteZeroExtend(Vector<uint> mask, Vector<uint> addresses) => GatherVectorByteZeroExtend(mask, addresses);
        public static unsafe Vector<uint> GatherVectorByteZeroExtend(Vector<uint> mask, byte* address, Vector<uint> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorByteZeroExtend(Vector<ulong> mask, byte* address, Vector<long> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorByteZeroExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorByteZeroExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorByteZeroExtend(Vector<ulong> mask, byte* address, Vector<ulong> indices) => GatherVectorByteZeroExtend(mask, address, indices);
        public static unsafe Vector<int> GatherVectorByteZeroExtendFirstFaulting(Vector<int> mask, byte* address, Vector<int> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        // public static Vector<int> GatherVectorByteZeroExtendFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorByteZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorByteZeroExtendFirstFaulting(Vector<int> mask, byte* address, Vector<uint> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorByteZeroExtendFirstFaulting(Vector<long> mask, byte* address, Vector<long> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static Vector<long> GatherVectorByteZeroExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorByteZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorByteZeroExtendFirstFaulting(Vector<long> mask, byte* address, Vector<ulong> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorByteZeroExtendFirstFaulting(Vector<uint> mask, byte* address, Vector<int> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        // public static Vector<uint> GatherVectorByteZeroExtendFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorByteZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorByteZeroExtendFirstFaulting(Vector<uint> mask, byte* address, Vector<uint> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorByteZeroExtendFirstFaulting(Vector<ulong> mask, byte* address, Vector<long> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static Vector<ulong> GatherVectorByteZeroExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorByteZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorByteZeroExtendFirstFaulting(Vector<ulong> mask, byte* address, Vector<ulong> offsets) => GatherVectorByteZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<double> GatherVectorFirstFaulting(Vector<double> mask, double* address, Vector<long> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static Vector<double> GatherVectorFirstFaulting(Vector<double> mask, Vector<ulong> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<double> GatherVectorFirstFaulting(Vector<double> mask, double* address, Vector<ulong> indices) => GatherVectorFirstFaulting(mask, address, indices);
        // public static Vector<int> GatherVectorFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorFirstFaulting(Vector<int> mask, int* address, Vector<int> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<int> GatherVectorFirstFaulting(Vector<int> mask, int* address, Vector<uint> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static Vector<long> GatherVectorFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorFirstFaulting(Vector<long> mask, long* address, Vector<long> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<long> GatherVectorFirstFaulting(Vector<long> mask, long* address, Vector<ulong> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<float> GatherVectorFirstFaulting(Vector<float> mask, float* address, Vector<int> indices) => GatherVectorFirstFaulting(mask, address, indices);
        // public static Vector<float> GatherVectorFirstFaulting(Vector<float> mask, Vector<uint> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<float> GatherVectorFirstFaulting(Vector<float> mask, float* address, Vector<uint> indices) => GatherVectorFirstFaulting(mask, address, indices);
        // public static Vector<uint> GatherVectorFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorFirstFaulting(Vector<uint> mask, uint* address, Vector<int> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorFirstFaulting(Vector<uint> mask, uint* address, Vector<uint> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static Vector<ulong> GatherVectorFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorFirstFaulting(Vector<ulong> mask, ulong* address, Vector<long> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorFirstFaulting(Vector<ulong> mask, ulong* address, Vector<ulong> indices) => GatherVectorFirstFaulting(mask, address, indices);
        public static unsafe Vector<int> GatherVectorInt16SignExtend(Vector<int> mask, short* address, Vector<int> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        // public static Vector<int> GatherVectorInt16SignExtend(Vector<int> mask, Vector<uint> addresses) => GatherVectorInt16SignExtend(mask, addresses);
        public static unsafe Vector<int> GatherVectorInt16SignExtend(Vector<int> mask, short* address, Vector<uint> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorInt16SignExtend(Vector<long> mask, short* address, Vector<long> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static Vector<long> GatherVectorInt16SignExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorInt16SignExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorInt16SignExtend(Vector<long> mask, short* address, Vector<ulong> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorInt16SignExtend(Vector<uint> mask, short* address, Vector<int> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        // public static Vector<uint> GatherVectorInt16SignExtend(Vector<uint> mask, Vector<uint> addresses) => GatherVectorInt16SignExtend(mask, addresses);
        public static unsafe Vector<uint> GatherVectorInt16SignExtend(Vector<uint> mask, short* address, Vector<uint> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorInt16SignExtend(Vector<ulong> mask, short* address, Vector<long> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorInt16SignExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorInt16SignExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorInt16SignExtend(Vector<ulong> mask, short* address, Vector<ulong> indices) => GatherVectorInt16SignExtend(mask, address, indices);
        public static unsafe Vector<int> GatherVectorInt16SignExtendFirstFaulting(Vector<int> mask, short* address, Vector<int> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        // public static Vector<int> GatherVectorInt16SignExtendFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorInt16SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorInt16SignExtendFirstFaulting(Vector<int> mask, short* address, Vector<uint> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<long> GatherVectorInt16SignExtendFirstFaulting(Vector<long> mask, short* address, Vector<long> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static Vector<long> GatherVectorInt16SignExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorInt16SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorInt16SignExtendFirstFaulting(Vector<long> mask, short* address, Vector<ulong> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorInt16SignExtendFirstFaulting(Vector<uint> mask, short* address, Vector<int> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        // public static Vector<uint> GatherVectorInt16SignExtendFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorInt16SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorInt16SignExtendFirstFaulting(Vector<uint> mask, short* address, Vector<uint> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorInt16SignExtendFirstFaulting(Vector<ulong> mask, short* address, Vector<long> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static Vector<ulong> GatherVectorInt16SignExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorInt16SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorInt16SignExtendFirstFaulting(Vector<ulong> mask, short* address, Vector<ulong> indices) => GatherVectorInt16SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<int> GatherVectorInt16WithByteOffsetsSignExtend(Vector<int> mask, short* address, Vector<int> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorInt16WithByteOffsetsSignExtend(Vector<int> mask, short* address, Vector<uint> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt16WithByteOffsetsSignExtend(Vector<long> mask, short* address, Vector<long> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt16WithByteOffsetsSignExtend(Vector<long> mask, short* address, Vector<ulong> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorInt16WithByteOffsetsSignExtend(Vector<uint> mask, short* address, Vector<int> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorInt16WithByteOffsetsSignExtend(Vector<uint> mask, short* address, Vector<uint> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt16WithByteOffsetsSignExtend(Vector<ulong> mask, short* address, Vector<long> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt16WithByteOffsetsSignExtend(Vector<ulong> mask, short* address, Vector<ulong> offsets) => GatherVectorInt16WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<int> mask, short* address, Vector<int> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<int> mask, short* address, Vector<uint> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<long> mask, short* address, Vector<long> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<long> mask, short* address, Vector<ulong> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<uint> mask, short* address, Vector<int> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<uint> mask, short* address, Vector<uint> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<ulong> mask, short* address, Vector<long> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(Vector<ulong> mask, short* address, Vector<ulong> offsets) => GatherVectorInt16WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, int* address, Vector<long> indices) => GatherVectorInt32SignExtend(mask, address, indices);
        public static Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorInt32SignExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorInt32SignExtend(Vector<long> mask, int* address, Vector<ulong> indices) => GatherVectorInt32SignExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, int* address, Vector<long> indices) => GatherVectorInt32SignExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorInt32SignExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorInt32SignExtend(Vector<ulong> mask, int* address, Vector<ulong> indices) => GatherVectorInt32SignExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorInt32SignExtendFirstFaulting(Vector<long> mask, int* address, Vector<long> indices) => GatherVectorInt32SignExtendFirstFaulting(mask, address, indices);
        public static Vector<long> GatherVectorInt32SignExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorInt32SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorInt32SignExtendFirstFaulting(Vector<long> mask, int* address, Vector<ulong> indices) => GatherVectorInt32SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorInt32SignExtendFirstFaulting(Vector<ulong> mask, int* address, Vector<long> indices) => GatherVectorInt32SignExtendFirstFaulting(mask, address, indices);
        public static Vector<ulong> GatherVectorInt32SignExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorInt32SignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorInt32SignExtendFirstFaulting(Vector<ulong> mask, int* address, Vector<ulong> indices) => GatherVectorInt32SignExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<long> GatherVectorInt32WithByteOffsetsSignExtend(Vector<long> mask, int* address, Vector<long> offsets) => GatherVectorInt32WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt32WithByteOffsetsSignExtend(Vector<long> mask, int* address, Vector<ulong> offsets) => GatherVectorInt32WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt32WithByteOffsetsSignExtend(Vector<ulong> mask, int* address, Vector<long> offsets) => GatherVectorInt32WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt32WithByteOffsetsSignExtend(Vector<ulong> mask, int* address, Vector<ulong> offsets) => GatherVectorInt32WithByteOffsetsSignExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(Vector<long> mask, int* address, Vector<long> offsets) => GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(Vector<long> mask, int* address, Vector<ulong> offsets) => GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(Vector<ulong> mask, int* address, Vector<long> offsets) => GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(Vector<ulong> mask, int* address, Vector<ulong> offsets) => GatherVectorInt32WithByteOffsetsSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorSByteSignExtend(Vector<int> mask, sbyte* address, Vector<int> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        // public static Vector<int> GatherVectorSByteSignExtend(Vector<int> mask, Vector<uint> addresses) => GatherVectorSByteSignExtend(mask, addresses);
        public static unsafe Vector<int> GatherVectorSByteSignExtend(Vector<int> mask, sbyte* address, Vector<uint> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorSByteSignExtend(Vector<long> mask, sbyte* address, Vector<long> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static Vector<long> GatherVectorSByteSignExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorSByteSignExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorSByteSignExtend(Vector<long> mask, sbyte* address, Vector<ulong> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorSByteSignExtend(Vector<uint> mask, sbyte* address, Vector<int> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        // public static Vector<uint> GatherVectorSByteSignExtend(Vector<uint> mask, Vector<uint> addresses) => GatherVectorSByteSignExtend(mask, addresses);
        public static unsafe Vector<uint> GatherVectorSByteSignExtend(Vector<uint> mask, sbyte* address, Vector<uint> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorSByteSignExtend(Vector<ulong> mask, sbyte* address, Vector<long> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorSByteSignExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorSByteSignExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorSByteSignExtend(Vector<ulong> mask, sbyte* address, Vector<ulong> indices) => GatherVectorSByteSignExtend(mask, address, indices);
        public static unsafe Vector<int> GatherVectorSByteSignExtendFirstFaulting(Vector<int> mask, sbyte* address, Vector<int> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        // public static Vector<int> GatherVectorSByteSignExtendFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorSByteSignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorSByteSignExtendFirstFaulting(Vector<int> mask, sbyte* address, Vector<uint> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorSByteSignExtendFirstFaulting(Vector<long> mask, sbyte* address, Vector<long> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static Vector<long> GatherVectorSByteSignExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorSByteSignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorSByteSignExtendFirstFaulting(Vector<long> mask, sbyte* address, Vector<ulong> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorSByteSignExtendFirstFaulting(Vector<uint> mask, sbyte* address, Vector<int> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        // public static Vector<uint> GatherVectorSByteSignExtendFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorSByteSignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorSByteSignExtendFirstFaulting(Vector<uint> mask, sbyte* address, Vector<uint> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorSByteSignExtendFirstFaulting(Vector<ulong> mask, sbyte* address, Vector<long> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static Vector<ulong> GatherVectorSByteSignExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorSByteSignExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorSByteSignExtendFirstFaulting(Vector<ulong> mask, sbyte* address, Vector<ulong> offsets) => GatherVectorSByteSignExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<int> mask, ushort* address, Vector<int> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<int> mask, ushort* address, Vector<uint> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<long> mask, ushort* address, Vector<long> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<long> mask, ushort* address, Vector<ulong> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<uint> mask, ushort* address, Vector<int> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<uint> mask, ushort* address, Vector<uint> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<ulong> mask, ushort* address, Vector<long> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt16WithByteOffsetsZeroExtend(Vector<ulong> mask, ushort* address, Vector<ulong> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<int> mask, ushort* address, Vector<int> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<int> mask, ushort* address, Vector<uint> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<long> mask, ushort* address, Vector<long> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<long> mask, ushort* address, Vector<ulong> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<uint> mask, ushort* address, Vector<int> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<uint> mask, ushort* address, Vector<uint> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<ulong> mask, ushort* address, Vector<long> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(Vector<ulong> mask, ushort* address, Vector<ulong> offsets) => GatherVectorUInt16WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt16ZeroExtend(Vector<int> mask, ushort* address, Vector<int> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        // public static Vector<int> GatherVectorUInt16ZeroExtend(Vector<int> mask, Vector<uint> addresses) => GatherVectorUInt16ZeroExtend(mask, addresses);
        public static unsafe Vector<int> GatherVectorUInt16ZeroExtend(Vector<int> mask, ushort* address, Vector<uint> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorUInt16ZeroExtend(Vector<long> mask, ushort* address, Vector<long> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static Vector<long> GatherVectorUInt16ZeroExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorUInt16ZeroExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorUInt16ZeroExtend(Vector<long> mask, ushort* address, Vector<ulong> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorUInt16ZeroExtend(Vector<uint> mask, ushort* address, Vector<int> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        // public static Vector<uint> GatherVectorUInt16ZeroExtend(Vector<uint> mask, Vector<uint> addresses) => GatherVectorUInt16ZeroExtend(mask, addresses);
        public static unsafe Vector<uint> GatherVectorUInt16ZeroExtend(Vector<uint> mask, ushort* address, Vector<uint> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorUInt16ZeroExtend(Vector<ulong> mask, ushort* address, Vector<long> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorUInt16ZeroExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorUInt16ZeroExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorUInt16ZeroExtend(Vector<ulong> mask, ushort* address, Vector<ulong> indices) => GatherVectorUInt16ZeroExtend(mask, address, indices);
        public static unsafe Vector<int> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<int> mask, ushort* address, Vector<int> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        // public static Vector<int> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<int> mask, ushort* address, Vector<uint> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<long> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<long> mask, ushort* address, Vector<long> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static Vector<long> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<long> mask, ushort* address, Vector<ulong> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<uint> mask, ushort* address, Vector<int> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        // public static Vector<uint> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<uint> mask, ushort* address, Vector<uint> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<ulong> mask, ushort* address, Vector<long> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static Vector<ulong> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorUInt16ZeroExtendFirstFaulting(Vector<ulong> mask, ushort* address, Vector<ulong> indices) => GatherVectorUInt16ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<int> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<int> mask, uint* address, Vector<int> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<int> mask, uint* address, Vector<uint> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<long> mask, uint* address, Vector<long> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<long> mask, uint* address, Vector<ulong> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<uint> mask, uint* address, Vector<int> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<uint> mask, uint* address, Vector<uint> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<ulong> mask, uint* address, Vector<long> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt32WithByteOffsetsZeroExtend(Vector<ulong> mask, uint* address, Vector<ulong> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtend(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<int> mask, uint* address, Vector<int> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<int> mask, uint* address, Vector<uint> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<long> mask, uint* address, Vector<long> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<long> mask, uint* address, Vector<ulong> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<uint> mask, uint* address, Vector<int> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<uint> mask, uint* address, Vector<uint> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<ulong> mask, uint* address, Vector<long> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(Vector<ulong> mask, uint* address, Vector<ulong> offsets) => GatherVectorUInt32WithByteOffsetsZeroExtendFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorUInt32ZeroExtend(Vector<int> mask, uint* address, Vector<int> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        // public static Vector<int> GatherVectorUInt32ZeroExtend(Vector<int> mask, Vector<uint> addresses) => GatherVectorUInt32ZeroExtend(mask, addresses);
        public static unsafe Vector<int> GatherVectorUInt32ZeroExtend(Vector<int> mask, uint* address, Vector<uint> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static unsafe Vector<long> GatherVectorUInt32ZeroExtend(Vector<long> mask, uint* address, Vector<long> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static Vector<long> GatherVectorUInt32ZeroExtend(Vector<long> mask, Vector<ulong> addresses) => GatherVectorUInt32ZeroExtend(mask, addresses);
        public static unsafe Vector<long> GatherVectorUInt32ZeroExtend(Vector<long> mask, uint* address, Vector<ulong> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorUInt32ZeroExtend(Vector<uint> mask, uint* address, Vector<int> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        // public static Vector<uint> GatherVectorUInt32ZeroExtend(Vector<uint> mask, Vector<uint> addresses) => GatherVectorUInt32ZeroExtend(mask, addresses);
        public static unsafe Vector<uint> GatherVectorUInt32ZeroExtend(Vector<uint> mask, uint* address, Vector<uint> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorUInt32ZeroExtend(Vector<ulong> mask, uint* address, Vector<long> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static Vector<ulong> GatherVectorUInt32ZeroExtend(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorUInt32ZeroExtend(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorUInt32ZeroExtend(Vector<ulong> mask, uint* address, Vector<ulong> indices) => GatherVectorUInt32ZeroExtend(mask, address, indices);
        public static unsafe Vector<int> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<int> mask, uint* address, Vector<int> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        // public static Vector<int> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<int> mask, Vector<uint> addresses) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<int> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<int> mask, uint* address, Vector<uint> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<long> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<long> mask, uint* address, Vector<long> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static Vector<long> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<long> mask, Vector<ulong> addresses) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<long> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<long> mask, uint* address, Vector<ulong> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<uint> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<uint> mask, uint* address, Vector<int> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        // public static Vector<uint> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<uint> mask, Vector<uint> addresses) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<uint> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<uint> mask, uint* address, Vector<uint> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<ulong> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<ulong> mask, uint* address, Vector<long> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static Vector<ulong> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<ulong> mask, Vector<ulong> addresses) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, addresses);
        public static unsafe Vector<ulong> GatherVectorUInt32ZeroExtendFirstFaulting(Vector<ulong> mask, uint* address, Vector<ulong> indices) => GatherVectorUInt32ZeroExtendFirstFaulting(mask, address, indices);
        public static unsafe Vector<double> GatherVectorWithByteOffsetFirstFaulting(Vector<double> mask, double* address, Vector<long> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<double> GatherVectorWithByteOffsetFirstFaulting(Vector<double> mask, double* address, Vector<ulong> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorWithByteOffsetFirstFaulting(Vector<int> mask, int* address, Vector<int> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorWithByteOffsetFirstFaulting(Vector<int> mask, int* address, Vector<uint> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorWithByteOffsetFirstFaulting(Vector<long> mask, long* address, Vector<long> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorWithByteOffsetFirstFaulting(Vector<long> mask, long* address, Vector<ulong> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<float> GatherVectorWithByteOffsetFirstFaulting(Vector<float> mask, float* address, Vector<int> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<float> GatherVectorWithByteOffsetFirstFaulting(Vector<float> mask, float* address, Vector<uint> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorWithByteOffsetFirstFaulting(Vector<uint> mask, uint* address, Vector<int> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorWithByteOffsetFirstFaulting(Vector<uint> mask, uint* address, Vector<uint> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorWithByteOffsetFirstFaulting(Vector<ulong> mask, ulong* address, Vector<long> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorWithByteOffsetFirstFaulting(Vector<ulong> mask, ulong* address, Vector<ulong> offsets) => GatherVectorWithByteOffsetFirstFaulting(mask, address, offsets);
        public static unsafe Vector<double> GatherVectorWithByteOffsets(Vector<double> mask, double* address, Vector<long> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<double> GatherVectorWithByteOffsets(Vector<double> mask, double* address, Vector<ulong> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorWithByteOffsets(Vector<int> mask, int* address, Vector<int> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<int> GatherVectorWithByteOffsets(Vector<int> mask, int* address, Vector<uint> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorWithByteOffsets(Vector<long> mask, long* address, Vector<long> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<long> GatherVectorWithByteOffsets(Vector<long> mask, long* address, Vector<ulong> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<float> GatherVectorWithByteOffsets(Vector<float> mask, float* address, Vector<int> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<float> GatherVectorWithByteOffsets(Vector<float> mask, float* address, Vector<uint> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorWithByteOffsets(Vector<uint> mask, uint* address, Vector<int> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<uint> GatherVectorWithByteOffsets(Vector<uint> mask, uint* address, Vector<uint> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorWithByteOffsets(Vector<ulong> mask, ulong* address, Vector<long> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);
        public static unsafe Vector<ulong> GatherVectorWithByteOffsets(Vector<ulong> mask, ulong* address, Vector<ulong> offsets) => GatherVectorWithByteOffsets(mask, address, offsets);

}
```
## SVE: Rename LoadVector extensions to match the type of the address

**Approved** | [#runtime/115360](https://github.com/dotnet/runtime/issues/115360#issuecomment-2981333387) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=0h53m18s)

The corrected names look more correct.  Looks good as proposed.

```C#
public static unsafe Vector<long> LoadVectorSByteSignExtendToInt64(sbyte* address); // LD1SB
public static unsafe Vector<ushort> LoadVectorSByteSignExtendToUInt16(sbyte* address); // LD1SB
public static unsafe Vector<uint> LoadVectorSByteSignExtendToUInt32(sbyte* address); // LD1SB
public static unsafe Vector<ulong> LoadVectorSByteSignExtendToUInt64(sbyte* address); // LD1SB

public static unsafe Vector<int> LoadVectorUInt16NonFaultingZeroExtendToInt32(Vector<int> mask, ushort* address); // LDNF1H
public static unsafe Vector<long> LoadVectorUInt16NonFaultingZeroExtendToInt64(Vector<long> mask, ushort* address); // LDNF1H
public static unsafe Vector<uint> LoadVectorUInt16NonFaultingZeroExtendToUInt32(Vector<uint> mask, ushort* address); // LDNF1H
public static unsafe Vector<ulong> LoadVectorUInt16NonFaultingZeroExtendToUInt64(Vector<ulong> mask, ushort* address); // LDNF1H

public static unsafe Vector<long> LoadVectorUInt32NonFaultingZeroExtendToInt64(Vector<long> mask, uint* address); // LDNF1W
public static unsafe Vector<ulong> LoadVectorUInt32NonFaultingZeroExtendToUInt64(Vector<ulong> mask, uint* address); // LDNF1W

public static unsafe Vector<long> LoadVectorUInt32ZeroExtendToInt64(uint* address); // LD1W
public static unsafe Vector<ulong> LoadVectorUInt32ZeroExtendToUInt64(uint* address); // LD1W

public static unsafe Vector<T> LoadVectorUInt32ZeroExtendFirstFaulting(uint* address); // LDFF1W // predicated

public static unsafe Vector<T> LoadVectorUInt16ZeroExtendFirstFaulting(ushort* address); // LDFF1H // predicated
```
## SVE: Correct multi register load methods

**Approved** | [#runtime/114910](https://github.com/dotnet/runtime/issues/114910#issuecomment-2981359633) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=0h55m7s)

* The consistent ordering is Load #x Vector, so all of the 2x/3x/4x were shifted left.
* The return-tuples should be named, the consistent naming would be (Value1, Value2, ..., ValueN)

```c#
namespace System.Runtime.Intrinsics.Arm;

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: loads
{
    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>) Load2xVector(Vector<T> mask, T* address); // LD1W or LD1D or LD1B or LD1H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>) Load3xVector(Vector<T> mask, T* address); // LD1W or LD1D or LD1B or LD1H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>, Vector<T>) Load4xVector(Vector<T> mask, T* address); // LD1W or LD1D or LD1B or LD1H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>) Load2xVectorAndUnzip(Vector<T> mask, T* address); // LD2W or LD2D or LD2B or LD2H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>) Load3xVectorAndUnzip(Vector<T> mask, T* address); // LD3W or LD3D or LD3B or LD3H

    /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
    public static unsafe (Vector<T>, Vector<T>, Vector<T>, Vector<T>) Load4xVectorAndUnzip(Vector<T> mask, T* address); // LD4W or LD4D or LD4B or LD4H
}
```
## ARM64 SVE: LoadVectorNonFaulting APIs require a mask due to FFR

**Approved** | [#runtime/108234](https://github.com/dotnet/runtime/issues/108234#issuecomment-2981375048) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=1h2m22s)

The insertion of the (`return type` mask) parameter as the first parameter looks good as proposed.

```c#
namespace System.Runtime.Intrinsics.Arm;

public partial class Sve
{

  public static unsafe Vector<short> LoadVectorByteNonFaultingZeroExtendToInt16(Vector<short> mask, byte* address); // LDNF1B

  public static unsafe Vector<int> LoadVectorByteNonFaultingZeroExtendToInt32(Vector<int> mask, byte* address); // LDNF1B

  public static unsafe Vector<long> LoadVectorByteNonFaultingZeroExtendToInt64(Vector<long> mask, byte* address); // LDNF1B

  public static unsafe Vector<ushort> LoadVectorByteNonFaultingZeroExtendToUInt16(Vector<ushort> mask, byte* address); // LDNF1B

  public static unsafe Vector<uint> LoadVectorByteNonFaultingZeroExtendToUInt32(Vector<uint> mask, byte* address); // LDNF1B

  public static unsafe Vector<ulong> LoadVectorByteNonFaultingZeroExtendToUInt64(Vector<ulong> mask, byte* address); // LDNF1B


  public static unsafe Vector<int> LoadVectorInt16NonFaultingSignExtendToInt32(Vector<int> mask, short* address); // LDNF1SH

  public static unsafe Vector<long> LoadVectorInt16NonFaultingSignExtendToInt64(Vector<long> mask, short* address); // LDNF1SH

  public static unsafe Vector<uint> LoadVectorInt16NonFaultingSignExtendToUInt32(Vector<uint> mask, short* address); // LDNF1SH

  public static unsafe Vector<ulong> LoadVectorInt16NonFaultingSignExtendToUInt64(Vector<ulong> mask, short* address); // LDNF1SH


  public static unsafe Vector<long> LoadVectorInt32NonFaultingSignExtendToInt64(Vector<long> mask, int* address); // LDNF1SW

  public static unsafe Vector<ulong> LoadVectorInt32NonFaultingSignExtendToUInt64(Vector<ulong> mask, int* address); // LDNF1SW


  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe Vector<T> LoadVectorNonFaulting(Vector<T> mask, T* address); // LDNF1W or LDNF1D or LDNF1B or LDNF1H


  public static unsafe Vector<short> LoadVectorSByteNonFaultingSignExtendToInt16(Vector<short> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<int> LoadVectorSByteNonFaultingSignExtendToInt32(Vector<int> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<long> LoadVectorSByteNonFaultingSignExtendToInt64(Vector<long> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<ushort> LoadVectorSByteNonFaultingSignExtendToUInt16(Vector<ushort> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<uint> LoadVectorSByteNonFaultingSignExtendToUInt32(Vector<uint> mask, sbyte* address); // LDNF1SB

  public static unsafe Vector<ulong> LoadVectorSByteNonFaultingSignExtendToUInt64(Vector<ulong> mask, sbyte* address); // LDNF1SB


  public static unsafe Vector<int> LoadVectorUInt16NonFaultingZeroExtendToInt32(Vector<int> mask, ushort* address); // LDNF1H

  public static unsafe Vector<long> LoadVectorUInt16NonFaultingZeroExtendToInt64(Vector<long> mask, ushort* address); // LDNF1H

  public static unsafe Vector<uint> LoadVectorUInt16NonFaultingZeroExtendToUInt32(Vector<uint> mask, ushort* address); // LDNF1H

  public static unsafe Vector<ulong> LoadVectorUInt16NonFaultingZeroExtendToUInt64(Vector<ulong> mask, ushort* address); // LDNF1H


  public static unsafe Vector<long> LoadVectorUInt32NonFaultingZeroExtendToInt64(Vector<long> mask, uint* address); // LDNF1W

  public static unsafe Vector<ulong> LoadVectorUInt32NonFaultingZeroExtendToUInt64(Vector<ulong> mask, uint* address); // LDNF1W

}
``` 
## SVE: Correct multi register store methods

**Approved** | [#runtime/115338](https://github.com/dotnet/runtime/issues/115338#issuecomment-2981386827) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=1h6m54s)

Updating to 4 Store methods and 3 StoreAndZip methods looks good as proposed

```C#
namespace System.Runtime.Intrinsics.Arm

/// VectorT Summary
public abstract class Sve : AdvSimd /// Feature: FEAT_SVE  Category: loads
{
  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, Vector<T> data); // ST1W or ST1D or ST1B or ST1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
 public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2) data); // ST1W or ST1D or ST1B or ST1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3) data); // ST1W or ST1D or ST1B or ST1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void Store(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3, Vector<T> Value4) data); // ST1W or ST1D or ST1B or ST1H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void StoreAndZip(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2) data); // ST2W or ST2D or ST1B or ST2H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void StoreAndZip(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3) data); // ST3W or ST3D or ST3B or ST3H

  /// T: float, double, sbyte, short, int, long, byte, ushort, uint, ulong
  public static unsafe void StoreAndZip(Vector<T> mask, T* address, (Vector<T> Value1, Vector<T> Value2, Vector<T> Value3, Vector<T> Value4) data); // ST4W or ST4D or ST4B or ST4H
}
```
## SLH-DSA

**Approved** | [#runtime/116282](https://github.com/dotnet/runtime/issues/116282#issuecomment-2981499495) | [Video](https://www.youtube.com/watch?v=c1tvQN-IooM&t=1h11m0s)


* We removed CmsSigner.HasPrivateKey because it was a) slightly confusing, and b) didn't have a strong use case.

```c#
namespace System.Security.Cryptogrpahy
{
    // System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public abstract class SlhDsa : IDisposable
    {
        public static bool IsSupported { get; }

        protected SlhDsa(SlhDsaAlgorithm algorithm);

        public SlhDsaAlgorithm Algorithm { get; }

        // Sign/Verify
        public byte[] SignData(byte[] data, byte[]? context = null);
        public void SignData(ReadOnlySpan<byte> data, Span<byte> destination, ReadOnlySpan<byte> context = default);
        protected abstract void SignDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, Span<byte> destination);
        
        public bool VerifyData(byte[] data, byte[] signature, byte[]? context = null);
        public bool VerifyData(ReadOnlySpan<byte> data, ReadOnlySpan<byte> signature, ReadOnlySpan<byte> context = default);
        protected abstract bool VerifyDataCore(ReadOnlySpan<byte> data, ReadOnlySpan<byte> context, ReadOnlySpan<byte> signature);

        public byte[] SignPreHash(byte[] hash, string hashAlgorithmOid, byte[]? context = null);
        public void SignPreHash(ReadOnlySpan<byte> hash, Span<byte> destination, string hashAlgorithmOid, ReadOnlySpan<byte> context = default);
        protected abstract void SignPreHashCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> context, string hashAlgorithmOid, Span<byte> destination);

        public bool VerifyPreHash(byte[] hash, byte[] signature, string hashAlgorithmOid, byte[]? context = null);
        public bool VerifyPreHash(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> signature, string hashAlgorithmOid, ReadOnlySpan<byte> context = default);
        protected abstract bool VerifyPreHashCore(ReadOnlySpan<byte> hash, ReadOnlySpan<byte> context, string hashAlgorithmOid, ReadOnlySpan<byte> signature);

        // Create/Import/Export
        public static SlhDsa GenerateKey(SlhDsaAlgorithm algorithm);

        public static SlhDsa ImportSlhDsaPublicKey(SlhDsaAlgorithm algorithm, byte[] source);
        public static SlhDsa ImportSlhDsaPublicKey(SlhDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public static SlhDsa ImportSlhDsaSecretKey(SlhDsaAlgorithm algorithm, byte[] source);
        public static SlhDsa ImportSlhDsaSecretKey(SlhDsaAlgorithm algorithm, ReadOnlySpan<byte> source);

        public byte[] ExportSlhDsaPublicKey();
        public void ExportSlhDsaPublicKey(Span<byte> destination);
        protected abstract void ExportSlhDsaPublicKeyCore(Span<byte> destination);

        public byte[] ExportSlhDsaSecretKey();
        public void ExportSlhDsaSecretKey(Span<byte> destination);
        protected abstract void ExportSlhDsaSecretKeyCore(Span<byte> destination);
        
        // SPKI / PKCS#8
        public static SlhDsa ImportSubjectPublicKeyInfo(byte[] source);
        public static SlhDsa ImportSubjectPublicKeyInfo(ReadOnlySpan<byte> source);

        public static SlhDsa ImportPkcs8PrivateKey(byte[] source);
        public static SlhDsa ImportPkcs8PrivateKey(ReadOnlySpan<byte> source);

        public static SlhDsa ImportFromPem(ReadOnlySpan<char> source);
        public static SlhDsa ImportFromPem(string source);

        public static SlhDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, ReadOnlySpan<byte> source);
        public static SlhDsa ImportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, ReadOnlySpan<byte> source);
        public static SlhDsa ImportEncryptedPkcs8PrivateKey(string password, byte[] source);

        public static SlhDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<byte> passwordBytes);
        public static SlhDsa ImportFromEncryptedPem(ReadOnlySpan<char> source, ReadOnlySpan<char> password);
        public static SlhDsa ImportFromEncryptedPem(string source, byte[] passwordBytes);
        public static SlhDsa ImportFromEncryptedPem(string source, string password);

        public byte[] ExportSubjectPublicKeyInfo();
        public bool TryExportSubjectPublicKeyInfo(Span<byte> destination, out int bytesWritten);
        public string ExportSubjectPublicKeyInfoPem();

        public byte[] ExportPkcs8PrivateKey();
        public bool TryExportPkcs8PrivateKey(Span<byte> destination, out int bytesWritten);
        protected virtual bool TryExportPkcs8PrivateKeyCore(Span<byte> destination, out int bytesWritten);
        public string ExportPkcs8PrivateKeyPem();

        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public byte[] ExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(ReadOnlySpan<char> password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);
        public bool TryExportEncryptedPkcs8PrivateKey(string password, PbeParameters pbeParameters, Span<byte> destination, out int bytesWritten);

        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<byte> passwordBytes, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(ReadOnlySpan<char> password, PbeParameters pbeParameters);
        public string ExportEncryptedPkcs8PrivateKeyPem(string password, PbeParameters pbeParameters);

        public void Dispose();
        protected virtual void Dispose(bool disposing);
    }

    // System.Security.Cryptography and Microsoft.Bcl.Cryptography
    [DebuggerDisplay("{Name,nq}")]
    [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed class SlhDsaAlgorithm : IEquatable<SlhDsaAlgorithm>
    {
        internal SlhDsaAlgorithm();

        public string Name { get; }
        public int PublicKeySizeInBytes { get; }
        public int SecretKeySizeInBytes { get; }
        public int SignatureSizeInBytes { get; }

        public static SlhDsaAlgorithm SlhDsaSha2_128f { get; }
        public static SlhDsaAlgorithm SlhDsaSha2_128s { get; }
        public static SlhDsaAlgorithm SlhDsaSha2_192f { get; }
        public static SlhDsaAlgorithm SlhDsaSha2_192s { get; }
        public static SlhDsaAlgorithm SlhDsaSha2_256f { get; }
        public static SlhDsaAlgorithm SlhDsaSha2_256s { get; }
        public static SlhDsaAlgorithm SlhDsaShake128f { get; }
        public static SlhDsaAlgorithm SlhDsaShake128s { get; }
        public static SlhDsaAlgorithm SlhDsaShake192f { get; }
        public static SlhDsaAlgorithm SlhDsaShake192s { get; }
        public static SlhDsaAlgorithm SlhDsaShake256f { get; }
        public static SlhDsaAlgorithm SlhDsaShake256s { get; }

        public bool Equals(SlhDsaAlgorithm? other);

        public static bool operator ==(SlhDsaAlgorithm? left, SlhDsaAlgorithm? right);
        public static bool operator !=(SlhDsaAlgorithm? left, SlhDsaAlgorithm? right);
    }

    // System.Security.Cryptography only
    [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
    public sealed class SlhDsaOpenSsl : SlhDsa
    {
        [UnsupportedOSPlatformAttribute("android")]
        [UnsupportedOSPlatformAttribute("browser")]
        [UnsupportedOSPlatformAttribute("ios")]
        [UnsupportedOSPlatformAttribute("osx")]
        [UnsupportedOSPlatformAttribute("tvos")]
        [UnsupportedOSPlatformAttribute("windows")]
        public SlhDsaOpenSsl(SafeEvpPKeyHandle pkeyHandle);

        public SafeEvpPKeyHandle DuplicateKeyHandle();
    }
}

namespace System.Security.Cryptography.X509Certificates
{
    // System.Security.Cryptography only
    public sealed partial class CertificateRequest
    {
        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(X500DistinguishedName subjectName, SlhDsa key);

        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CertificateRequest(string subjectName, SlhDsa key);
    }
    
    // System.Security.Cryptography only
    public sealed partial class PublicKey
    {
        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public PublicKey(SlhDsa key);

        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        [UnsupportedOSPlatformAttribute("browser")]
        public SlhDsa? GetSlhDsaPublicKey();
    }

    // System.Security.Cryptography only
    public partial class X509Certificate2
    {
        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public X509Certificate2 CopyWithPrivateKey(SlhDsa privateKey);

        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public SlhDsa? GetSlhDsaPrivateKey();

        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public SlhDsa? GetSlhDsaPublicKey();
    }

    // System.Security.Cryptography only
    public abstract partial class X509SignatureGenerator
    {
        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public static X509SignatureGenerator CreateForSlhDsa(SlhDsa key);
    }

    // Microsoft.Bcl.Cryptography only
    public static partial class X509CertificateKeyAccessors
    {
        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static SlhDsa? GetSlhDsaPublicKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static SlhDsa? GetSlhDsaPrivateKey(this X509Certificate2 certificate);

        [Experimental("SYSLIB5006", UrlFormat = "https://aka.ms/dotnet-warnings/{0}")]
        public static X509Certificate2 CopyWithPrivateKey(this X509Certificate2 certificate, SlhDsa privateKey)
    }
}

#if NET
namespace System.Security.Cryptography.Pkcs
{
    public sealed partial class CmsSigner
    {
        [Experimental("SYSLIB5006", UrlFormat="https://aka.ms/dotnet-warnings/{0}")]
        public CmsSigner(SubjectIdentifierType signerIdentifierType, X509Certificate2? certificate, SlhDsa? privateKey);

        public bool HasPrivateKey { get; }
    }
}
#endif
```

# APIs in the Microsoft.ML.Core assembly

## Microsoft.ML.Core.Data

```C#
 namespace Microsoft.ML.Core.Data {
     public interface IDataReader<in TSource> {
         Schema GetOutputSchema();
         IDataView Read(TSource input);
     }
     public interface IDataReaderEstimator<in TSource, out TReader> where TReader : IDataReader<TSource> {
         TReader Fit(TSource input);
         SchemaShape GetOutputSchema();
     }
     public interface IEstimator<out TTransformer> where TTransformer : ITransformer {
         TTransformer Fit(IDataView input);
         SchemaShape GetOutputSchema(SchemaShape inputSchema);
     }
     public interface ITransformer {
         bool IsRowToRowMapper { get; }
         Schema GetOutputSchema(Schema inputSchema);
         IRowToRowMapper GetRowToRowMapper(Schema inputSchema);
         IDataView Transform(IDataView input);
     }
     public class SchemaException : Exception {
         public SchemaException();
     }
     public sealed class SchemaShape {
         public readonly SchemaShape.Column[] Columns;
         public SchemaShape(IEnumerable<SchemaShape.Column> columns);
         public static SchemaShape Create(Schema schema);
         public bool TryFindColumn(string name, out SchemaShape.Column column);
         public sealed class Column {
             public readonly SchemaShape Metadata;
             public readonly ColumnType ItemType;
             public readonly bool IsKey;
             public readonly string Name;
             public readonly SchemaShape.Column.VectorKind Kind;
             public Column(string name, SchemaShape.Column.VectorKind vecKind, ColumnType itemType, bool isKey, SchemaShape metadata=null);
             public string GetTypeString();
             public bool IsCompatibleWith(SchemaShape.Column inputColumn);
             public enum VectorKind {
                 Scalar = 0,
                 VariableVector = 2,
                 Vector = 1,
             }
         }
     }
 }
```

## Microsoft.ML.Runtime

```C#
 namespace Microsoft.ML.Runtime {
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct ChannelMessage {
         public readonly ChannelMessageKind Kind;
         public readonly MessageSensitivity Sensitivity;
         public ChannelMessage(ChannelMessageKind kind, MessageSensitivity sensitivity, string message);
         public ChannelMessage(ChannelMessageKind kind, MessageSensitivity sensitivity, string fmt, params object[] args);
         public string Message { get; }
     }
     public enum ChannelMessageKind {
         Error = 3,
         Info = 1,
         Trace = 0,
         Warning = 2,
     }
     public sealed class ComponentCatalog {
         public IEnumerable<ComponentCatalog.EntryPointInfo> AllEntryPoints();
         public static TRes CreateInstance<TRes>(IHostEnvironment env, Type signatureType, string name, string options, params object[] extra) where TRes : class;
         public ComponentCatalog.LoadableClassInfo[] FindLoadableClasses(string name);
         public ComponentCatalog.LoadableClassInfo[] FindLoadableClasses<TArgs, TSig>();
         public ComponentCatalog.LoadableClassInfo[] FindLoadableClasses<TSig>();
         public ComponentCatalog.LoadableClassInfo[] GetAllClasses();
         public IEnumerable<string> GetAllComponentKinds();
         public IEnumerable<ComponentCatalog.ComponentInfo> GetAllComponents(string kind);
         public IEnumerable<ComponentCatalog.ComponentInfo> GetAllComponents(Type interfaceType);
         public ComponentCatalog.LoadableClassInfo[] GetAllDerivedClasses(Type typeBase, Type typeSig);
         public Type[] GetAllSignatureTypes();
         public ComponentCatalog.LoadableClassInfo GetLoadableClassInfo(string loadName, Type signatureType);
         public ComponentCatalog.LoadableClassInfo GetLoadableClassInfo<TSig>(string loadName);
         public void RegisterAssembly(Assembly assembly, bool throwOnError=true);
         public static string SignatureToString(Type sig);
         public static bool TryCreateInstance<TRes, TSig>(IHostEnvironment env, out TRes result, string name, string options, params object[] extra) where TRes : class;
         public bool TryFindComponent(string kind, string alias, out ComponentCatalog.ComponentInfo component);
         public bool TryFindComponent(Type argumentType, out ComponentCatalog.ComponentInfo component);
         public bool TryFindComponent(Type interfaceType, string alias, out ComponentCatalog.ComponentInfo component);
         public bool TryFindComponent(Type interfaceType, Type argumentType, out ComponentCatalog.ComponentInfo component);
         public bool TryFindComponentCaseInsensitive(Type interfaceType, string alias, out ComponentCatalog.ComponentInfo component);
         public bool TryFindEntryPoint(string name, out ComponentCatalog.EntryPointInfo entryPoint);
         public bool TryGetComponentKind(Type signatureType, out string kind);
         public bool TryGetComponentShortName(Type type, out string name);
         public sealed class ComponentInfo {
             public readonly string Description;
             public readonly string FriendlyName;
             public readonly string Kind;
             public readonly string Name;
             public readonly string[] Aliases;
             public readonly Type ArgumentType;
             public readonly Type InterfaceType;
         }
         public sealed class EntryPointInfo {
             public readonly ObsoleteAttribute ObsoleteAttribute;
             public readonly MethodInfo Method;
             public readonly string Description;
             public readonly string FriendlyName;
             public readonly string Name;
             public readonly string ShortName;
             public readonly string[] XmlInclude;
             public readonly Type InputType;
             public readonly Type OutputType;
             public readonly Type[] InputKinds;
             public readonly Type[] OutputKinds;
             public override string ToString();
         }
         public sealed class LoadableClassInfo {
             public Type ArgType { get; }
             public ConstructorInfo Constructor { get; }
             public MethodInfo CreateMethod { get; }
             public string DocName { get; }
             public MethodInfo InstanceGetter { get; }
             public bool IsHidden { get; }
             public Type LoaderType { get; }
             public IReadOnlyList<string> LoadNames { get; }
             public bool RequireEnvironment { get; }
             public IReadOnlyList<Type> SignatureTypes { get; }
             public string Summary { get; }
             public Type Type { get; }
             public string UserName { get; }
             public object CreateArguments();
             public object CreateInstance(IHostEnvironment env, object args, object[] extra);
             public TRes CreateInstance<TRes>(IHostEnvironment env);
             public TRes CreateInstance<TRes>(IHostEnvironment env, object args, object[] extra);
         }
     }
     public static class ComponentFactoryUtils {
         public static IComponentFactory<TArg1, TArg2, TArg3, TComponent> CreateFromFunction<TArg1, TArg2, TArg3, TComponent>(Func<IHostEnvironment, TArg1, TArg2, TArg3, TComponent> factory);
         public static IComponentFactory<TArg1, TArg2, TComponent> CreateFromFunction<TArg1, TArg2, TComponent>(Func<IHostEnvironment, TArg1, TArg2, TComponent> factory);
         public static IComponentFactory<TArg1, TComponent> CreateFromFunction<TArg1, TComponent>(Func<IHostEnvironment, TArg1, TComponent> factory);
         public static IComponentFactory<TComponent> CreateFromFunction<TComponent>(Func<IHostEnvironment, TComponent> factory);
     }
     public static class Contracts {
         public const string IsMarkedKey = "ML_IsMarked";
         public const string SensitivityKey = "ML_Sensitivity";
         public static void Assert(this IExceptionContext ctx, bool f);
         public static void Assert(this IExceptionContext ctx, bool f, string msg);
         public static void Assert(bool f);
         public static void Assert(bool f, string msg);
         public static void AssertNonEmpty(this IExceptionContext ctx, string s);
         public static void AssertNonEmpty(this IExceptionContext ctx, string s, string msg);
         public static void AssertNonEmpty(string s);
         public static void AssertNonEmpty(string s, string msg);
         public static void AssertNonEmpty<T>(this IExceptionContext ctx, ICollection<T> args);
         public static void AssertNonEmpty<T>(this IExceptionContext ctx, ICollection<T> args, string msg);
         public static void AssertNonEmpty<T>(ICollection<T> args);
         public static void AssertNonEmpty<T>(ICollection<T> args, string msg);
         public static void AssertNonEmpty<T>(ReadOnlySpan<T> args);
         public static void AssertNonEmpty<T>(Span<T> args);
         public static void AssertNonWhiteSpace(this IExceptionContext ctx, string s);
         public static void AssertNonWhiteSpace(this IExceptionContext ctx, string s, string msg);
         public static void AssertNonWhiteSpace(string s);
         public static void AssertNonWhiteSpace(string s, string msg);
         public static void AssertValue<T>(this IExceptionContext ctx, T val) where T : class;
         public static void AssertValue<T>(this IExceptionContext ctx, T val, string paramName) where T : class;
         public static void AssertValue<T>(this IExceptionContext ctx, T val, string name, string msg) where T : class;
         public static void AssertValue<T>(T val) where T : class;
         public static void AssertValue<T>(T val, string paramName) where T : class;
         public static void AssertValue<T>(T val, string name, string msg) where T : class;
         public static void AssertValueOrNull<T>(this IExceptionContext ctx, T val) where T : class;
         public static void AssertValueOrNull<T>(T val) where T : class;
         public static void Check(this IExceptionContext ctx, bool f);
         public static void Check(this IExceptionContext ctx, bool f, string msg);
         public static void Check(bool f);
         public static void Check(bool f, string msg);
         public static void CheckAlive(this IHostEnvironment env);
         public static void CheckDecode(this IExceptionContext ctx, bool f);
         public static void CheckDecode(this IExceptionContext ctx, bool f, string msg);
         public static void CheckDecode(bool f);
         public static void CheckDecode(bool f, string msg);
         public static void CheckIO(this IExceptionContext ctx, bool f);
         public static void CheckIO(this IExceptionContext ctx, bool f, string msg);
         public static void CheckIO(bool f);
         public static void CheckIO(bool f, string msg);
         public static string CheckNonEmpty(this IExceptionContext ctx, string s, string paramName);
         public static string CheckNonEmpty(this IExceptionContext ctx, string s, string paramName, string msg);
         public static string CheckNonEmpty(string s, string paramName);
         public static string CheckNonEmpty(string s, string paramName, string msg);
         public static ICollection<T> CheckNonEmpty<T>(this IExceptionContext ctx, ICollection<T> args, string paramName);
         public static ICollection<T> CheckNonEmpty<T>(this IExceptionContext ctx, ICollection<T> args, string paramName, string msg);
         public static T[] CheckNonEmpty<T>(this IExceptionContext ctx, T[] args, string paramName);
         public static T[] CheckNonEmpty<T>(this IExceptionContext ctx, T[] args, string paramName, string msg);
         public static ICollection<T> CheckNonEmpty<T>(ICollection<T> args, string paramName);
         public static ICollection<T> CheckNonEmpty<T>(ICollection<T> args, string paramName, string msg);
         public static T[] CheckNonEmpty<T>(T[] args, string paramName);
         public static T[] CheckNonEmpty<T>(T[] args, string paramName, string msg);
         public static string CheckNonWhiteSpace(this IExceptionContext ctx, string s, string paramName);
         public static string CheckNonWhiteSpace(this IExceptionContext ctx, string s, string paramName, string msg);
         public static string CheckNonWhiteSpace(string s, string paramName);
         public static string CheckNonWhiteSpace(string s, string paramName, string msg);
         public static void CheckParam(this IExceptionContext ctx, bool f, string paramName);
         public static void CheckParam(this IExceptionContext ctx, bool f, string paramName, string msg);
         public static void CheckParam(bool f, string paramName);
         public static void CheckParam(bool f, string paramName, string msg);
         public static void CheckParamValue<T>(this IExceptionContext ctx, bool f, T value, string paramName, string msg);
         public static void CheckParamValue<T>(bool f, T value, string paramName, string msg);
         public static T CheckRef<T>(this IExceptionContext ctx, T val, string paramName) where T : class;
         public static T CheckRef<T>(this IExceptionContext ctx, T val, string paramName, string msg) where T : class;
         public static T CheckRef<T>(T val, string paramName) where T : class;
         public static void CheckUserArg(this IExceptionContext ctx, bool f, string name);
         public static void CheckUserArg(this IExceptionContext ctx, bool f, string name, string msg);
         public static void CheckUserArg(bool f, string name);
         public static void CheckUserArg(bool f, string name, string msg);
         public static void CheckValue<T>(this IExceptionContext ctx, T val, string paramName) where T : class;
         public static T CheckValue<T>(this IExceptionContext ctx, T val, string paramName, string msg) where T : class;
         public static void CheckValue<T>(T val, string paramName) where T : class;
         public static T CheckValue<T>(T val, string paramName, string msg) where T : class;
         public static void CheckValueOrNull<T>(this IExceptionContext ctx, T val) where T : class;
         public static void CheckValueOrNull<T>(T val) where T : class;
         public static Exception Except();
         public static Exception Except(this IExceptionContext ctx);
         public static Exception Except(this IExceptionContext ctx, Exception inner, string msg);
         public static Exception Except(this IExceptionContext ctx, Exception inner, string msg, params object[] args);
         public static Exception Except(this IExceptionContext ctx, string msg);
         public static Exception Except(this IExceptionContext ctx, string msg, params object[] args);
         public static Exception Except(Exception inner, string msg);
         public static Exception Except(Exception inner, string msg, params object[] args);
         public static Exception Except(string msg);
         public static Exception Except(string msg, params object[] args);
         public static Exception ExceptDecode();
         public static Exception ExceptDecode(this IExceptionContext ctx);
         public static Exception ExceptDecode(this IExceptionContext ctx, Exception inner, string msg);
         public static Exception ExceptDecode(this IExceptionContext ctx, Exception inner, string msg, params object[] args);
         public static Exception ExceptDecode(this IExceptionContext ctx, string msg);
         public static Exception ExceptDecode(this IExceptionContext ctx, string msg, params object[] args);
         public static Exception ExceptDecode(Exception inner, string msg);
         public static Exception ExceptDecode(Exception inner, string msg, params object[] args);
         public static Exception ExceptDecode(string msg);
         public static Exception ExceptDecode(string msg, params object[] args);
         public static Exception ExceptEmpty(this IExceptionContext ctx, string paramName);
         public static Exception ExceptEmpty(this IExceptionContext ctx, string paramName, string msg);
         public static Exception ExceptEmpty(this IExceptionContext ctx, string paramName, string msg, params object[] args);
         public static Exception ExceptEmpty(string paramName);
         public static Exception ExceptEmpty(string paramName, string msg);
         public static Exception ExceptEmpty(string paramName, string msg, params object[] args);
         public static Exception ExceptIO();
         public static Exception ExceptIO(this IExceptionContext ctx);
         public static Exception ExceptIO(this IExceptionContext ctx, Exception inner, string msg);
         public static Exception ExceptIO(this IExceptionContext ctx, Exception inner, string msg, params object[] args);
         public static Exception ExceptIO(this IExceptionContext ctx, string msg);
         public static Exception ExceptIO(this IExceptionContext ctx, string msg, params object[] args);
         public static Exception ExceptIO(Exception inner, string msg);
         public static Exception ExceptIO(Exception inner, string msg, params object[] args);
         public static Exception ExceptIO(string msg);
         public static Exception ExceptIO(string msg, params object[] args);
         public static Exception ExceptNotImpl();
         public static Exception ExceptNotImpl(this IExceptionContext ctx);
         public static Exception ExceptNotImpl(this IExceptionContext ctx, string msg);
         public static Exception ExceptNotImpl(this IExceptionContext ctx, string msg, params object[] args);
         public static Exception ExceptNotImpl(string msg);
         public static Exception ExceptNotImpl(string msg, params object[] args);
         public static Exception ExceptNotSupp();
         public static Exception ExceptNotSupp(this IExceptionContext ctx);
         public static Exception ExceptNotSupp(this IExceptionContext ctx, string msg);
         public static Exception ExceptNotSupp(this IExceptionContext ctx, string msg, params object[] args);
         public static Exception ExceptNotSupp(string msg);
         public static Exception ExceptNotSupp(string msg, params object[] args);
         public static Exception ExceptParam(this IExceptionContext ctx, string paramName);
         public static Exception ExceptParam(this IExceptionContext ctx, string paramName, string msg);
         public static Exception ExceptParam(this IExceptionContext ctx, string paramName, string msg, params object[] args);
         public static Exception ExceptParam(string paramName);
         public static Exception ExceptParam(string paramName, string msg);
         public static Exception ExceptParam(string paramName, string msg, params object[] args);
         public static Exception ExceptParamValue<T>(this IExceptionContext ctx, T value, string paramName, string msg);
         public static Exception ExceptParamValue<T>(this IExceptionContext ctx, T value, string paramName, string msg, params object[] args);
         public static Exception ExceptParamValue<T>(T value, string paramName, string msg);
         public static Exception ExceptParamValue<T>(T value, string paramName, string msg, params object[] args);
         public static Exception ExceptSchemaMismatch(this IExceptionContext ctx, string paramName, string columnRole, string columnName);
         public static Exception ExceptSchemaMismatch(this IExceptionContext ctx, string paramName, string columnRole, string columnName, string expectedType, string actualType);
         public static Exception ExceptSchemaMismatch(string paramName, string columnRole, string columnName);
         public static Exception ExceptSchemaMismatch(string paramName, string columnRole, string columnName, string expectedType, string actualType);
         public static Exception ExceptUserArg(this IExceptionContext ctx, string name);
         public static Exception ExceptUserArg(this IExceptionContext ctx, string name, string msg);
         public static Exception ExceptUserArg(this IExceptionContext ctx, string name, string msg, params object[] args);
         public static Exception ExceptUserArg(string name);
         public static Exception ExceptUserArg(string name, string msg);
         public static Exception ExceptUserArg(string name, string msg, params object[] args);
         public static Exception ExceptValue(this IExceptionContext ctx, string paramName);
         public static Exception ExceptValue(this IExceptionContext ctx, string paramName, string msg);
         public static Exception ExceptValue(this IExceptionContext ctx, string paramName, string msg, params object[] args);
         public static Exception ExceptValue(string paramName);
         public static Exception ExceptValue(string paramName, string msg);
         public static Exception ExceptValue(string paramName, string msg, params object[] args);
         public static Exception ExceptWhiteSpace(this IExceptionContext ctx, string paramName);
         public static Exception ExceptWhiteSpace(this IExceptionContext ctx, string paramName, string msg);
         public static Exception ExceptWhiteSpace(string paramName);
         public static Exception ExceptWhiteSpace(string paramName, string msg);
         public static bool IsMarked(this Exception ex);
         public static TException Mark<TException>(TException ex) where TException : Exception;
         public static TException MarkSensitive<TException>(this TException ex, MessageSensitivity sensitivity) where TException : Exception;
         public static IExceptionContext NotSensitive();
         public static IExceptionContext NotSensitive(this IExceptionContext ctx);
         public static TException Process<TException>(this TException ex, IExceptionContext ectx=null) where TException : Exception;
         public static IExceptionContext SchemaSensitive();
         public static IExceptionContext SchemaSensitive(this IExceptionContext ctx);
         public static MessageSensitivity Sensitivity(this Exception ex);
         public static Action<string, IExceptionContext> SetAssertHandler(Action<string, IExceptionContext> handler);
         public static IExceptionContext UserSensitive();
         public static IExceptionContext UserSensitive(this IExceptionContext ctx);
     }
     public static class HostExtensions {
         public static T Apply<T>(this IHost host, string channelName, Func<IChannel, T> func);
         public static void Error(this IChannel ch, string fmt);
         public static void Error(this IChannel ch, string fmt, params object[] args);
         public static void Info(this IChannel ch, string fmt);
         public static void Info(this IChannel ch, string fmt, params object[] args);
         public static void Trace(this IChannel ch, string fmt);
         public static void Trace(this IChannel ch, string fmt, params object[] args);
         public static void Warning(this IChannel ch, string fmt);
         public static void Warning(this IChannel ch, string fmt, params object[] args);
     }
     public interface IBulkDistributionPredictor<in TFeatures, in TFeaturesCollection, TResult, TResultCollection, out TResultDistribution, out TResultDistributionCollection> : IBulkPredictor<TFeatures, TFeaturesCollection, TResult, TResultCollection>, IDistPredictorProducing<TResult, TResultDistribution>, IDistributionPredictor<TFeatures, TResult, TResultDistribution>, IPredictor, IPredictor<TFeatures, TResult>, IPredictorProducing<TResult> {
         TResultDistributionCollection BulkPredictDistribution(TFeaturesCollection featuresCollection);
         TResultDistributionCollection BulkPredictDistribution(TFeaturesCollection featuresCollection, out TResultCollection resultCollection);
     }
     public interface IBulkPredictor<in TFeatures, in TFeaturesCollection, out TResult, out TResultCollection> : IPredictor, IPredictor<TFeatures, TResult>, IPredictorProducing<TResult> {
         TResultCollection BulkPredict(TFeaturesCollection featuresCollection);
     }
     public interface IChannel : IDisposable, IExceptionContext, IPipe<ChannelMessage> {
         void Error(MessageSensitivity sensitivity, string fmt);
         void Error(MessageSensitivity sensitivity, string fmt, params object[] args);
         void Info(MessageSensitivity sensitivity, string fmt);
         void Info(MessageSensitivity sensitivity, string fmt, params object[] args);
         void Trace(MessageSensitivity sensitivity, string fmt);
         void Trace(MessageSensitivity sensitivity, string fmt, params object[] args);
         void Warning(MessageSensitivity sensitivity, string fmt);
         void Warning(MessageSensitivity sensitivity, string fmt, params object[] args);
     }
     public interface IChannelProvider : IExceptionContext {
         IChannel Start(string name);
         IPipe<TMessage> StartPipe<TMessage>(string name);
     }
     public interface IComponentFactory
     public interface IComponentFactory<out TComponent> : IComponentFactory {
         TComponent CreateComponent(IHostEnvironment env);
     }
     public interface IComponentFactory<in TArg1, out TComponent> : IComponentFactory {
         TComponent CreateComponent(IHostEnvironment env, TArg1 argument1);
     }
     public interface IComponentFactory<in TArg1, in TArg2, out TComponent> : IComponentFactory {
         TComponent CreateComponent(IHostEnvironment env, TArg1 argument1, TArg2 argument2);
     }
     public interface IComponentFactory<in TArg1, in TArg2, in TArg3, out TComponent> : IComponentFactory {
         TComponent CreateComponent(IHostEnvironment env, TArg1 argument1, TArg2 argument2, TArg3 argument3);
     }
     public interface IDistPredictorProducing<out TResult, out TResultDistribution> : IPredictor, IPredictorProducing<TResult>
     public interface IDistributionPredictor<in TFeatures, TResult, out TResultDistribution> : IDistPredictorProducing<TResult, TResultDistribution>, IPredictor, IPredictor<TFeatures, TResult>, IPredictorProducing<TResult> {
         TResultDistribution PredictDistribution(TFeatures features);
         TResultDistribution PredictDistribution(TFeatures features, out TResult result);
     }
     public interface IExceptionContext {
         string ContextDescription { get; }
         TException Process<TException>(TException ex) where TException : Exception;
     }
     public interface IFileHandle : IDisposable {
         bool CanRead { get; }
         bool CanWrite { get; }
         Stream CreateWriteStream();
         Stream OpenReadStream();
     }
     public interface IHost : IChannelProvider, IExceptionContext, IHostEnvironment, IProgressChannelProvider {
         IRandom Rand { get; }
         void StopExecution();
     }
     public interface IHostEnvironment : IChannelProvider, IExceptionContext, IProgressChannelProvider {
         ComponentCatalog ComponentCatalog { get; }
         int ConcurrencyFactor { get; }
         bool IsCancelled { get; }
         IFileHandle CreateOutputFile(string path);
         IFileHandle CreateTempFile(string suffix=null, string prefix=null);
         IFileHandle OpenInputFile(string path);
         IHost Register(string name, Nullable<int> seed=default(Nullable<int>), Nullable<bool> verbose=default(Nullable<bool>), Nullable<int> conc=default(Nullable<int>));
     }
     public interface IModelCombiner<TModel, TPredictor> where TPredictor : IPredictor {
         TPredictor CombineModels(IEnumerable<TModel> models);
     }
     public interface IParameterValue : IEquatable<IParameterValue> {
         string Name { get; }
         string ValueText { get; }
     }
     public interface IParameterValue<out TValue> : IEquatable<IParameterValue>, IParameterValue {
         TValue Value { get; }
     }
     public interface IPipe<TMessage> : IDisposable, IExceptionContext {
         void Send(TMessage msg);
     }
     public interface IPredictor {
         PredictionKind PredictionKind { get; }
     }
     public interface IPredictor<in TFeatures, out TResult> : IPredictor, IPredictorProducing<TResult> {
         TResult Predict(TFeatures features);
     }
     public interface IPredictorProducing<out TResult> : IPredictor
     public interface IProgressChannel : IDisposable, IProgressChannelProvider {
         void Checkpoint(params Nullable<double>[] values);
         void SetHeader(ProgressHeader header, Action<IProgressEntry> fillAction);
     }
     public interface IProgressChannelProvider {
         IProgressChannel StartProgressChannel(string name);
     }
     public interface IProgressEntry {
         void SetMetric(int index, double value);
         void SetProgress(int index, double value);
         void SetProgress(int index, double value, double lim);
     }
     public interface IRandom {
         int Next();
         int Next(int limit);
         double NextDouble();
         int NextSigned();
         float NextSingle();
     }
     public interface IRunResult : IComparable<IRunResult> {
         bool IsMetricMaximizing { get; }
         IComparable MetricValue { get; }
         ParameterSet ParameterSet { get; }
     }
     public interface IRunResult<T> : IComparable<IRunResult>, IRunResult where T : IComparable<T> {
         new T MetricValue { get; }
     }
     public interface ISweeper {
         ParameterSet[] ProposeSweeps(int maxSweeps, IEnumerable<IRunResult> previousRuns=null);
     }
     public interface ISweepResultEvaluator<in TResults> {
         IRunResult GetRunResult(ParameterSet parameters, TResults results);
     }
     public interface ITrainer {
         TrainerInfo Info { get; }
         PredictionKind PredictionKind { get; }
         IPredictor Train(TrainContext context);
     }
     public interface ITrainer<out TPredictor> : ITrainer where TPredictor : IPredictor {
         new TPredictor Train(TrainContext context);
     }
     public interface ITrainerArguments
     public interface IValueGenerator {
         int Count { get; }
         string Name { get; }
         IParameterValue this[int i] { get; }
         IParameterValue CreateFromNormalized(double normalizedValue);
         string ToStringParameter(IHostEnvironment env);
     }
     public sealed class LoadableClassAttribute : LoadableClassAttributeBase {
         public LoadableClassAttribute(string summary, Type instType, Type argType, Type sigType, string userName, params string[] loadNames);
         public LoadableClassAttribute(string summary, Type instType, Type loaderType, Type argType, Type sigType, string userName, params string[] loadNames);
         public LoadableClassAttribute(string summary, Type instType, Type loaderType, Type argType, Type[] sigTypes, string userName, params string[] loadNames);
         public LoadableClassAttribute(string summary, Type instType, Type argType, Type[] sigTypes, string userName, params string[] loadNames);
         public LoadableClassAttribute(Type instType, Type argType, Type sigType, string userName, params string[] loadNames);
         public LoadableClassAttribute(Type instType, Type loaderType, Type argType, Type sigType, string userName, params string[] loadNames);
         public LoadableClassAttribute(Type instType, Type loaderType, Type argType, Type[] sigTypes, string userName, params string[] loadNames);
         public LoadableClassAttribute(Type instType, Type argType, Type[] sigTypes, string userName, params string[] loadNames);
     }
     public abstract class LoadableClassAttributeBase : Attribute {
         protected LoadableClassAttributeBase(string summary, Type instType, Type loaderType, Type argType, Type[] sigTypes, string userName, params string[] loadNames);
         public Type ArgType { get; private set; }
         public Type[] CtorTypes { get; private set; }
         public string DocName { get; set; }
         public Type InstanceType { get; private set; }
         public Type LoaderType { get; private set; }
         public string[] LoadNames { get; private set; }
         public Type[] SigTypes { get; private set; }
         public string Summary { get; private set; }
         public string UserName { get; private set; }
     }
     public enum MessageSensitivity {
         All = -1,
         None = 0,
         Schema = 2,
         Unknown = -1,
         UserData = 1,
     }
     public sealed class ParameterSet : IEnumerable, IEnumerable<IParameterValue>, IEquatable<ParameterSet> {
         public ParameterSet(Dictionary<string, IParameterValue> paramValues, int hash);
         public ParameterSet(IEnumerable<IParameterValue> parameters);
         public int Count { get; }
         public IParameterValue this[string name] { get; }
         public ParameterSet Clone();
         public bool Equals(ParameterSet other);
         public IEnumerator<IParameterValue> GetEnumerator();
         public override int GetHashCode();
         IEnumerator System.Collections.IEnumerable.GetEnumerator();
         public override string ToString();
     }
     public enum PredictionKind {
         AnomalyDetection = 8,
         BinaryClassification = 2,
         Clustering = 9,
         Custom = 1,
         MultiClassClassification = 3,
         MultiOutputRegression = 5,
         Ranking = 6,
         Recommendation = 7,
         Regression = 4,
         SequenceClassification = 10,
         Unknown = 0,
     }
     public sealed class ProgressHeader {
         public readonly string[] MetricNames;
         public readonly string[] UnitNames;
         public ProgressHeader(params string[] unitNames);
         public ProgressHeader(string[] metricNames, string[] unitNames);
     }
     public static class RandomUtils {
         public static TauswortheHybrid Create();
         public static TauswortheHybrid Create(IRandom seed);
         public static TauswortheHybrid Create(int seed);
         public static TauswortheHybrid Create(Nullable<int> seed);
         public static TauswortheHybrid Create(Nullable<uint> seed);
         public static TauswortheHybrid Create(uint seed);
     }
     public sealed class RunMetric {
         public RunMetric(float primaryMetric, IEnumerable<float> metricDistribution=null);
         public float PrimaryMetric { get; }
         public float[] GetMetricDistribution();
     }
     public sealed class RunResult : IComparable<IRunResult>, IRunResult, IRunResult<double> {
         public RunResult(ParameterSet parameterSet);
         public RunResult(ParameterSet parameterSet, double metricValue, bool isMetricMaximizing);
         public bool HasMetricValue { get; }
         public bool IsMetricMaximizing { get; }
         public double MetricValue { get; }
         IComparable Microsoft.ML.Runtime.IRunResult.MetricValue { get; }
         public ParameterSet ParameterSet { get; }
         public int CompareTo(IRunResult other);
     }
     public sealed class ServerChannel : IDisposable, ServerChannel.IPendingBundleNotification {
         public void Acknowledge(Action<ServerChannel.Bundle> toDo);
         public static ServerChannel.IServerFactory CreateDefaultServerFactoryOrNull(IHostEnvironment env);
         public void Dispose();
         public void Publish();
         public void Register<T1, T2, T3, TRet>(string name, Func<T1, T2, T3, TRet> func);
         public void Register<T1, T2, TRet>(string name, Func<T1, T2, TRet> func);
         public void Register<T1, TRet>(string name, Func<T1, TRet> func);
         public void Register<TRet>(string name, Func<TRet> func);
         public static ServerChannel Start(IChannelProvider provider, string identifier);
         public sealed class Bundle {
             public readonly IReadOnlyDictionary<string, Delegate> NameToFuncs;
             public readonly string Identifier;
             public void AddDoneAction(Action onDone);
         }
         public interface IPendingBundleNotification {
             void Acknowledge(Action<ServerChannel.Bundle> toDo);
         }
         public interface IServer : IDisposable {
             Uri BaseAddress { get; }
         }
         public interface IServerFactory : IComponentFactory, IComponentFactory<IChannel, ServerChannel.IServer> {
             new ServerChannel.IServer CreateComponent(IHostEnvironment env, IChannel ch);
         }
     }
     public static class ServerChannelUtilities {
         public static ServerChannel StartServerChannel(this IChannelProvider provider, string identifier);
     }
     public delegate void SignatureAnomalyDetectorTrainer();
     public delegate void SignatureBinaryClassifierTrainer();
     public delegate void SignatureClusteringTrainer();
     public delegate void SignatureDefault();
     public delegate void SignatureMatrixRecommendingTrainer();
     public delegate void SignatureModelCombiner(PredictionKind kind);
     public delegate void SignatureMultiClassClassifierTrainer();
     public delegate void SignatureMultiOutputRegressorTrainer();
     public delegate void SignatureRankerTrainer();
     public delegate void SignatureRegressorTrainer();
     public delegate void SignatureSequenceTrainer();
     public delegate void SignatureSuggestedSweepsParser();
     public delegate void SignatureSweeper();
     public delegate void SignatureSweepResultEvaluator();
     public delegate void SignatureTrainer();
     public sealed class SimpleFileHandle : IDisposable, IFileHandle {
         public SimpleFileHandle(IExceptionContext ectx, string path, bool needsWrite, bool autoDelete);
         public bool CanRead { get; }
         public bool CanWrite { get; }
         public Stream CreateWriteStream();
         public void Dispose();
         public Stream OpenReadStream();
     }
     public sealed class SysRandom : IRandom {
         public SysRandom();
         public SysRandom(int seed);
         public int Next();
         public int Next(int limit);
         public double NextDouble();
         public int NextSigned();
         public float NextSingle();
         public static SysRandom Wrap(Random rnd);
     }
     public sealed class TauswortheHybrid : IRandom {
         public TauswortheHybrid(IRandom rng);
         public TauswortheHybrid(TauswortheHybrid.State state);
         public TauswortheHybrid.State GetState();
         public int Next();
         public int Next(int limit);
         public double NextDouble();
         public int NextSigned();
         public float NextSingle();
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public struct State {
             public readonly uint U1;
             public readonly uint U2;
             public readonly uint U3;
             public readonly uint U4;
             public State(uint seed);
             public State(uint u1, uint u2, uint u3, uint u4);
             public static TauswortheHybrid.State Load(BinaryReader reader);
             public void Save(BinaryWriter writer);
         }
     }
     public sealed class TelemetryException : TelemetryMessage {
         public readonly Exception Exception;
         public TelemetryException(Exception exception);
     }
     public abstract class TelemetryMessage {
         protected TelemetryMessage();
         public static TelemetryMessage CreateCommand(string commandName, string commandText);
         public static TelemetryMessage CreateException(Exception exception);
         public static TelemetryMessage CreateMetric(string metricName, double metricValue, Dictionary<string, string> properties=null);
         public static TelemetryMessage CreateTrainer(string trainerName, string trainerParams);
         public static TelemetryMessage CreateTransform(string transformName, string transformParams);
     }
     public sealed class TelemetryMetric : TelemetryMessage {
         public readonly IDictionary<string, string> Properties;
         public readonly double Value;
         public readonly string Name;
         public TelemetryMetric(string name, double value, IDictionary<string, string> properties=null);
     }
     public sealed class TelemetryTrace : TelemetryMessage {
         public readonly string Name;
         public readonly string Text;
         public readonly string Type;
         public TelemetryTrace(string text, string name, string type);
     }
     public sealed class TrainContext {
         public TrainContext(RoleMappedData trainingSet, RoleMappedData validationSet=null, IPredictor initialPredictor=null);
         public IPredictor InitialPredictor { get; }
         public RoleMappedData TrainingSet { get; }
         public RoleMappedData ValidationSet { get; }
     }
     public static class TrainerExtensions {
         public static IPredictor Train(this ITrainer trainer, RoleMappedData trainData);
         public static TPredictor Train<TPredictor>(this ITrainer<TPredictor> trainer, RoleMappedData trainData) where TPredictor : IPredictor;
     }
     public sealed class TrainerInfo {
         public TrainerInfo(bool normalization=true, bool calibration=false, bool caching=true, bool supportValid=false, bool supportIncrementalTrain=false);
         public bool NeedCalibration { get; }
         public bool NeedNormalization { get; }
         public bool SupportsIncrementalTraining { get; }
         public bool SupportsValidation { get; }
         public bool WantCaching { get; }
     }
 }
```

## Microsoft.ML.Runtime.Command

```C#
 namespace Microsoft.ML.Runtime.Command {
     public interface ICommand {
         void Run();
     }
     public delegate void SignatureCommand();
 }
```

## Microsoft.ML.Runtime.CommandLine

```C#
 namespace Microsoft.ML.Runtime.CommandLine {
     public class ArgumentAttribute : Attribute {
         public ArgumentAttribute(ArgumentType type);
         public string[] Aliases { get; }
         public string HelpText { get; set; }
         public bool Hide { get; set; }
         public bool IsInputFileName { get; set; }
         public bool IsRequired { get; }
         public string Name { get; set; }
         public string NullName { get; set; }
         public string Purpose { get; set; }
         public string ShortName { get; set; }
         public Type SignatureType { get; set; }
         public double SortOrder { get; set; }
         public ArgumentType Type { get; }
         public ArgumentAttribute.VisibilityType Visibility { get; set; }
         public enum VisibilityType {
             CmdLineOnly = 1,
             EntryPointsOnly = 2,
             Everywhere = 0,
         }
     }
     public enum ArgumentType {
         AtMostOnce = 0,
         LastOccurenceWins = 4,
         Multiple = 4,
         MultipleUnique = 6,
         Required = 1,
         Unique = 2,
     }
     public sealed class CharCursor {
         public CharCursor(string text);
         public CharCursor(string text, int min, int lim);
         public char ChCur { get; }
         public bool Eof { get; }
         public int IchCur { get; }
         public char ChNext();
         public char ChPeek(int dich);
         public string GetRest();
     }
     public sealed class CmdLexer {
         public CmdLexer(CharCursor curs, bool escapes=true);
         public bool Error { get; }
         public void GetToken(StringBuilder bldr);
         public void SkipWhiteSpace();
         public static string UnquoteValue(string str);
     }
     public sealed class CmdParser {
         public static string ArgumentsUsage(IHostEnvironment env, Type type, object defaults, bool showRsp=false, Nullable<int> columns=default(Nullable<int>));
         public static string CombineSettings(string[] settings);
         public static ICommandLineComponentFactory CreateComponentFactory(Type factoryType, Type signatureType, string settings);
         public static CmdParser.ArgInfo GetArgInfo(Type type, object defaults);
         public static IEnumerable<KeyValuePair<string, string>> GetSettingPairs(IHostEnvironment env, object values, SettingsFlags flags=(SettingsFlags)(0));
         public static IEnumerable<KeyValuePair<string, string>> GetSettingPairs(IHostEnvironment env, object values, object defaults, SettingsFlags flags=(SettingsFlags)(0));
         public static string GetSettings(IHostEnvironment env, object values, object defaults, SettingsFlags flags=(SettingsFlags)(3));
         public static bool IsNumericType(Type type);
         public static bool LexString(string text, out string[] arguments);
         public static bool ParseArguments(IHostEnvironment env, string settings, object destination);
         public static bool ParseArguments(IHostEnvironment env, string settings, object destination, ErrorReporter reporter);
         public static bool ParseArguments(IHostEnvironment env, string settings, object destination, ErrorReporter reporter, out string helpText);
         public static bool ParseArguments(IHostEnvironment env, string settings, object destination, out string helpText);
         public static bool ParseArguments(IHostEnvironment env, string settings, object destination, Type destinationType, ErrorReporter reporter);
         public static string TrimExePath(string args, out string exe);
         public static bool TryGetFirstToken(string text, out string token, out string rest);
         public sealed class ArgInfo {
             public readonly CmdParser.ArgInfo.Arg[] Args;
             public bool TryGetArg(string name, out CmdParser.ArgInfo.Arg arg);
             public sealed class Arg {
                 public object DefaultValue { get; }
                 public FieldInfo Field { get; }
                 public string HelpText { get; }
                 public int Index { get; }
                 public bool IsCollection { get; }
                 public bool IsDefault { get; }
                 public bool IsHidden { get; }
                 public bool IsRequired { get; }
                 public bool IsTaggedCollection { get; }
                 public Type ItemType { get; }
                 public string ItemTypeString { get; }
                 public Type ItemValueType { get; }
                 public ArgumentType Kind { get; }
                 public string LongName { get; }
                 public string NullName { get; }
                 public string[] ShortNames { get; }
                 public double SortOrder { get; }
             }
         }
     }
     public sealed class CmdQuoter {
         public static bool NeedsQuoting(string str);
         public static bool NeedsQuoting(StringBuilder sb, int ich);
         public static bool QuoteValue(string str, StringBuilder sb, bool force=false);
     }
     public class DefaultArgumentAttribute : ArgumentAttribute {
         public DefaultArgumentAttribute(ArgumentType type);
     }
     public class EnumValueDisplayAttribute : Attribute {
         public readonly string Name;
         public EnumValueDisplayAttribute(string name);
     }
     public delegate void ErrorReporter(string message);
     public class HideEnumValueAttribute : Attribute {
         public HideEnumValueAttribute();
     }
     public interface ICommandLineComponentFactory : IComponentFactory {
         string Name { get; }
         Type SignatureType { get; }
         string GetSettingsString();
     }
     public enum SettingsFlags {
         Default = 3,
         None = 0,
         NoSlashes = 2,
         NoUnparse = 4,
         ShortNames = 1,
     }
     public static class SpecialPurpose {
         public const string ColumnName = "ColumnName";
         public const string ColumnSelector = "ColumnSelector";
         public const string MultilineText = "MultilineText";
     }
 }
```

## Microsoft.ML.Runtime.Data

```C#
 namespace Microsoft.ML.Runtime.Data {
     public sealed class BoolType : PrimitiveType {
         public static BoolType Instance { get; }
         public override bool Equals(ColumnType other);
         public override string ToString();
     }
     public abstract class ChannelProviderBase : IExceptionContext {
         protected ChannelProviderBase(string shortName, string parentFullName, bool verbose);
         public virtual string ContextDescription { get; }
         public abstract int Depth { get; }
         public string FullName { get; }
         public string ParentFullName { get; }
         public string ShortName { get; }
         public bool Verbose { get; }
         protected virtual string GenerateFullName();
         public virtual TException Process<TException>(TException ex) where TException : Exception;
         public static class ExceptionContextKeys {
             public const string ParentComponent = "Parent component";
             public const string Phase = "Phase";
             public const string ThrowingComponent = "Throwing component";
         }
     }
     public sealed class ColumnInfo {
         public readonly ColumnType Type;
         public readonly int Index;
         public readonly string Name;
         public static ColumnInfo CreateFromIndex(ISchema schema, int index);
         public static ColumnInfo CreateFromName(ISchema schema, string name, string descName);
         public static bool TryCreateFromName(ISchema schema, string name, out ColumnInfo colInfo);
     }
     public abstract class ColumnType : IEquatable<ColumnType> {
         protected ColumnType(Type rawType);
         public KeyType AsKey { get; }
         public PrimitiveType AsPrimitive { get; }
         public VectorType AsVector { get; }
         public bool IsBool { get; }
         public bool IsDateTime { get; }
         public bool IsDateTimeZone { get; }
         public bool IsKey { get; }
         public bool IsKnownSizeVector { get; }
         public bool IsNumber { get; }
         public bool IsPrimitive { get; }
         public bool IsStandardScalar { get; }
         public bool IsText { get; }
         public bool IsTimeSpan { get; }
         public bool IsVector { get; }
         public ColumnType ItemType { get; }
         public int KeyCount { get; }
         public DataKind RawKind { get; }
         public Type RawType { get; }
         public int ValueCount { get; }
         public int VectorSize { get; }
         public abstract bool Equals(ColumnType other);
         public bool SameSizeAndItemType(ColumnType other);
     }
     public sealed class ConsoleEnvironment : HostEnvironmentBase<ConsoleEnvironment> {
         public const string ComponentHistoryKey = "ComponentHistory";
         public ConsoleEnvironment(Nullable<int> seed=default(Nullable<int>), bool verbose=false, MessageSensitivity sensitivity=(MessageSensitivity)(-1), int conc=0, TextWriter outWriter=null, TextWriter errWriter=null);
         protected override IChannel CreateCommChannel(ChannelProviderBase parent, string name);
         protected override IPipe<TMessage> CreatePipe<TMessage>(ChannelProviderBase parent, string name);
         protected override IFileHandle CreateTempFileCore(IHostEnvironment env, string suffix=null, string prefix=null);
         public void PrintProgress();
         protected override IHost RegisterCore(HostEnvironmentBase<ConsoleEnvironment> source, string shortName, string parentFullName, IRandom rand, bool verbose, Nullable<int> conc);
     }
     public enum CursorState {
         Done = 2,
         Good = 1,
         NotStarted = 0,
     }
     public enum DataKind : byte {
         BL = (byte)12,
         Bool = (byte)12,
         DateTime = (byte)14,
         DateTimeZone = (byte)15,
         DT = (byte)14,
         DZ = (byte)15,
         I1 = (byte)1,
         I2 = (byte)3,
         I4 = (byte)5,
         I8 = (byte)7,
         Num = (byte)9,
         R4 = (byte)9,
         R8 = (byte)10,
         Text = (byte)11,
         TimeSpan = (byte)13,
         TS = (byte)13,
         TX = (byte)11,
         TXT = (byte)11,
         U1 = (byte)2,
         U16 = (byte)16,
         U2 = (byte)4,
         U4 = (byte)6,
         U8 = (byte)8,
         UG = (byte)16,
     }
     public static class DataKindExtensions {
         public const DataKind KindLim = (byte)17;
         public const DataKind KindMin = (byte)1;
         public const int KindCount = 16;
         public static DataKind FromIndex(int index);
         public static string GetString(this DataKind kind);
         public static int ToIndex(this DataKind kind);
         public static ulong ToMaxInt(this DataKind kind);
         public static long ToMinInt(this DataKind kind);
         public static Type ToType(this DataKind kind);
         public static bool TryGetDataKind(this Type type, out DataKind kind);
     }
     public sealed class DateTimeOffsetType : PrimitiveType {
         public static DateTimeOffsetType Instance { get; }
         public override bool Equals(ColumnType other);
         public override string ToString();
     }
     public sealed class DateTimeType : PrimitiveType {
         public static DateTimeType Instance { get; }
         public override bool Equals(ColumnType other);
         public override string ToString();
     }
     public abstract class HostEnvironmentBase<TEnv> : ChannelProviderBase, IChannelProvider, IDisposable, IExceptionContext, IHostEnvironment, IMessageDispatcher, IProgressChannelProvider where TEnv : HostEnvironmentBase<TEnv> {
         protected readonly HostEnvironmentBase<TEnv> Master;
         protected readonly ProgressReporting.ProgressTracker ProgressTracker;
         protected readonly ConcurrentDictionary<Type, HostEnvironmentBase<TEnv>.Dispatcher> ListenerDict;
         protected readonly TEnv Root;
         protected HostEnvironmentBase(HostEnvironmentBase<TEnv> source, IRandom rand, bool verbose, Nullable<int> conc, string shortName=null, string parentFullName=null);
         protected HostEnvironmentBase(IRandom rand, bool verbose, int conc, string shortName=null, string parentFullName=null);
         public ComponentCatalog ComponentCatalog { get; }
         public int ConcurrencyFactor { get; set; }
         public override string ContextDescription { get; }
         public override int Depth { get; }
         public bool IsCancelled { get; protected set; }
         protected bool IsDisposed { get; }
         public void AddListener<TMessage>(Action<IMessageSource, TMessage> listenerFunc);
         protected IFileHandle CreateAndRegisterTempFile(IHostEnvironment env, string suffix=null, string prefix=null);
         protected abstract IChannel CreateCommChannel(ChannelProviderBase parent, string name);
         public IFileHandle CreateOutputFile(string path);
         protected virtual IFileHandle CreateOutputFileCore(IHostEnvironment env, string path);
         protected abstract IPipe<TMessage> CreatePipe<TMessage>(ChannelProviderBase parent, string name);
         public IFileHandle CreateTempFile(string suffix=null, string prefix=null);
         protected virtual IFileHandle CreateTempFileCore(IHostEnvironment env, string suffix=null, string prefix=null);
         public virtual void Dispose();
         protected HostEnvironmentBase<TEnv>.Dispatcher<TMessage> EnsureDispatcher<TMessage>();
         protected Action<IMessageSource, TMessage> GetDispatchDelegate<TMessage>();
         protected virtual bool HasBadFileCharacters(string str=null);
         public IFileHandle OpenInputFile(string path);
         protected virtual IFileHandle OpenInputFileCore(IHostEnvironment env, string path);
         public virtual void PrintMessageNormalized(TextWriter writer, string message, bool removeLastNewLine, string prefix=null);
         public override TException Process<TException>(TException ex);
         public IHost Register(string name, Nullable<int> seed=default(Nullable<int>), Nullable<bool> verbose=default(Nullable<bool>), Nullable<int> conc=default(Nullable<int>));
         protected abstract IHost RegisterCore(HostEnvironmentBase<TEnv> source, string shortName, string parentFullName, IRandom rand, bool verbose, Nullable<int> conc);
         public void RemoveListener<TMessage>(Action<IMessageSource, TMessage> listenerFunc);
         public IChannel Start(string name);
         public IPipe<TMessage> StartPipe<TMessage>(string name);
         public IProgressChannel StartProgressChannel(string name);
         protected virtual IProgressChannel StartProgressChannelCore(HostEnvironmentBase<TEnv>.HostBase host, string name);
         protected abstract class ChannelBase : HostEnvironmentBase<TEnv>.PipeBase<ChannelMessage>, IChannel, IDisposable, IExceptionContext, IPipe<ChannelMessage> {
             protected readonly TEnv Root;
             protected ChannelBase(TEnv root, ChannelProviderBase parent, string shortName, Action<IMessageSource, ChannelMessage> dispatch);
             public void Error(MessageSensitivity sensitivity, string msg);
             public void Error(MessageSensitivity sensitivity, string fmt, params object[] args);
             public void Info(MessageSensitivity sensitivity, string msg);
             public void Info(MessageSensitivity sensitivity, string fmt, params object[] args);
             public void Trace(MessageSensitivity sensitivity, string msg);
             public void Trace(MessageSensitivity sensitivity, string fmt, params object[] args);
             public void Warning(MessageSensitivity sensitivity, string msg);
             public void Warning(MessageSensitivity sensitivity, string fmt, params object[] args);
         }
         protected abstract class Dispatcher {
             protected Dispatcher();
         }
         protected sealed class Dispatcher<TMessage> : HostEnvironmentBase<TEnv>.Dispatcher {
             public Dispatcher();
             public Action<IMessageSource, TMessage> Dispatch { get; }
             public void AddListener(Action<IMessageSource, TMessage> listenerFunc);
             public void RemoveListener(Action<IMessageSource, TMessage> listenerFunc);
         }
         public abstract class HostBase : HostEnvironmentBase<TEnv>, IChannelProvider, IExceptionContext, IHost, IHostEnvironment, IProgressChannelProvider {
             public HostBase(HostEnvironmentBase<TEnv> source, string shortName, string parentFullName, IRandom rand, bool verbose, Nullable<int> conc);
             public override int Depth { get; }
             public IRandom Rand { get; }
             public new IHost Register(string name, Nullable<int> seed=default(Nullable<int>), Nullable<bool> verbose=default(Nullable<bool>), Nullable<int> conc=default(Nullable<int>));
             public void StopExecution();
         }
         protected sealed class Pipe<TMessage> : HostEnvironmentBase<TEnv>.PipeBase<TMessage> {
             public Pipe(ChannelProviderBase parent, string shortName, Action<IMessageSource, TMessage> dispatch);
         }
         protected abstract class PipeBase<TMessage> : ChannelProviderBase, IDisposable, IExceptionContext, IMessageSource, IPipe<TMessage> {
             public readonly ChannelProviderBase Parent;
             protected readonly Action<IMessageSource, TMessage> Dispatch;
             protected PipeBase(ChannelProviderBase parent, string shortName, Action<IMessageSource, TMessage> dispatch);
             public override int Depth { get; }
             public void Dispose();
             protected virtual void DisposeCore();
             public override TException Process<TException>(TException ex);
             public void Send(TMessage msg);
         }
     }
     public interface ICounted {
         long Batch { get; }
         long Position { get; }
         ValueGetter<UInt128> GetIdGetter();
     }
     public interface ICursor : ICounted, IDisposable {
         CursorState State { get; }
         ICursor GetRootCursor();
         bool MoveMany(long count);
         bool MoveNext();
     }
     public interface IDataView : ISchematized {
         bool CanShuffle { get; }
         Nullable<long> GetRowCount(bool lazy=true);
         IRowCursor GetRowCursor(Func<int, bool> needCol, IRandom rand=null);
         IRowCursor[] GetRowCursorSet(out IRowCursorConsolidator consolidator, Func<int, bool> needCol, int n, IRandom rand=null);
     }
     public interface IMessageDispatcher : IChannelProvider, IExceptionContext, IHostEnvironment, IProgressChannelProvider {
         void AddListener<TMessage>(Action<IMessageSource, TMessage> listenerFunc);
         void RemoveListener<TMessage>(Action<IMessageSource, TMessage> listenerFunc);
     }
     public interface IMessageSource {
         string FullName { get; }
         string ShortName { get; }
         bool Verbose { get; }
     }
     public interface IRow : ICounted, ISchematized {
         ValueGetter<TValue> GetGetter<TValue>(int col);
         bool IsColumnActive(int col);
     }
     public interface IRowCursor : ICounted, ICursor, IDisposable, IRow, ISchematized
     public interface IRowCursorConsolidator {
         IRowCursor CreateCursor(IChannelProvider provider, IRowCursor[] inputs);
     }
     public interface IRowToRowMapper : ISchematized {
         Schema InputSchema { get; }
         Func<int, bool> GetDependencies(Func<int, bool> predicate);
         IRow GetRow(IRow input, Func<int, bool> active, out Action disposer);
     }
     public interface ISchema {
         int ColumnCount { get; }
         string GetColumnName(int col);
         ColumnType GetColumnType(int col);
         void GetMetadata<TValue>(string kind, int col, ref TValue value);
         ColumnType GetMetadataTypeOrNull(string kind, int col);
         IEnumerable<KeyValuePair<string, ColumnType>> GetMetadataTypes(int col);
         bool TryGetColumnIndex(string name, out int col);
     }
     public interface ISchemaBindableMapper {
         ISchemaBoundMapper Bind(IHostEnvironment env, RoleMappedSchema schema);
     }
     public interface ISchemaBoundMapper : ISchematized {
         ISchemaBindableMapper Bindable { get; }
         RoleMappedSchema InputRoleMappedSchema { get; }
         IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> GetInputColumnRoles();
     }
     public interface ISchemaBoundRowMapper : IRowToRowMapper, ISchemaBoundMapper, ISchematized
     public interface ISchematized {
         Schema Schema { get; }
     }
     public interface IValueMapper {
         ColumnType InputType { get; }
         ColumnType OutputType { get; }
         ValueMapper<TSrc, TDst> GetMapper<TSrc, TDst>();
     }
     public interface IValueMapperDist : IValueMapper {
         ColumnType DistType { get; }
         ValueMapper<TSrc, TDst, TDist> GetMapper<TSrc, TDst, TDist>();
     }
     public sealed class KeyType : PrimitiveType {
         public KeyType(DataKind kind, ulong min, int count, bool contiguous=true);
         public bool Contiguous { get; }
         public int Count { get; }
         public ulong Min { get; }
         public override bool Equals(ColumnType other);
         public override bool Equals(object other);
         public override int GetHashCode();
         public static bool IsValidDataKind(DataKind kind);
         public override string ToString();
     }
     public abstract class LinkedRootCursorBase<TInput> : RootCursorBase where TInput : class, ICursor {
         protected LinkedRootCursorBase(IChannelProvider provider, TInput input);
         protected TInput Input { get; }
         protected ICursor Root { get; }
         public override void Dispose();
     }
     public abstract class LinkedRowFilterCursorBase : LinkedRowRootCursorBase {
         protected LinkedRowFilterCursorBase(IChannelProvider provider, IRowCursor input, Schema schema, bool[] active);
         public override long Batch { get; }
         protected abstract bool Accept();
         public override ValueGetter<UInt128> GetIdGetter();
         protected override bool MoveNextCore();
     }
     public abstract class LinkedRowRootCursorBase : LinkedRootCursorBase<IRowCursor>, ICounted, ICursor, IDisposable, IRow, IRowCursor, ISchematized {
         protected LinkedRowRootCursorBase(IChannelProvider provider, IRowCursor input, Schema schema, bool[] active);
         public Schema Schema { get; }
         public virtual ValueGetter<TValue> GetGetter<TValue>(int col);
         public bool IsColumnActive(int col);
     }
     public static class MetadataUtils {
         public static ColumnType ScoreColumnSetIdType { get; }
         public static Exception ExceptGetMetadata();
         public static Exception ExceptGetMetadata(this IExceptionContext ctx);
         public static VectorType GetCategoricalType(int rangeCount);
         public static IEnumerable<int> GetColumnSet(this Schema schema, string metadataKind, string value);
         public static IEnumerable<int> GetColumnSet(this Schema schema, string metadataKind, uint value);
         public static KeyValuePair<string, ColumnType> GetKeyNamesPair(int size);
         public static uint GetMaxMetadataKind(this Schema schema, out int colMax, string metadataKind, Func<Schema, int, bool> filterFunc=null);
         public static VectorType GetNamesType(int size);
         public static KeyValuePair<string, ColumnType> GetPair(this ColumnType type, string kind);
         public static void GetSlotNames(RoleMappedSchema schema, RoleMappedSchema.ColumnRole role, int vectorSize, ref VBuffer<ReadOnlyMemory<char>> slotNames);
         public static KeyValuePair<string, ColumnType> GetSlotNamesPair(int size);
         public static IEnumerable<SchemaShape.Column> GetTrainerOutputMetadata(bool isNormalized=false);
         public static bool HasKeyNames(this Schema schema, int col, int keyCount);
         public static bool HasSlotNames(this SchemaShape.Column col);
         public static bool HasSlotNames(this Schema schema, int col, int vectorSize);
         public static bool IsHidden(this Schema schema, int col);
         public static bool IsNormalized(this SchemaShape.Column col);
         public static bool IsNormalized(this Schema schema, int col);
         public static void Marshal<THave, TNeed>(this MetadataUtils.MetadataGetter<THave> getter, int col, ref TNeed dst);
         public static IEnumerable<T> Prepend<T>(this IEnumerable<T> tail, params T[] head);
         public static bool TryGetCategoricalFeatureIndices(Schema schema, int colIndex, out int[] categoricalFeatures);
         public static bool TryGetMetadata<T>(this Schema schema, PrimitiveType type, string kind, int col, ref T value);
         public static class Const {
             public static class ScoreColumnKind {
                 public const string AnomalyDetection = "AnomalyDetection";
                 public const string BinaryClassification = "BinaryClassification";
                 public const string Clustering = "Clustering";
                 public const string ItemSimilarity = "ItemSimilarity";
                 public const string MultiClassClassification = "MultiClassClassification";
                 public const string MultiOutputRegression = "MultiOutputRegression";
                 public const string QuantileRegression = "QuantileRegression";
                 public const string Ranking = "Ranking";
                 public const string Recommender = "Recommender";
                 public const string Regression = "Regression";
                 public const string SequenceClassification = "SequenceClassification";
                 public const string WhatTheFeature = "WhatTheFeature";
             }
             public static class ScoreValueKind {
                 public const string PredictedLabel = "PredictedLabel";
                 public const string Probability = "Probability";
                 public const string Score = "Score";
             }
         }
         public static class Kinds {
             public const string CategoricalSlotRanges = "CategoricalSlotRanges";
             public const string HasMissingValues = "HasMissingValues";
             public const string IsNormalized = "IsNormalized";
             public const string IsUserVisible = "IsUserVisible";
             public const string KeyValues = "KeyValues";
             public const string ScoreColumnKind = "ScoreColumnKind";
             public const string ScoreColumnSetId = "ScoreColumnSetId";
             public const string ScoreValueKind = "ScoreValueKind";
             public const string SlotNames = "SlotNames";
             public const string TrainingLabelValues = "TrainingLabelValues";
         }
         public delegate void MetadataGetter<TValue>(int col, ref TValue dst);
     }
     public sealed class NumberType : PrimitiveType {
         public static NumberType Float { get; }
         public static NumberType I1 { get; }
         public static NumberType I2 { get; }
         public static NumberType I4 { get; }
         public static NumberType I8 { get; }
         public static NumberType R4 { get; }
         public static NumberType R8 { get; }
         public static NumberType U1 { get; }
         public static NumberType U2 { get; }
         public static NumberType U4 { get; }
         public static NumberType U8 { get; }
         public static NumberType UG { get; }
         public override bool Equals(ColumnType other);
         public static new NumberType FromKind(DataKind kind);
         public static NumberType FromType(Type type);
         public override string ToString();
     }
     public abstract class PrimitiveType : ColumnType {
         protected PrimitiveType(Type rawType);
         public static PrimitiveType FromKind(DataKind kind);
     }
     public static class ProgressReporting {
         public sealed class ProgressChannel : IDisposable, IProgressChannel, IProgressChannelProvider {
             public ProgressChannel(IExceptionContext ectx, ProgressReporting.ProgressTracker tracker, string computationName);
             public string Name { get; }
             public void Checkpoint(params Nullable<double>[] values);
             public void Dispose();
             public ProgressReporting.ProgressEntry GetProgress();
             public void SetHeader(ProgressHeader header, Action<IProgressEntry> fillAction);
             public IProgressChannel StartProgressChannel(string name);
         }
         public sealed class ProgressEntry : IProgressEntry {
             public readonly ProgressHeader Header;
             public readonly bool IsCheckpoint;
             public readonly Nullable<double>[] Metrics;
             public readonly Nullable<double>[] Progress;
             public readonly Nullable<double>[] ProgressLim;
             public ProgressEntry(bool isCheckpoint, ProgressHeader header);
             public void SetMetric(int index, double value);
             public void SetProgress(int index, double value);
             public void SetProgress(int index, double value, double lim);
         }
         public sealed class ProgressEvent {
             public readonly ProgressReporting.ProgressEvent.EventKind Kind;
             public readonly ProgressReporting.ProgressEntry ProgressEntry;
             public readonly DateTime EventTime;
             public readonly DateTime StartTime;
             public readonly int Index;
             public readonly string Name;
             public ProgressEvent(int index, string name, DateTime startTime, ProgressReporting.ProgressEvent.EventKind kind);
             public ProgressEvent(int index, string name, DateTime startTime, ProgressReporting.ProgressEntry entry);
             public enum EventKind {
                 Progress = 1,
                 Start = 0,
                 Stop = 2,
             }
         }
         public sealed class ProgressTracker {
             public ProgressTracker(IExceptionContext ectx);
             public List<ProgressReporting.ProgressEvent> GetAllProgress();
             public void Log(ProgressReporting.ProgressChannel source, ProgressReporting.ProgressEvent.EventKind kind, ProgressReporting.ProgressEntry entry);
         }
     }
     public static class ReadOnlyMemoryUtils {
         public static void AddLowerCaseToStringBuilder(ReadOnlySpan<char> span, StringBuilder sb);
         public static NormStr AddToPool(ReadOnlyMemory<char> memory, NormStr.Pool pool);
         public static StringBuilder AppendMemory(this StringBuilder sb, ReadOnlyMemory<char> memory);
         public static StringBuilder AppendSpan(this StringBuilder sb, ReadOnlySpan<char> span);
         public static bool EqualsStr(string s, ReadOnlyMemory<char> memory);
         public static NormStr FindInPool(ReadOnlyMemory<char> memory, NormStr.Pool pool);
         public static IEnumerable<ReadOnlyMemory<char>> Split(ReadOnlyMemory<char> memory, char[] separators);
         public static bool SplitOne(ReadOnlyMemory<char> memory, char separator, out ReadOnlyMemory<char> left, out ReadOnlyMemory<char> right);
         public static bool SplitOne(ReadOnlyMemory<char> memory, char[] separators, out ReadOnlyMemory<char> left, out ReadOnlyMemory<char> right);
         public static ReadOnlyMemory<char> TrimEndWhiteSpace(ReadOnlyMemory<char> memory);
         public static ReadOnlyMemory<char> TrimSpaces(ReadOnlyMemory<char> memory);
         public static ReadOnlyMemory<char> TrimWhiteSpace(ReadOnlyMemory<char> memory);
     }
     public delegate bool RefPredicate<T>(ref T value);
     public sealed class RoleMappedData {
         public RoleMappedData(IDataView data, bool opt=false, params KeyValuePair<RoleMappedSchema.ColumnRole, string>[] roles);
         public RoleMappedData(IDataView data, IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> roles, bool opt=false);
         public RoleMappedData(IDataView data, string label, string feature, string group=null, string weight=null, string name=null, IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> custom=null, bool opt=false);
         public IDataView Data { get; }
         public RoleMappedSchema Schema { get; }
     }
     public sealed class RoleMappedSchema {
         public RoleMappedSchema(Schema schema, bool opt=false, params KeyValuePair<RoleMappedSchema.ColumnRole, string>[] roles);
         public RoleMappedSchema(Schema schema, IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> roles, bool opt=false);
         public RoleMappedSchema(Schema schema, string label, string feature, string group=null, string weight=null, string name=null, IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> custom=null, bool opt=false);
         public ColumnInfo Feature { get; }
         public ColumnInfo Group { get; }
         public ColumnInfo Label { get; }
         public ColumnInfo Name { get; }
         public Schema Schema { get; }
         public ColumnInfo Weight { get; }
         public static KeyValuePair<RoleMappedSchema.ColumnRole, string> CreatePair(RoleMappedSchema.ColumnRole role, string name);
         public IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> GetColumnRoleNames();
         public IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, string>> GetColumnRoleNames(RoleMappedSchema.ColumnRole role);
         public IEnumerable<KeyValuePair<RoleMappedSchema.ColumnRole, ColumnInfo>> GetColumnRoles();
         public IReadOnlyList<ColumnInfo> GetColumns(RoleMappedSchema.ColumnRole role);
         public ColumnInfo GetUniqueColumn(RoleMappedSchema.ColumnRole role);
         public bool Has(RoleMappedSchema.ColumnRole role);
         public bool HasMultiple(RoleMappedSchema.ColumnRole role);
         public bool HasUnique(RoleMappedSchema.ColumnRole role);
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public struct ColumnRole {
             public readonly string Value;
             public ColumnRole(string value);
             public static RoleMappedSchema.ColumnRole Feature { get; }
             public static RoleMappedSchema.ColumnRole FeatureContributions { get; }
             public static RoleMappedSchema.ColumnRole Group { get; }
             public static RoleMappedSchema.ColumnRole Label { get; }
             public static RoleMappedSchema.ColumnRole Name { get; }
             public static RoleMappedSchema.ColumnRole Weight { get; }
             public KeyValuePair<RoleMappedSchema.ColumnRole, string> Bind(string name);
             public static implicit operator RoleMappedSchema.ColumnRole (string value);
         }
     }
     public abstract class RootCursorBase : ICounted, ICursor, IDisposable {
         protected readonly IChannel Ch;
         protected RootCursorBase(IChannelProvider provider);
         public abstract long Batch { get; }
         protected bool IsGood { get; }
         public long Position { get; private set; }
         public CursorState State { get; private set; }
         public virtual void Dispose();
         public abstract ValueGetter<UInt128> GetIdGetter();
         public ICursor GetRootCursor();
         public bool MoveMany(long count);
         protected virtual bool MoveManyCore(long count);
         public bool MoveNext();
         protected abstract bool MoveNextCore();
     }
     public sealed class Schema : ISchema {
         public Schema(IEnumerable<Schema.Column> columns);
         public int ColumnCount { get; }
         public Schema.Column this[int col] { get; }
         public Schema.Column this[string name] { get; }
         public static Schema Create(ISchema inputSchema);
         public string GetColumnName(int col);
         public Schema.Column GetColumnOrNull(string name);
         public IEnumerable<ValueTuple<int, Schema.Column>> GetColumns();
         public ColumnType GetColumnType(int col);
         public void GetMetadata<TValue>(string kind, int col, ref TValue value);
         public ColumnType GetMetadataTypeOrNull(string kind, int col);
         public IEnumerable<KeyValuePair<string, ColumnType>> GetMetadataTypes(int col);
         public bool TryGetColumnIndex(string name, out int col);
         public sealed class Column {
             public Column(string name, ColumnType type, Schema.Metadata metadata);
             public Schema.Metadata Metadata { get; }
             public string Name { get; }
             public ColumnType Type { get; }
         }
         public sealed class Metadata {
             public Metadata(IEnumerable<ValueTuple<Schema.Column, Delegate>> values);
             public Schema Schema { get; }
             public ValueGetter<TValue> GetGetter<TValue>(int col);
             public void GetValue<TValue>(string kind, ref TValue value);
             public sealed class Builder {
                 public Builder();
                 public void Add(Schema.Column column, Delegate getter);
                 public void Add(Schema.Metadata metadata, Func<string, bool> selector);
                 public void Add<TValue>(Schema.Column column, ValueGetter<TValue> getter);
                 public void AddKeyValues<TValue>(int size, PrimitiveType valueType, ValueGetter<VBuffer<TValue>> getter);
                 public void AddSlotNames(int size, ValueGetter<VBuffer<ReadOnlyMemory<char>>> getter);
                 public Schema.Metadata GetMetadata();
             }
         }
     }
     public abstract class StructuredType : ColumnType {
         protected StructuredType(Type rawType);
     }
     public abstract class SynchronizedCursorBase<TBase> : ICounted, ICursor, IDisposable where TBase : class, ICursor {
         protected readonly IChannel Ch;
         protected SynchronizedCursorBase(IChannelProvider provider, TBase input);
         public long Batch { get; }
         protected TBase Input { get; }
         protected bool IsGood { get; }
         public long Position { get; }
         public CursorState State { get; }
         public virtual void Dispose();
         public ValueGetter<UInt128> GetIdGetter();
         public ICursor GetRootCursor();
         public bool MoveMany(long count);
         public bool MoveNext();
     }
     public sealed class TextType : PrimitiveType {
         public static TextType Instance { get; }
         public override bool Equals(ColumnType other);
         public override string ToString();
     }
     public sealed class TimeSpanType : PrimitiveType {
         public static TimeSpanType Instance { get; }
         public override bool Equals(ColumnType other);
         public override string ToString();
     }
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct UInt128 : IComparable<UInt128>, IEquatable<UInt128> {
         public readonly ulong Hi;
         public readonly ulong Lo;
         public UInt128(ulong lo, ulong hi);
         public UInt128 Combine(UInt128 other);
         public int CompareTo(UInt128 other);
         public bool Equals(UInt128 other);
         public override bool Equals(object obj);
         public UInt128 Fork();
         public override int GetHashCode();
         public UInt128 Next();
         public static UInt128 operator +(UInt128 first, ulong second);
         public static bool operator ==(UInt128 first, ulong second);
         public static explicit operator double (UInt128 x);
         public static bool operator >(UInt128 first, ulong second);
         public static bool operator >=(UInt128 first, ulong second);
         public static bool operator !=(UInt128 first, ulong second);
         public static bool operator <(UInt128 first, ulong second);
         public static bool operator <=(UInt128 first, ulong second);
         public static UInt128 operator -(UInt128 first, ulong second);
         public override string ToString();
     }
     public delegate void ValueGetter<TValue>(ref TValue value);
     public delegate void ValueMapper<TSrc, TDst>(ref TSrc src, ref TDst dst);
     public delegate void ValueMapper<TVal1, TVal2, TVal3>(ref TVal1 val1, ref TVal2 val2, ref TVal3 val3);
     [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
     public struct VBuffer<T> {
         public readonly int Count;
         public readonly int Length;
         public readonly int[] Indices;
         public readonly T[] Values;
         public VBuffer(int length, int count, T[] values, int[] indices);
         public VBuffer(int length, T[] values, int[] indices=null);
         public bool IsDense { get; }
         public static void Copy(ref VBuffer<T> src, ref VBuffer<T> dst);
         public static void Copy(T[] src, int srcIndex, ref VBuffer<T> dst, int length);
         public void CopyTo(ref VBuffer<T> dst);
         public void CopyTo(ref VBuffer<T> dst, int srcMin, int length);
         public void CopyTo(ref VBuffer<T> dst, int[] indicesInclude, int count);
         public void CopyTo(T[] dst);
         public void CopyTo(T[] dst, int ivDst, T defaultValue=null);
         public void CopyToDense(ref VBuffer<T> dst);
         public IEnumerable<T> DenseValues();
         public T GetItemOrDefault(int slot);
         public void GetItemOrDefault(int slot, ref T dst);
         public IEnumerable<KeyValuePair<int, T>> Items(bool all=false);
     }
     public sealed class VectorType : StructuredType {
         public VectorType(PrimitiveType itemType, VectorType template);
         public VectorType(PrimitiveType itemType, VectorType template, params int[] dims);
         public VectorType(PrimitiveType itemType, int size=0);
         public VectorType(PrimitiveType itemType, params int[] dims);
         public int DimCount { get; }
         public new PrimitiveType ItemType { get; }
         public override bool Equals(ColumnType other);
         public override bool Equals(object other);
         public int GetDim(int idim);
         public override int GetHashCode();
         public bool IsSubtypeOf(VectorType other);
         public override string ToString();
     }
 }
```

## Microsoft.ML.Runtime.EntryPoints

```C#
 namespace Microsoft.ML.Runtime.EntryPoints {
     public sealed class EntryPointModuleAttribute : LoadableClassAttributeBase {
         public EntryPointModuleAttribute(Type loaderType);
     }
     public static class EntryPointUtils {
         public static Nullable<T> AsNullable<T>(this Optional<T> opt) where T : struct;
         public static IHost CheckArgsAndCreateHost(IHostEnvironment env, string hostName, object input);
         public static void CheckInputArgs(IExceptionContext ectx, object args);
         public static string FindColumnOrNull(IExceptionContext ectx, Schema schema, Optional<string> value);
         public static bool IsValueWithinRange(this TlcModule.RangeAttribute range, object val);
     }
     public interface IMlState
     public interface IPredictorModel {
         IPredictor Predictor { get; }
         ITransformModel TransformModel { get; }
         IPredictorModel Apply(IHostEnvironment env, ITransformModel transformModel);
         string[] GetLabelInfo(IHostEnvironment env, out ColumnType labelType);
         RoleMappedSchema GetTrainingSchema(IHostEnvironment env);
         void PrepareData(IHostEnvironment env, IDataView input, out RoleMappedData roleMappedData, out IPredictor predictor);
         void Save(IHostEnvironment env, Stream stream);
     }
     public interface ITransformModel {
         Schema InputSchema { get; }
         Schema OutputSchema { get; }
         IDataView Apply(IHostEnvironment env, IDataView input);
         ITransformModel Apply(IHostEnvironment env, ITransformModel input);
         IRowToRowMapper AsRowToRowMapper(IExceptionContext ectx);
         void Save(IHostEnvironment env, Stream stream);
     }
     public abstract class Optional {
         public readonly bool IsExplicit;
         protected Optional(bool isExplicit);
         public abstract object GetValue();
     }
     public sealed class Optional<T> : Optional {
         public readonly T Value;
         public static Optional<T> Explicit(T value);
         public override object GetValue();
         public static Optional<T> Implicit(T value);
         public static implicit operator T (Optional<T> optional);
         public static implicit operator Optional<T> (T value);
         public override string ToString();
     }
     public delegate void SignatureEntryPointModule();
     public static class TlcModule {
         public static TlcModule.DataKind GetDataType(Type type);
         public static bool IsNumericKind(TlcModule.DataKind kind);
         public sealed class ComponentAttribute : Attribute {
             public ComponentAttribute();
             public string Alias { get; set; }
             public string[] Aliases { get; set; }
             public string Desc { get; set; }
             public string DocName { get; set; }
             public string FriendlyName { get; set; }
             public string Name { get; set; }
         }
         public sealed class ComponentKindAttribute : Attribute {
             public readonly string Kind;
             public ComponentKindAttribute(string kind);
         }
         public enum DataKind {
             Array = 12,
             Bool = 6,
             Char = 4,
             Component = 14,
             DataView = 7,
             Dictionary = 13,
             Enum = 11,
             FileHandle = 8,
             Float = 3,
             Int = 1,
             PredictorModel = 10,
             State = 15,
             String = 5,
             TransformModel = 9,
             UInt = 2,
             Unknown = 0,
         }
         public sealed class EntryPointAttribute : Attribute {
             public EntryPointAttribute();
             public string Desc { get; set; }
             public string Name { get; set; }
             public string ShortName { get; set; }
             public string UserName { get; set; }
             public string[] XmlInclude { get; set; }
         }
         public sealed class EntryPointKindAttribute : Attribute {
             public readonly Type[] Kinds;
             public EntryPointKindAttribute(params Type[] kinds);
         }
         public sealed class OptionalInputAttribute : Attribute {
             public OptionalInputAttribute();
         }
         public sealed class OutputAttribute : Attribute {
             public OutputAttribute();
             public string Desc { get; set; }
             public string Name { get; set; }
             public double SortOrder { get; set; }
         }
         public sealed class RangeAttribute : Attribute {
             public RangeAttribute();
             public object Inf { get; set; }
             public object Max { get; set; }
             public object Min { get; set; }
             public object Sup { get; set; }
             public Type Type { get; }
             public void CastToDouble();
             public override string ToString();
         }
         public sealed class SweepableDiscreteParamAttribute : TlcModule.SweepableParamAttribute {
             public SweepableDiscreteParamAttribute(object[] values, bool isBool=false);
             public SweepableDiscreteParamAttribute(string name, object[] values, bool isBool=false);
             public object[] Options { get; }
             public override IComparable RawValue { get; set; }
             public override TlcModule.SweepableParamAttribute Clone();
             public int IndexOf(object option);
             public override IComparable ProcessedValue();
             public override void SetUsingValueText(string valueText);
             public override string ToString();
         }
         public sealed class SweepableFloatParamAttribute : TlcModule.SweepableParamAttribute {
             public SweepableFloatParamAttribute(float min, float max, float stepSize=-1f, int numSteps=-1, bool isLogScale=false);
             public SweepableFloatParamAttribute(string name, float min, float max, float stepSize=-1f, int numSteps=-1, bool isLogScale=false);
             public bool IsLogScale { get; }
             public float Max { get; }
             public float Min { get; }
             public Nullable<int> NumSteps { get; }
             public Nullable<float> StepSize { get; }
             public override TlcModule.SweepableParamAttribute Clone();
             public override void SetUsingValueText(string valueText);
             public override string ToString();
         }
         public sealed class SweepableLongParamAttribute : TlcModule.SweepableParamAttribute {
             public SweepableLongParamAttribute(long min, long max, float stepSize=-1f, int numSteps=-1, bool isLogScale=false);
             public SweepableLongParamAttribute(string name, long min, long max, float stepSize=-1f, int numSteps=-1, bool isLogScale=false);
             public bool IsLogScale { get; }
             public long Max { get; }
             public long Min { get; }
             public Nullable<int> NumSteps { get; }
             public Nullable<float> StepSize { get; }
             public override TlcModule.SweepableParamAttribute Clone();
             public override void SetUsingValueText(string valueText);
             public override string ToString();
         }
         public abstract class SweepableParamAttribute : Attribute {
             protected SweepableParamAttribute();
             public bool Frozen { get; set; }
             public string Name { get; set; }
             public virtual IComparable RawValue { get; set; }
             public abstract TlcModule.SweepableParamAttribute Clone();
             public virtual IComparable ProcessedValue();
             public abstract void SetUsingValueText(string valueText);
         }
     }
 }
```

## Microsoft.ML.Runtime.Internal.CpuMath

```C#
 namespace Microsoft.ML.Runtime.Internal.CpuMath {
     public static class MemUtils {
         public unsafe static void MemoryCopy(void* source, void* destination, long destinationSizeInBytes, long sourceBytesToCopy);
     }
 }
```

## Microsoft.ML.Runtime.Internal.Utilities

```C#
 namespace Microsoft.ML.Runtime.Internal.Utilities {
     public sealed class BigArray<T> {
         public BigArray(long size=(long)0);
         public long Length { get; }
         public T this[long index] { get; set; }
         public void AddRange(T[] src, int length);
         public void ApplyAt(long index, BigArray<T>.Visitor manip);
         public void ApplyRange(long min, long lim, BigArray<T>.Visitor manip);
         public void CopyTo(long idx, T[] dst, int length);
         public void FillRange(long min, long lim, T value);
         public void Resize(long newLength);
         public void TrimCapacity();
         public delegate void Visitor(long index, ref T item);
     }
     public abstract class BinFinderBase {
         protected BinFinderBase();
         protected int CountBins { get; private set; }
         protected int CountValues { get; private set; }
         public double[] FindBins(int cbin, IList<double> values, int numZeroes=0);
         public float[] FindBins(int cbin, IList<float> values, int numZeroes=0);
         protected abstract void FindBinsCore(List<int> counts, int[] path);
         public void FindBinsWithCounts(IList<int> counts, int numValues, int cbin, int[] path);
         public static double GetSplitValue(double a, double b);
         public static float GetSplitValue(float a, float b);
     }
     public static class CharUtils {
         public static char ToLowerInvariant(char c);
         public static char ToUpperInvariant(char c);
     }
     public static class CmdIndenter {
         public static string GetIndentedCommandLine(string commandLine);
     }
     public class DoubleParser {
         public DoubleParser();
         public static DoubleParser.Result Parse(ReadOnlySpan<char> span, out double value);
         public static DoubleParser.Result Parse(ReadOnlySpan<char> span, out float value);
         public static bool TryParse(ReadOnlySpan<char> span, out double value);
         public static bool TryParse(ReadOnlySpan<char> span, out double value, out int ichEnd);
         public static bool TryParse(ReadOnlySpan<char> span, out float value);
         public static bool TryParse(ReadOnlySpan<char> span, out float value, out int ichEnd);
         public enum Result {
             Empty = 1,
             Error = 3,
             Extra = 2,
             Good = 0,
         }
     }
     public sealed class DynamicBinFinder : BinFinderBase {
         public DynamicBinFinder();
         protected override void FindBinsCore(List<int> counts, int[] path);
     }
     public sealed class ExceptionMarshaller : IDisposable {
         public ExceptionMarshaller();
         public CancellationToken Token { get; }
         public void Dispose();
         public void Set(string component, Exception ex);
         public void ThrowIfSet(IExceptionContext ectx);
     }
     public sealed class FixedSizeQueue<T> {
         public FixedSizeQueue(int capacity);
         public int Capacity { get; }
         public int Count { get; }
         public bool IsFull { get; }
         public T this[int index] { get; }
         public void AddLast(T item);
         public void Clear();
         public T PeekFirst();
         public T PeekLast();
         public T RemoveFirst();
         public T RemoveLast();
     }
     public static class FloatUtils {
         public static float FromBits(uint bits);
         public static double FromBits(ulong bits);
         public static ulong GetBits(double x);
         public static uint GetBits(float x);
         public static int GetExponent(double x);
         public static int GetExponent(float x);
         public static double GetFromPartsDouble(int sign, int exp, ulong man);
         public static float GetFromPartsSingle(int sign, int exp, uint man);
         public static void GetParts(double x, out int sign, out int exp, out ulong man, out bool fFinite);
         public static void GetParts(float x, out int sign, out int exp, out uint man, out bool fFinite);
         public static double GetPowerOfTwoDouble(int exp);
         public static float GetPowerOfTwoSingle(int exp);
         public static bool IsDenormal(double x);
         public static bool IsDenormal(float x);
         public static bool IsFinite(double x);
         public static bool IsFinite(double[] values, int count);
         public static bool IsFinite(float x);
         public static bool IsFinite(float[] values, int count);
         public static bool IsFiniteNonZero(double x);
         public static bool IsFiniteNonZero(float x);
         public static bool IsFiniteNormal(double x);
         public static bool IsFiniteNormal(float x);
         public static int NormalizeExponent(ref double x);
         public static int NormalizeExponent(ref float x);
         public static double SetExponent(double x, int exp);
         public static float SetExponent(float x, int exp);
         public static string ToRoundTripString(double x);
         public static string ToRoundTripString(float x);
         public static double Truncate(double x);
         public static double TruncateMantissaToSingleBit(double x);
         public static float TruncateMantissaToSingleBit(float x);
     }
     public sealed class GreedyBinFinder : BinFinderBase {
         public GreedyBinFinder();
         protected override void FindBinsCore(List<int> counts, int[] path);
     }
     public sealed class HashArray<TItem> where TItem : IEquatable<TItem>, IComparable<TItem> {
         public HashArray();
         public int Count { get; }
         public int Add(TItem val);
         public void CopyTo(TItem[] array);
         public TItem GetItem(int it);
         public void Sort();
         public bool TryAdd(TItem val);
         public bool TryGetIndex(TItem val, out int index);
     }
     public static class Hashing {
         public static int CombinedHash(int startHash, params object[] os);
         public static int CombinedHash<T>(int startHash, params T[] os);
         public static int CombineHash(int n1, int n2);
         public static uint CombineHash(uint u1, uint u2);
         public static int HashInt(int n);
         public static uint HashSequence(uint[] sequence, int min, int lim);
         public static uint HashString(ReadOnlySpan<char> str);
         public static uint HashString(StringBuilder sb);
         public static uint HashUint(uint u);
         public static uint MixHash(uint hash);
         public static uint MurmurHash(uint hash, ReadOnlySpan<char> span, bool toUpper=false);
         public static uint MurmurHash(uint hash, StringBuilder data, int ichMin, int ichLim, bool toUpper=false);
         public static uint MurmurHash(uint hash, uint[] data, int min, int lim);
         public static uint MurmurRound(uint hash, uint chunk);
     }
     public sealed class Heap<T> {
         public Heap(Func<T, T, bool> fnReverse);
         public Heap(Func<T, T, bool> fnReverse, int capacity);
         public int Count { get; }
         public Func<T, T, bool> FnReverse { get; }
         public T Top { get; }
         public void Add(T item);
         public void Clear();
         public T Pop();
     }
     public abstract class HeapNode {
         protected HeapNode();
         public bool InHeap { get; }
         public sealed class Heap<T> where T : HeapNode {
             public Heap(Func<T, T, bool> fnReverse);
             public Heap(Func<T, T, bool> fnReverse, int capacity);
             public int Count { get; }
             public Func<T, T, bool> FnReverse { get; }
             public T Top { get; }
             public void Add(T item);
             public void Clear();
             public void Delete(T item);
             public T Pop();
         }
     }
     public sealed class HybridMemoryStream : Stream {
         public HybridMemoryStream(int maxLen=1073741824);
         public override bool CanRead { get; }
         public override bool CanSeek { get; }
         public override bool CanWrite { get; }
         public override long Length { get; }
         public override long Position { get; set; }
         public override void Close();
         public static Stream CreateCache(Stream stream, int maxLen=1073741824);
         protected override void Dispose(bool disposing);
         public override void Flush();
         public override int Read(byte[] buffer, int offset, int count);
         public override int ReadByte();
         public override long Seek(long offset, SeekOrigin origin);
         public override void SetLength(long value);
         public override void Write(byte[] buffer, int offset, int count);
         public override void WriteByte(byte value);
     }
     public sealed class IndentingTextWriter : TextWriter {
         public override Encoding Encoding { get; }
         public void Indent();
         public IndentingTextWriter.Scope Nest();
         public void Outdent();
         public void Skip();
         public static IndentingTextWriter Wrap(TextWriter wrt, string indentString="  ");
         public override void Write(bool value);
         public override void Write(char value);
         public override void Write(char[] buffer);
         public override void Write(char[] buffer, int index, int count);
         public override void Write(decimal value);
         public override void Write(double value);
         public override void Write(int value);
         public override void Write(long value);
         public override void Write(object value);
         public override void Write(float value);
         public override void Write(string value);
         public override void Write(string format, object arg0);
         public override void Write(string format, object arg0, object arg1);
         public override void Write(string format, object arg0, object arg1, object arg2);
         public override void Write(string format, params object[] arg);
         public override void Write(uint value);
         public override void Write(ulong value);
         public override void WriteLine();
         public override void WriteLine(bool value);
         public override void WriteLine(char value);
         public override void WriteLine(char[] buffer);
         public override void WriteLine(char[] buffer, int index, int count);
         public override void WriteLine(decimal value);
         public override void WriteLine(double value);
         public override void WriteLine(int value);
         public override void WriteLine(long value);
         public override void WriteLine(object value);
         public override void WriteLine(float value);
         public override void WriteLine(string value);
         public override void WriteLine(string format, object arg0);
         public override void WriteLine(string format, object arg0, object arg1);
         public override void WriteLine(string format, object arg0, object arg1, object arg2);
         public override void WriteLine(string format, params object[] arg);
         public override void WriteLine(uint value);
         public override void WriteLine(ulong value);
         [System.Runtime.InteropServices.StructLayoutAttribute(System.Runtime.InteropServices.LayoutKind.Sequential)]
         public struct Scope : IDisposable {
             public Scope(IndentingTextWriter wrt);
             public void Dispose();
         }
     }
     public interface IReservoirSampler<T> {
         long NumSampled { get; }
         int Size { get; }
         IEnumerable<T> GetSample();
         void Lock();
         void Sample();
     }
     public sealed class LimitedConcurrencyLevelTaskScheduler : TaskScheduler {
         public LimitedConcurrencyLevelTaskScheduler(int concurrencyLevel);
         public override int MaximumConcurrencyLevel { get; }
         protected override IEnumerable<Task> GetScheduledTasks();
         protected override void QueueTask(Task task);
         protected override bool TryDequeue(Task task);
         protected override bool TryExecuteTaskInline(Task task, bool taskWasPreviouslyQueued);
     }
     public static class ListExtensions {
         public static T Peek<T>(this List<T> list);
         public static T Pop<T>(this List<T> list);
         public static void PopTo<T>(this List<T> list, int depth);
         public static void Push<T>(this List<T> list, T item);
     }
     public sealed class LruCache<TKey, TValue> {
         public LruCache(int size);
         public IEnumerable<TKey> Keys { get; }
         public void Add(TKey key, TValue value);
         public bool TryGetValue(TKey key, out TValue value);
     }
     public sealed class MadeObjectPool<T> : ObjectPoolBase<T> {
         public MadeObjectPool(Func<T> maker);
         protected override T Create();
     }
     public static class MathUtils {
         public const float DefaultMaxAbsErr = 1E-12f;
         public const float DefaultMaxRelativeErr = 1E-08f;
         public static bool AlmostEqual(float a, float b);
         public static bool AlmostEqual(float a, float b, float maxRelErr, float maxAbsError);
         public static void ApplySoftMax(float[] src, float[] dst);
         public static void ApplySoftMax(float[] src, float[] dst, int start, int end);
         public static int ArgMax(float[] a);
         public static int ArgMax(float[] a, int count);
         public static int ArgMin(float[] a);
         public static int ArgMin(float[] a, int count);
         public static double Cos(double a);
         public static double CosineSimilarity(float[] a, float[] b, int aIdx, int bIdx, int len);
         public static double CrossEntropy(double probTrue, double probPredicted, bool useLnNotLog2=false);
         public static long DivisionCeiling(long numerator, long denomenator);
         public static double Entropy(double prob, bool useLnNotLog2=false);
         public static float ExpFast(float x);
         public static float ExpSlow(float x);
         public static float GetMedianInPlace(float[] src, int count);
         public static float LnSum(IEnumerable<float> terms);
         public static float Log(float x);
         public static float Log(float a, float newBase);
         public static double LogFactorial(int n);
         public static double LogGamma(double x);
         public static float Max(float[] a);
         public static float Min(float[] a);
         public static float Pow(float x, float y);
         public static int Product(int[] a);
         public static float Sigmoid(float x);
         public static float SigmoidFast(float x);
         public static float SigmoidSlow(float x);
         public static void SimpleLinearRegression(float[] x, float[] y, out float a, out float b);
         public static double Sin(double a);
         public static float SoftMax(float lx, float ly);
         public static float SoftMax(float[] inputs, int count);
         public static float Sqrt(float x);
         public static float Tanh(float x);
         public static float TanhFast(float x);
         public static float TanhSlow(float x);
         public static float ToFloat(this double dbl);
         public static void ToFloat(this float dbl);
         public static double TStatisticToPValue(double t, double df);
     }
     public static class MatrixTransposeOps {
         public unsafe static void Transpose(double* src, double* dst, int m, int n);
         public unsafe static void Transpose(float* src, float* dst, int m, int n);
         public static void Transpose<T>(T[] src, T[] dst, int m, int n);
         public static void TransposeSingleThread<T>(T[] src, T[] dst, int m, int n);
     }
     public sealed class MinWaiter {
         public MinWaiter(int waiters);
         public ManualResetEventSlim Register(long position);
         public int Retire();
     }
     public sealed class NormStr {
         public readonly int Id;
         public readonly ReadOnlyMemory<char> Value;
         public override int GetHashCode();
         public sealed class Pool : IEnumerable, IEnumerable<NormStr> {
             public Pool();
             public int Count { get; }
             public NormStr Add(ReadOnlyMemory<char> str);
             public NormStr Add(string str);
             public NormStr Add(StringBuilder sb);
             public NormStr Get(ReadOnlyMemory<char> str, bool add=false);
             public NormStr Get(string str, bool add=false);
             public NormStr Get(StringBuilder sb, bool add=false);
             public IEnumerator<NormStr> GetEnumerator();
             public NormStr GetNormStrById(int id);
             IEnumerator System.Collections.IEnumerable.GetEnumerator();
         }
     }
     public sealed class ObjectPool<T> : ObjectPoolBase<T> where T : class, new() {
         public ObjectPool();
         protected override T Create();
     }
     public abstract class ObjectPoolBase<T> {
         public int Count { get; }
         public int NumCreated { get; }
         protected abstract T Create();
         public T Get();
         public void Return(T item);
     }
     public sealed class OrderedWaiter {
         public OrderedWaiter(bool firstCleared=true);
         public OrderedWaiter(long startPos);
         public long Increment();
         public long IncrementAll();
         public void SignalException(Exception ex);
         public void Wait(long position, CancellationToken token=default(CancellationToken));
     }
     public static class PlatformUtils {
         public static ReadOnlyCollection<T> AsReadOnly<T>(this T[] items);
         public static Type[] GetGenericTypeArgumentsEx(this Type type);
         public static bool IsGenericEx(this Type type, Type typeDef);
     }
     public sealed class ReservoirSamplerWithoutReplacement<T> : IReservoirSampler<T> {
         public ReservoirSamplerWithoutReplacement(IRandom rnd, int size, ValueGetter<T> getter);
         public long NumSampled { get; }
         public int Size { get; }
         public IEnumerable<T> GetSample();
         public void Lock();
         public void Sample();
     }
     public sealed class ReservoirSamplerWithReplacement<T> : IReservoirSampler<T> {
         public ReservoirSamplerWithReplacement(IRandom rnd, int size, ValueGetter<T> getter);
         public long NumSampled { get; }
         public int Size { get; }
         public T[] GetCache();
         public IEnumerable<T> GetSample();
         public void Lock();
         public void Sample();
     }
     public sealed class ResourceManagerUtils {
         public const string CustomResourcesUrlEnvVariable = "MICROSOFTML_RESOURCE_URL";
         public const string TimeoutEnvVariable = "MICROSOFTML_RESOURCE_TIMEOUT";
         public static ResourceManagerUtils Instance { get; }
         public Task<ResourceManagerUtils.ResourceDownloadResults> EnsureResource(IHostEnvironment env, IChannel ch, string relativeUrl, string fileName, string dir, int timeout);
         public static ResourceManagerUtils.ResourceDownloadResults GetErrorMessage(out string errorMessage, params ResourceManagerUtils.ResourceDownloadResults[] result);
         public static string GetUrl(string suffix);
         public sealed class ResourceDownloadResults {
             public readonly string FileName;
             public ResourceDownloadResults(string fileName, string errorMessage, string downloadUrl=null);
         }
     }
     public static class Stats {
         public static double SampleFromBeta(IRandom rand, double alpha1, double alpha2);
         public static int SampleFromBinomial(IRandom r, int n, double p);
         public static float SampleFromCauchy(IRandom rand);
         public static void SampleFromDirichlet(IRandom rand, double[] alphas, double[] result);
         public static double SampleFromGamma(IRandom r, double alpha);
         public static double SampleFromGaussian(IRandom rand);
         public static float SampleFromLaplacian(IRandom rand, float mean, float scale);
         public static int SampleFromPoisson(IRandom rand, double lambda);
         public static long SampleLong(long rangeSize, IRandom rand);
     }
     public sealed class SubsetStream : Stream {
         public SubsetStream(Stream stream, Nullable<long> length=default(Nullable<long>));
         public override bool CanRead { get; }
         public override bool CanSeek { get; }
         public override bool CanTimeout { get; }
         public override bool CanWrite { get; }
         public override long Length { get; }
         public override long Position { get; set; }
         public override void Flush();
         public override int Read(byte[] buffer, int offset, int count);
         public override long Seek(long offset, SeekOrigin origin);
         public override void SetLength(long value);
         public override void Write(byte[] buffer, int offset, int count);
     }
     public sealed class SummaryStatistics : SummaryStatisticsBase {
         public SummaryStatistics();
         public double Kurtosis { get; }
         public double KurtosisZ { get; }
         public double Nonzero { get; }
         public double OmnibusK2 { get; }
         public double Skewness { get; }
         public double SkewnessZ { get; }
         public void Add(SummaryStatistics s);
         public override void Add(double v, double w=1, long c=(long)1);
         public override bool Equals(object obj);
         public override int GetHashCode();
         public static SummaryStatistics operator +(SummaryStatistics a, SummaryStatistics b);
         public override string ToString();
     }
     public abstract class SummaryStatisticsBase {
         protected double M2;
         public double Count { get; private set; }
         public double Max { get; private set; }
         public double Mean { get; private set; }
         public double Min { get; private set; }
         public double NonzeroCount { get; private set; }
         public double NonzeroWeight { get; private set; }
         public long RawCount { get; private set; }
         public double SampleStdDev { get; }
         public double SampleVariance { get; }
         public double StandardErrorMean { get; }
         public void Add(SummaryStatisticsBase s);
         public virtual void Add(double v, double w=1, long c=(long)1);
         public override bool Equals(object obj);
         public override int GetHashCode();
         public override string ToString();
     }
     public sealed class SummaryStatisticsUpToSecondOrderMoments : SummaryStatisticsBase {
         public SummaryStatisticsUpToSecondOrderMoments();
         public static SummaryStatisticsUpToSecondOrderMoments operator +(SummaryStatisticsUpToSecondOrderMoments a, SummaryStatisticsUpToSecondOrderMoments b);
     }
     public sealed class SupervisedBinFinder {
         public SupervisedBinFinder();
         public double[] FindBins(int maxBins, int minBinSize, int nLabels, IList<double> values, IList<int> labels);
         public float[] FindBins(int maxBins, int minBinSize, int nLabels, IList<float> values, IList<int> labels);
     }
     public sealed class TextReaderStream : Stream {
         public TextReaderStream(TextReader baseReader);
         public TextReaderStream(TextReader baseReader, Encoding encoding);
         public override bool CanRead { get; }
         public override bool CanSeek { get; }
         public override bool CanWrite { get; }
         public override long Length { get; }
         public override long Position { get; set; }
         public override void Close();
         protected override void Dispose(bool disposing);
         public override void Flush();
         public override int Read(byte[] buffer, int offset, int count);
         public override int ReadByte();
         public override long Seek(long offset, SeekOrigin origin);
         public override void SetLength(long value);
         public override void Write(byte[] buffer, int offset, int count);
     }
     public sealed class Tree<TKey, TValue> : ICollection<KeyValuePair<TKey, Tree<TKey, TValue>>>, IDictionary<TKey, Tree<TKey, TValue>>, IEnumerable, IEnumerable<KeyValuePair<TKey, Tree<TKey, TValue>>> {
         public Tree();
         public int Count { get; }
         public bool HasNodeValue { get; }
         public bool IsReadOnly { get; }
         public TKey Key { get; }
         public ICollection<TKey> Keys { get; }
         public TValue NodeValue { get; set; }
         public Tree<TKey, TValue> Parent { get; }
         public Tree<TKey, TValue> this[TKey key] { get; set; }
         public ICollection<Tree<TKey, TValue>> Values { get; }
         public void Add(KeyValuePair<TKey, Tree<TKey, TValue>> item);
         public void Add(TKey key, Tree<TKey, TValue> newChild);
         public void Clear();
         public void ClearNodeValue();
         public bool Contains(KeyValuePair<TKey, Tree<TKey, TValue>> item);
         public bool ContainsKey(TKey key);
         public void CopyTo(KeyValuePair<TKey, Tree<TKey, TValue>>[] array, int arrayIndex);
         public void Detach();
         public IEnumerator<KeyValuePair<TKey, Tree<TKey, TValue>>> GetEnumerator();
         public bool Remove(KeyValuePair<TKey, Tree<TKey, TValue>> item);
         public bool Remove(TKey key);
         IEnumerator System.Collections.IEnumerable.GetEnumerator();
         public bool TryGetValue(TKey key, out Tree<TKey, TValue> value);
     }
     public static class Utils {
         public const int ArrayMaxSize = 2146435071;
         public const string CustomSearchDirEnvVariable = "MICROSOFTML_RESOURCE_PATH";
         public static uint Abs(int a);
         public static ulong Abs(long a);
         public static bool Add<T>(ref HashSet<T> @set, T item);
         public static void Add<T>(ref List<T> list, T item);
         public static void Add<TKey, TValue>(ref Dictionary<TKey, TValue> map, TKey key, TValue value);
         public static bool AreEqual(bool[] arr1, bool[] arr2);
         public static bool AreEqual(double[] arr1, double[] arr2);
         public static bool AreEqual(int[] arr1, int[] arr2);
         public static bool AreEqual(float[] arr1, float[] arr2);
         public static T[] BuildArray<T>(int length, Func<int, T> func);
         public static void BuildSubsetMaps(int lim, Func<int, bool> pred, out int[] map, out int[] invMap);
         public static int Cbit(uint u);
         public static int Cbit(ulong uu);
         public static int CbitHighZero(uint u);
         public static int CbitHighZero(ulong uu);
         public static int CbitLowZero(uint u);
         public static int CbitLowZero(ulong uu);
         public static void CheckOptionalUserDirectory(string file, string userArgument);
         public static void CloseEx(this Stream stream);
         public static void CloseEx(this TextWriter writer);
         public static T[] Concat<T>(T[] a, T[] b);
         public static T[] Concat<T>(params T[][] arrays);
         public static long CopyRange(this Stream source, Stream destination, long length, int bufferSize=81920);
         public static T[] CreateArray<T>(int length, T value);
         public static Thread CreateBackgroundThread(ParameterizedThreadStart start);
         public static Thread CreateBackgroundThread(ThreadStart start);
         public static string CreateFolderIfNotExists(string folder);
         public static Thread CreateForegroundThread(ParameterizedThreadStart start);
         public static Thread CreateForegroundThread(ThreadStart start);
         public static int EnsureSize<T>(ref T[] array, int min, bool keepOld=true);
         public static int EnsureSize<T>(ref T[] array, int min, int max, bool keepOld=true);
         public static string ExtractLettersAndNumbers(string value);
         public static void FillIdentity(int[] a, int lim);
         public static string FindExistentFileOrNull(string fileName, string folderPrefix=null, string customSearchDir=null, Type assemblyForBasePath=null);
         public static int FindIndexSorted(this IList<int> input, int value);
         public static int FindIndexSorted(this IList<int> input, int min, int lim, int value);
         public static int FindIndexSorted(this double[] input, double value);
         public static int FindIndexSorted(this double[] input, int min, int lim, double value);
         public static int FindIndexSorted(this int[] input, int value);
         public static int FindIndexSorted(this int[] input, int min, int lim, int value);
         public static int FindIndexSorted(this float[] input, int min, int lim, float value);
         public static int FindIndexSorted(this float[] input, float value);
         public static int FindIndexSorted<T, TValue>(this T[] input, int min, int lim, Func<T, TValue, bool> func, TValue value);
         public static int FindIndexSorted<T>(this T[] input, int min, int lim, Func<T, bool> func);
         public static long FpCur(this BinaryReader reader);
         public static long FpCur(this BinaryWriter writer);
         public static int GetCardinality(BitArray bitArray);
         public static string GetDescription(this Enum value);
         public static uint GetHi(ulong uu);
         public static int[] GetIdentityPermutation(int size);
         public static uint GetLo(ulong uu);
         public static int[] GetRandomPermutation(IRandom rand, int size);
         public static int[] GetRandomPermutation(Random rand, int size);
         public static int IbitHigh(uint u);
         public static void InterlockedAdd(ref double target, double v);
         public static int[] InvertPermutation(int[] perm);
         public static bool IsIncreasing(int min, int[] values, int lim);
         public static bool IsIncreasing(int min, int[] values, int len, int lim);
         public static bool IsPowerOfTwo(int x);
         public static bool IsPowerOfTwo(long x);
         public static bool IsPowerOfTwo(uint x);
         public static bool IsPowerOfTwo(ulong x);
         public static bool IsSorted(int[] values);
         public static bool IsSorted(float[] values);
         public static int Leb128IntLength(ulong value);
         public static ulong MakeUlong(uint uHi, uint uLo);
         public static void MarshalActionInvoke(Action act, Type genArg);
         public static void MarshalActionInvoke<TArg1, TArg2, TArg3>(Action<TArg1, TArg2, TArg3> act, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3);
         public static void MarshalActionInvoke<TArg1, TArg2>(Action<TArg1, TArg2> act, Type genArg, TArg1 arg1, TArg2 arg2);
         public static void MarshalActionInvoke<TArg1>(Action<TArg1> act, Type genArg, TArg1 arg1);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TArg9, TArg10, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TArg9, TArg10, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5, TArg6 arg6, TArg7 arg7, TArg8 arg8, TArg9 arg9, TArg10 arg10);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TArg9, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TArg9, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5, TArg6 arg6, TArg7 arg7, TArg8 arg8, TArg9 arg9);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TArg8, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5, TArg6 arg6, TArg7 arg7, TArg8 arg8);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TArg7, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5, TArg6 arg6, TArg7 arg7);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TArg6, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5, TArg6 arg6);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TArg5, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TArg5, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4, TArg5 arg5);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TArg4, TRet>(Func<TArg1, TArg2, TArg3, TArg4, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3, TArg4 arg4);
         public static TRet MarshalInvoke<TArg1, TArg2, TArg3, TRet>(Func<TArg1, TArg2, TArg3, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2, TArg3 arg3);
         public static TRet MarshalInvoke<TArg1, TArg2, TRet>(Func<TArg1, TArg2, TRet> func, Type genArg, TArg1 arg1, TArg2 arg2);
         public static TRet MarshalInvoke<TArg1, TRet>(Func<TArg1, TRet> func, Type genArg, TArg1 arg1);
         public static TRet MarshalInvoke<TRet>(Func<TRet> func, Type genArg);
         public static StreamWriter OpenWriter(Stream stream, Encoding encoding=null, int bufferSize=1024, bool leaveOpen=true);
         public static void Push<T>(ref Stack<T> stack, T item);
         public static BitArray ReadBitArray(this BinaryReader reader);
         public static void ReadBlock(this Stream s, byte[] buff, int offset, int length);
         public static bool[] ReadBoolArray(this BinaryReader reader);
         public static bool[] ReadBoolArray(this BinaryReader reader, int size);
         public static bool ReadBoolByte(this BinaryReader reader);
         public static byte[] ReadByteArray(this BinaryReader reader);
         public static byte[] ReadByteArray(this BinaryReader reader, int size);
         public unsafe static void ReadBytes(this BinaryReader reader, void* destination, long destinationSizeInBytes, long bytesToRead);
         public unsafe static void ReadBytes(this BinaryReader reader, void* destination, long destinationSizeInBytes, long bytesToRead, ref byte[] work);
         public static char[] ReadCharArray(this BinaryReader reader);
         public static char[] ReadCharArray(this BinaryReader reader, int size);
         public static double[] ReadDoubleArray(this BinaryReader reader);
         public static double[] ReadDoubleArray(this BinaryReader reader, int size);
         public static float ReadFloat(this BinaryReader reader);
         public static float[] ReadFloatArray(this BinaryReader reader);
         public static float[] ReadFloatArray(this BinaryReader reader, int size);
         public static void ReadFloatArray(this BinaryReader reader, float[] array, int start, int count);
         public static int[] ReadIntArray(this BinaryReader reader);
         public static int[] ReadIntArray(this BinaryReader reader, int size);
         public static ulong ReadLeb128Int(this BinaryReader reader);
         public static long[] ReadLongArray(this BinaryReader reader);
         public static long[] ReadLongArray(this BinaryReader reader, int size);
         public static float[] ReadSingleArray(this BinaryReader reader);
         public static float[] ReadSingleArray(this BinaryReader reader, int size);
         public static uint[] ReadUIntArray(this BinaryReader reader);
         public static uint[] ReadUIntArray(this BinaryReader reader, int size);
         public static void Reverse<T>(T[] a, int iMin, int iLim);
         public static void Seek(this BinaryReader reader, long fp);
         public static void Seek(this BinaryWriter writer, long fp);
         public static void Set<TKey, TValue>(ref Dictionary<TKey, TValue> map, TKey key, TValue value);
         public static void Shuffle<T>(IRandom rand, T[] rgv);
         public static void Shuffle<T>(IRandom rand, T[] rgv, int min, int lim);
         public static void Shuffle<T>(Random rand, T[] rgv);
         public static void Shuffle<T>(Random rand, T[] rgv, int min, int lim);
         public static int Size(Array x);
         public static int Size(BitArray x);
         public static int Size(string x);
         public static int Size(StringBuilder x);
         public static int Size<T>(HashSet<T> x);
         public static int Size<T>(IList<T> x);
         public static int Size<T>(IReadOnlyList<T> x);
         public static int Size<T>(List<T> x);
         public static int Size<T>(SortedSet<T> x);
         public static int Size<T>(Stack<T> x);
         public static int Size<T>(T[] x);
         public static int Size<TKey, TValue>(Dictionary<TKey, TValue> x);
         public static bool StartsWithInvariantCulture(this string str, string startsWith);
         public static bool StartsWithInvariantCultureIgnoreCase(this string str, string startsWith);
         public static void Swap<T>(ref T a, ref T b);
         public static T[] ToArray<T>(List<T> list);
         public static bool TryFindIndexSorted(this int[] input, int min, int lim, int value, out int index);
         public static bool TryGetValue<TKey, TValue>(Dictionary<TKey, TValue> map, TKey key, out TValue value);
         public static uint UMaskBelow(int ibit);
         public static uint UMaskBelowEx(int ibit);
         public static ulong UuMaskBelow(int ibit);
         public static void WriteBitArray(this BinaryWriter writer, BitArray arr);
         public static void WriteBoolByte(this BinaryWriter writer, bool x);
         public static void WriteBoolByteArray(this BinaryWriter writer, bool[] values);
         public static void WriteBoolByteArray(this BinaryWriter writer, bool[] values, int count);
         public static void WriteBoolBytesNoCount(this BinaryWriter writer, bool[] values, int count);
         public static void WriteByteArray(this BinaryWriter writer, byte[] values);
         public static void WriteByteArray(this BinaryWriter writer, byte[] values, int count);
         public static void WriteBytesNoCount(this BinaryWriter writer, byte[] values, int count);
         public static long WriteByteStream(this BinaryWriter writer, IEnumerable<byte> e);
         public static void WriteCharArray(this BinaryWriter writer, char[] values);
         public static void WriteDoubleArray(this BinaryWriter writer, double[] values);
         public static void WriteDoubleArray(this BinaryWriter writer, double[] values, int count);
         public static void WriteDoublesNoCount(this BinaryWriter writer, double[] values, int count);
         public static long WriteDoubleStream(this BinaryWriter writer, IEnumerable<double> e);
         public static void WriteFloatArray(this BinaryWriter writer, IEnumerable<float> values, int count);
         public static void WriteFloatArray(this BinaryWriter writer, float[] values);
         public static void WriteFloatArray(this BinaryWriter writer, float[] values, int count);
         public static void WriteFloatArray(this BinaryWriter writer, float[] values, int start, int count);
         public static void WriteFloatsNoCount(this BinaryWriter writer, float[] values, int count);
         public static void WriteIntArray(this BinaryWriter writer, int[] values);
         public static void WriteIntArray(this BinaryWriter writer, int[] values, int count);
         public static void WriteIntsNoCount(this BinaryWriter writer, int[] values, int count);
         public static long WriteIntStream(this BinaryWriter writer, IEnumerable<int> e);
         public static void WriteLeb128Int(this BinaryWriter writer, ulong value);
         public static long WriteLongStream(this BinaryWriter writer, IEnumerable<long> e);
         public static long WriteSByteStream(this BinaryWriter writer, IEnumerable<sbyte> e);
         public static long WriteShortStream(this BinaryWriter writer, IEnumerable<short> e);
         public static void WriteSingleArray(this BinaryWriter writer, float[] values);
         public static void WriteSingleArray(this BinaryWriter writer, float[] values, int count);
         public static void WriteSinglesNoCount(this BinaryWriter writer, float[] values, int count);
         public static long WriteSingleStream(this BinaryWriter writer, IEnumerable<float> e);
         public static long WriteStringStream(this BinaryWriter writer, IEnumerable<string> e);
         public static void WriteUIntArray(this BinaryWriter writer, uint[] values);
         public static void WriteUIntArray(this BinaryWriter writer, uint[] values, int count);
         public static void WriteUIntsNoCount(this BinaryWriter writer, uint[] values, int count);
         public static long WriteUIntStream(this BinaryWriter writer, IEnumerable<uint> e);
         public static long WriteULongStream(this BinaryWriter writer, IEnumerable<long> e);
         public static long WriteUShortStream(this BinaryWriter writer, IEnumerable<ushort> e);
     }
     public static class VBufferUtils {
         public static void Apply<T>(ref VBuffer<T> dst, VBufferUtils.SlotValueManipulator<T> manip);
         public static void ApplyAt<T>(ref VBuffer<T> dst, int slot, VBufferUtils.SlotValueManipulator<T> manip, VBufferUtils.ValuePredicate<T> pred=null);
         public static void ApplyInto<TSrc1, TSrc2, TDst>(ref VBuffer<TSrc1> a, ref VBuffer<TSrc2> b, ref VBuffer<TDst> dst, Func<int, TSrc1, TSrc2, TDst> func);
         public static void ApplyIntoEitherDefined<TSrc, TDst>(ref VBuffer<TSrc> src, ref VBuffer<TDst> dst, Func<int, TSrc, TDst> func);
         public static void ApplyWith<TSrc, TDst>(ref VBuffer<TSrc> src, ref VBuffer<TDst> dst, VBufferUtils.PairManipulator<TSrc, TDst> manip);
         public static void ApplyWithCopy<TSrc, TDst>(ref VBuffer<TSrc> src, ref VBuffer<TDst> dst, ref VBuffer<TDst> res, VBufferUtils.PairManipulatorCopy<TSrc, TDst> manip);
         public static void ApplyWithEitherDefined<TSrc, TDst>(ref VBuffer<TSrc> src, ref VBuffer<TDst> dst, VBufferUtils.PairManipulator<TSrc, TDst> manip);
         public static void ApplyWithEitherDefinedCopy<TSrc, TDst>(ref VBuffer<TSrc> src, ref VBuffer<TDst> dst, ref VBuffer<TDst> res, VBufferUtils.PairManipulatorCopy<TSrc, TDst> manip);
         public static void Clear<T>(ref VBuffer<T> dst);
         public static void Copy<T>(List<T> src, ref VBuffer<T> dst, int length);
         public static VBuffer<T> CreateDense<T>(int length);
         public static VBuffer<T> CreateEmpty<T>(int length);
         public static void CreateMaybeSparseCopy<T>(ref VBuffer<T> src, ref VBuffer<T> dst, RefPredicate<T> isDefaultPredicate, float sparsityThreshold=0.25f);
         public static void Densify<T>(ref VBuffer<T> dst);
         public static void DensifyFirst<T>(ref VBuffer<T> dst, int denseCount);
         public static void ForEachBothDefined<T>(ref VBuffer<T> a, ref VBuffer<T> b, Action<int, T, T> visitor);
         public static void ForEachDefined<T>(ref VBuffer<T> a, Action<int, T> visitor);
         public static void ForEachEitherDefined<T>(ref VBuffer<T> a, ref VBuffer<T> b, Action<int, T, T> visitor);
         public static bool HasNaNs(ref VBuffer<double> buffer);
         public static bool HasNaNs(ref VBuffer<float> buffer);
         public static bool HasNonFinite(ref VBuffer<double> buffer);
         public static bool HasNonFinite(ref VBuffer<float> buffer);
         public delegate void PairManipulator<TSrc, TDst>(int slot, TSrc src, ref TDst dst);
         public delegate void PairManipulatorCopy<TSrc, TDst>(int slot, TSrc src, TDst dst, ref TDst res);
         public delegate void SlotValueManipulator<T>(int slot, ref T value);
         public delegate bool ValuePredicate<T>(ref T src);
     }
 }
```

## Microsoft.ML.Runtime.TreePredictor

```C#
 namespace Microsoft.ML.Runtime.TreePredictor {
     public interface INode {
         Dictionary<string, object> KeyValues { get; }
     }
     public interface ITree {
         int[] GtChild { get; }
         int[] LteChild { get; }
         int NumLeaves { get; }
         int NumNodes { get; }
         INode GetNode(int nodeId, bool isLeaf, IEnumerable<string> featureNames);
     }
     public interface ITree<TFeatures> : ITree {
         int GetLeaf(ref TFeatures features);
     }
     public interface ITreeEnsemble {
         int NumTrees { get; }
         ITree[] GetTrees();
     }
     public static class NodeKeys {
         public const string Extras = "Extras";
         public const string GainValue = "GainValue";
         public const string LeafValue = "LeafValue";
         public const string PreviousLeafValue = "PreviousLeafValue";
         public const string SplitGain = "SplitGain";
         public const string SplitName = "SplitName";
         public const string Threshold = "Threshold";
     }
 }
```


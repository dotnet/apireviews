# API Review 2016-02-02

This API review was also recorded and is available on [Google Hangouts](https://plus.google.com/events/c1544j6igbh6g9cg333u9l99hno)

## Overview

We reviewed the following issues:

* [Notes](#3423-a-replacement-api-for-datareadergetschematable) | [#3423](https://github.com/dotnet/corefx/issues/3423) A replacement API for DataReader.GetSchemaTable()
* [Notes](#1039-why-is-datatableviewset-absent) | [#1039](https://github.com/dotnet/corefx/issues/1039) Why is DataTable/View/Set absent?

## Notes

### #3423 A replacement API for DataReader.GetSchemaTable()

Status: **Needs work** |
[Proposal](https://gist.githubusercontent.com/saurabh500/d27f5fcbfa7bcf12623b/raw/8715f08e49f1e18a5d52208eb64fa7e1cd187371/SchemaProposal.md) |
[Issue](https://github.com/dotnet/corefx/issues/3423) |
[Video](https://plus.google.com/events/c1544j6igbh6g9cg333u9l99hno)

The proposal is available in [this Gist](https://gist.githubusercontent.com/saurabh500/d27f5fcbfa7bcf12623b/raw/8715f08e49f1e18a5d52208eb64fa7e1cd187371/SchemaProposal.md).

* Independent of whether we bring `DataTable`, there is a desire to make the core of ADO.NET not having to depend on data table APIs
* Currently, the only way to get detailed schema information for a given query is via an API that returns a data table.
* The proposal addresses this by exposing a new API that returns schema information as a collection of `DbColumn`, which is a new type that would hold schema information
* Our goal is add this new functionality while being able to author libraries that can run on both, the .NET Framework as well as .NET Core
* This means we can add new types (which would live in an assembly that the consuming code would have to deploy with their .NET Framework app) but we can't change the implementation of types or add new methods on types that are already exposed in the .NET Framework
* Conceptually, schema information is a provider specific concept, in other words SQL, MySQL, and Oracle all have different implementations. Hence, the most natural way to expose the API is as a virtual method on `DbReader`. However, given the previous constraints we can't do this today.
* We concluded that we don't like the proposed registration concept as this feels quite heavyweight and fairly complicated.
* Instead, we're thinking of the following approach:
    - We'll expose a new type that provides an extension method for `DbReader`. Given an instance, it will produce a `ReadOnlyCollection<DbColumn>`
    - We'll add a new interface that represents the concept of getting schema information and has the same signature as the extension method.
    - We'll have two different implementations for the extension method:
        1. The .NET Framework version will be implemented by calling the existing method that returns the schema information via a `DataTable`.
        2. On .NET Core, we rely on the `DbReader` derivatives to implement the interface.
    - In the next version of the .NET Framework/.NET Core we'll add a virtual to `DbReader` has the same signature as the extension method. In order to do so, the .NET Core implementers need to implement this interface explicitly.
* Next steps: Saurabh will update the proposal

### #1039 Why is DataTable/View/Set absent?

Status: **Needs Work** |
[Issue](https://github.com/dotnet/corefx/issues/1039) |
[Video](https://plus.google.com/events/c1544j6igbh6g9cg333u9l99hno)

We also briefly talked about the future of `DataTable` and `DataSet`. Nick mentioned that he recently tried to port them to .NET Core by using the reference source. He echoed what the ADO.NET team said earlier: the code base is quite large and supports many complicated scenarios, such as in-memory transactions and validating the schema via XML.

In the context of .NET Core, it seems these are scenarios most folks would care about:

* Schema information (replaced by the proposal above)
* Representation for tabular data, such as parsed CSV
* Performing bulk insert operations

We talked about these options:

1. We could decide not to port them to .NET Core
2. We could decide to port them as-is to .NET Core, in a legacy package, similar to what we do for non-generic collections
3. We could subset `DataTable` and `DataSet` that avoids their complexities but still addresses the core scenarios

Of course, (1) and (2) doesn't prevent us from designing a disjoint API, such as `AwesomeDataTable`.

The sentiment of (3) is that it's unclear what the benefit would be; the question is how much of the code using data tables would be able to port to .NET Core. In other words, how impactful the subsetting ends up being.

It seems we need:

* Gain more clarity on the scenarios that `DataTable` / `DataSet` are addressing
* Gather some data on which functionality is most commonly used by apps

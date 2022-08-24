# API Review 08/24/2022

##  Intermediate Generator outputs

**NeedsWork** | [#roslyn/63291](https://github.com/dotnet/roslyn/issues/63291#issuecomment-1226474936)

### API Review

* The razor source generator don't just produce C#, it produces semantic info the editor needs to run
* Razor tooling wants not just razor source, but even the generated C# source. With the suppressed generator today, this isn't possible.
* We need to think about OOP in this API, since we may need to communicate between the generator running in a different process and the tooling running in the same process.
  * Would maybe need to serialize/deserialize in the same process, but we already say this is necessary for C# source
* Are these just a non-source artifact?
  * Intermediate might not be the right word in this case?
  * Maybe `AddNonSourceOutput`?
* Could a user just get a stage of the pipeline and see the output of that stage?
  * May be ok, but it might also not be that the intermediate step has the exact thing you want
  * Serialization concerns for this.
* Razor already needs serialization for tag helpers, so perhaps it doesn't matter that we start with just strings.
* We do want to think about some of these intermediate outputs not needing to be written to disk. These intermediate pieces of info don't need to be written to disk for razor, but might for generators that want to write config files.
* We could also look into allowing outputs to be `ROS<byte>` or similar, and users can serialize and rehydrate however they choose.


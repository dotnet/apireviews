# API Review 10/12/2023

## Deprecate syntax serialization logic.

**Approved** | [#roslyn/70275](https://github.com/dotnet/roslyn/issues/70275#issuecomment-1760354738)

### API Review

* We're ok with this deprecation
* We'll try to get an `Obsolete(warning: true)` and a NFW on usage into 17.8 so we can be more sure on whether this is in use or not.

**Conclusion**: Approved
## IFieldSymbol.AssociatedSymbol should return the IParameterSymbol of the primary constructor parameter, if the field is a capture

**Approved** | [#roslyn/70208](https://github.com/dotnet/roslyn/issues/70208#issuecomment-1760358078)

### API Review

* Should we have `AssociatedPrimaryConstructorParameter`?
    * We've documented the existing API as possibly changing in the future for new features.
    * Would require polyfilling on analyzers that support more than just the current Roslyn level.

**Conclusion**: Approved
## Add support for respecting custom configured severity settings in editorconfig

**Approved** | [#roslyn/52991](https://github.com/dotnet/roslyn/issues/52991#issuecomment-1760359293)

### API Review

* Generally looks good
* Let's rename `CustomConfigurable` to `CustomSeverityConfigurable` to be more specific about what is configurable

**Conclusion** : Approved

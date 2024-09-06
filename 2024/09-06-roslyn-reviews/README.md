# API Review 09/06/2024

## API: Analyzer redirecting

**NeedsWork** | [#roslyn/74989](https://github.com/dotnet/roslyn/issues/74989#issuecomment-2334871473)

### API Review

* What assembly should this go in?
    * Workspaces may be nice?
    * Does the IDE need to be able to know the real paths involved?
    * Will VBCS compiler need to do the mapping as well?
        * We can't currently think of a reason
    * Seems like using in the compiler layer outside of VS might break MSBuild incremental builds?
    * If it goes in the workspaces layer, we can potentially make it internal, which seems like a nice benefit.
* Should it be async?
* Single, full paths aren't great.
    * Should we try to design something for multiple?
    * There is an implicit contract that everything in one folder needs to redirect to another folder.
    * The assembly load would already work fine if they end up in different ALCs, because of backcompat requirements.

**Conclusion**: Investigate making the API internal in the workspaces layer.

# API Review 02/27/2023

## Productize QuickGrid

**Approved** | [#aspnetcore/46789](https://github.com/dotnet/aspnetcore/issues/46789#issuecomment-1446794160)

API Review Notes:

- `SortDirection? IsDefaultSort` is a weird name for a non bool. Can we make these two different properties? Non-default columns can have a default direction, right?
  - We decided that we should keep these together for convenience, even though there is no other easy way to change the sort direction of a PropertyColumn. We assume it mostly matters only if the grid is sorted by that column by default. Otherwise you can click twice.
  - Let's rename to `InitialSortDirection` though, and it being non-null will still imply it's the default sort.
    - We hope that it's still 
    - Is there an error if this is set on multiple columns? Can we make the default sorted column a property of the grid instead of the column so you cannot set this on multiple column? How would you reference the column? Index? Name? Is there a name?
  - Upon further discussion. Let's add an `IsDefaultSortColumn` bool too, and make `InitialSortDirection` non-nullable.
  - We don't want this to be visible for PropertyColumn because there's already a sort.
- Can we do `IReadOnlyDictionary<string, object?> Attributes` on the grid? Yes, but not on the column because it might be too inefficient to do per cell.
- Align vs Alignment. Do we want to prefer a noun to a verb for an enum? No. It seems we prefer the shorter name.
  - Is the default alignment really left? What if I want to leave it up to the parent CSS? Should it be nullable? Or have an Auto?
  - What about "Left", "Center", "Right"? In CSS, to support left-to-right languages, they also use "Start" and "End" in some places. Let's support "Start" and "end" as well as "Left" and "Right" for better globalization support.
    - https://developer.mozilla.org/en-US/docs/Web/CSS/text-align
  - Why not just wash our hands of handling the alignment of the columns and make people add their own custom CSS?
    - This might require too much knowledge about the grid's CSS classes and CSS in general to use.
  - Do we want to align headers or numbers differently from other cell types? Not for now.
- Does GridItemsProviderResult need to have settable properties? Let's use init like we did for the request.
- Why doesn't ItemsPerPage return a Task like the method to change the index? Because we want it to easily be bindable from some value. Making ItemsPerPage init-only would also break binding.
  - You could create a SetItemsPerPageAsync method and bind to that, but it's WAAAAY more verbose.
- Should we rename `Paginator.Value` to `Paginator.State`? Sure. It's not used by all consumers of the grid, so it shouldn't break everyone. And it's a little more descriptive.
- Can we move SortBy to the ColumnBase? Yes. Most people don't need it for `PropertyColumn`, but it could make sense sometimes. **NOTE**. Make sure that `PropertyColumn` can still have a reasonable default value for `SortBy`.
- Are we okay with `EditorBrowsableState.Never`? Sure. They're hidden in the infrastructure namespace.
- Do we want to keep `[Parameter] public bool ResizableColumns { get; set; }`? It could be a bug farm since it's complicated. It would need a lot more knobs to super useful. Column sizes won't be remembered. We could add it back later if we want. Let's remove it for now.
- **TODO:** Make sure we copy the arrow direction for ascending and descending columns from windows explorer or excel.

API Approved!

```cs
// Microsoft.AspNetCore.Components.QuickGrid.dll

namespace Microsoft.AspNetCore.Components.QuickGrid;

public partial class QuickGrid<TGridItem> : ComponentBase, IAsyncDisposable
{
    public QuickGrid();
    [Parameter] public IQueryable<TGridItem>? Items { get; set; }
    [Parameter] public GridItemsProvider<TGridItem>? ItemsProvider { get; set; }
    [Parameter] public IReadOnlyDictionary<string, object?> Attributes { get; set; }
    [Parameter] public string? Theme { get; set; }
    [Parameter] public RenderFragment? ChildContent { get; set; }
    [Parameter] public bool Virtualize { get; set; }
    [Parameter] public float ItemSize { get; set; }
    [Parameter] public Func<TGridItem, object> ItemKey { get; set; }
    [Parameter] public PaginationState? Pagination { get; set; }
    protected override Task OnParametersSetAsync();
    protected override async Task OnAfterRenderAsync(bool firstRender);
    public Task SortByColumnAsync(ColumnBase<TGridItem> column, SortDirection direction);
    public Task ShowColumnOptions(ColumnBase<TGridItem> column);
    public async Task RefreshDataAsync();
    public async ValueTask DisposeAsync();
}

public enum Align
{
    Start,
    Center,
    End,
    Left,
    Right,
}

public abstract partial class ColumnBase<TGridItem> : ComponentBase
{
    public ColumnBase();
    [Parameter] public string? Title { get; set; }
    [Parameter] public string? Class { get; set; }
    [Parameter] public Align Align { get; set; }
    [Parameter] public RenderFragment<ColumnBase<TGridItem>>? HeaderTemplate { get; set; }
    [Parameter] public RenderFragment? ColumnOptions { get; set; }
    [Parameter] public bool? Sortable { get; set; }
    [Parameter] public SortDirection InitialSortDirection { get; set; }
    [Parameter] public bool IsDefaultSortColumn { get; set; }
    [Parameter] public GridSort<TGridItem>? SortBy { get; set; }
    [Parameter] public RenderFragment<PlaceholderContext>? PlaceholderTemplate { get; set; }
    public QuickGrid<TGridItem> Grid;
    protected internal abstract void CellContent(RenderTreeBuilder builder, TGridItem item);
    protected internal RenderFragment HeaderContent { get; protected set; }
    protected virtual bool IsSortableByDefault();
    protected override void BuildRenderTree(RenderTreeBuilder builder);
}

public delegate ValueTask<GridItemsProviderResult<TGridItem>> GridItemsProvider<TGridItem>(
    GridItemsProviderRequest<TGridItem> request);

public readonly struct GridItemsProviderRequest<TGridItem>
{
    public int StartIndex { get; init; }
    public int? Count { get; init; }
    public ColumnBase<TGridItem>? SortByColumn { get; init; }
    public bool SortByAscending { get; init; }
    public CancellationToken CancellationToken { get; init; }
    public IReadOnlyCollection<SortedProperty> GetSortByProperties();
    public IQueryable<TGridItem> ApplySorting(IQueryable<TGridItem> source);
}

public readonly struct SortedProperty
{
     public string PropertyName { get; required init; }
     public SortDirection Direction { get; init; }
}

public struct GridItemsProviderResult<TGridItem>
{
    public ICollection<TGridItem> Items { get; required init; }
    public int TotalItemCount { get; init; }
}

public static class GridItemsProviderResult
{
    public static GridItemsProviderResult<TGridItem> From<TGridItem>(ICollection<TGridItem> items, int totalItemCount);
}

public sealed class GridSort<TGridItem>
{
    public static GridSort<TGridItem> ByAscending<U>(Expression<Func<TGridItem, U>> expression);
    public static GridSort<TGridItem> ByDescending<U>(Expression<Func<TGridItem, U>> expression);
    public GridSort<TGridItem> ThenAscending<U>(Expression<Func<TGridItem, U>> expression);
    public GridSort<TGridItem> ThenDescending<U>(Expression<Func<TGridItem, U>> expression);
}

public class PaginationState
{
    public PaginationState();
    public int ItemsPerPage { get; set; }
    public int CurrentPageIndex { get; }
    public int? TotalItemCount { get; }
    public int? LastPageIndex { get; }
    public event EventHandler<int?>? TotalItemCountChanged;
    public override int GetHashCode();
    public Task SetCurrentPageIndexAsync(int pageIndex);
}

public partial class Paginator : ComponentBase, IDisposable
{
    public Paginator();
    [Parameter, EditorRequired] public PaginationState State { get; set; }
    [Parameter] public RenderFragment? SummaryTemplate { get; set; }
    public void Dispose();
    protected override void OnParametersSet();
    protected override void BuildRenderTree(RenderTreeBuilder builder);
}

public class PropertyColumn<TGridItem, TProp> : ColumnBase<TGridItem>, ISortBuilderColumn<TGridItem>
{
    [Parameter, EditorRequired] public Expression<Func<TGridItem, TProp>> Property { get; set; }
    [Parameter] public string? Format { get; set; }
    protected override void OnParametersSet();
    protected internal override void CellContent(RenderTreeBuilder builder, TGridItem item);
}

public enum SortDirection
{
    Auto,
    Ascending,
    Descending,
}

public class TemplateColumn<TGridItem> : ColumnBase<TGridItem>, ISortBuilderColumn<TGridItem>
{
    public TemplateColumn();
    [Parameter] public RenderFragment<TGridItem> ChildContent { get; set; }
    protected internal override void CellContent(RenderTreeBuilder builder, TGridItem item);
    protected override bool IsSortableByDefault();
}

public interface IAsyncQueryExecutor
{
    bool IsSupported<T>(IQueryable<T> queryable);
    Task<int> CountAsync<T>(IQueryable<T> queryable);
    Task<T[]> ToArrayAsync<T>(IQueryable<T> queryable);
}
```

```cs
// Microsoft.AspNetCore.Components.QuickGrid.dll

namespace Microsoft.AspNetCore.Components.QuickGrid.Infrastructure;

[EditorBrowsable(EditorBrowsableState.Never)]
public sealed class ColumnsCollectedNotifier<TGridItem> : IComponent
{
    public void Attach(RenderHandle renderHandle);
    public Task SetParametersAsync(ParameterView parameters);
}

[EditorBrowsable(EditorBrowsableState.Never)]
public sealed class Defer : ComponentBase
{
    public Defer();
    public RenderFragment? ChildContent { get; set; }
}

[EventHandler("onclosecolumnoptions", typeof(EventArgs), enableStopPropagation: true, enablePreventDefault: true)]
public static class EventHandlers
{
}
```

### New API in an existing namespace:

```diff
// Microsoft.AspNetCore.Components.QuickGrid.EntityFrameworkAdapter.dll

namespace Microsoft.Extensions.DependencyInjection;

+public static class EntityFrameworkAdapterServiceCollectionExtensions
+{
+    public static void AddQuickGridEntityFrameworkAdapter(this IServiceCollection services);
+}
```

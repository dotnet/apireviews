# Task Dialog

Status: **Review not complete** |
[Video](https://youtu.be/BvWt4KumHD0?t=142) |
[Issue](https://github.com/dotnet/winforms/issues/146) |
[API Ref](TaskDialog.md)

## Notes

* Using the `System.Windows.Forms` namespace seems fine, that's consistent with
  the other WinForms controls
    - Might make it harder to reuse from WPF because importing that namespace
      will bring a bunch of other constructs
* `TaskDialog.Show()` should be renamed to `ShowDialog()` to match existing
  terms
* We should investigate whether extending `CommonDialog` makes sense here/is
  possible
* `TaskDialog` doesn't have a collection of pages but only a single property
  because only a single page is visible at any given time.
* `TaskDialog.Opened` -- we should align the naming with whatever event
  corresponds with that on `Form`.
* `TaskDialog.Opened` and `TaskDialog.Closed` seem to correspond to
  `TaskDialogPage.Created` and `TaskDialogPage.Destroyed` we should align the
    - Investigate what naming makes most sense across the entire code, maybe do
      a small user study naming.
* Rename `instruction` to `mainInstruction`
* Rename `title` to `caption`
* Can we remove `TaskDialogStandardIcon` and replace the enum values with static
  fields on `TaskDialogIcon`. Also instead of implicit operators, can we just
  have a constructor that takes an `Icon`?
    - Same for `TaskDialogStandardButton`.
* Remove `TaskDialogButtonClickedEventArgs`, i.e. make `TaskDialogButton`
* If we want to implement the WinForms Designer support for designing
  TaskDialog, then we should avoid using methods, and use properties instead.
    - We will consider adding designer support in .NET 5.0
    - Allow users to design the visuals and the flow between the pages in the
      designer and implement everything else programmatically

## Next Steps

* Address feedback from today
* Conclude user study on APIs
* Do the next cycle of reviews with users feedback

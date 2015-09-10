# API Review 2015-09-08

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#3025-secureexception-proposal) | [#3025](https://github.com/dotnet/corefx/issues/3025) SecureException proposal
* [Notes](#1182-systemlinq-performance-improvement-suggestions) | [#1182](https://github.com/dotnet/corefx/issues/1182) System.Linq performance improvement suggestions
* [Notes](#1244-add-overloads-to-string-trimming) | [#1244](https://github.com/dotnet/corefx/issues/1244) Add overloads to string trimming
* [Notes](#1661-add-paramvaluetostring-into-argumentexception-message) | [#1661](https://github.com/dotnet/corefx/issues/1661) Add ParamValue.ToString into ArgumentException message
* [Notes](#1942-proposal-dictionarytkey-tvaluetryaddtkey-tvalue) | [#1942](https://github.com/dotnet/corefx/issues/1942) Proposal: Dictionary\<TKey, TValue>.TryAdd(TKey, TValue)
* [Notes](#2056-review-webspecific-encoding-apis) | [#2056](https://github.com/dotnet/corefx/issues/2056) Review web-specific encoding APIs
* [Notes](#2996-systemglobalization-expose-datetimeformat-getshortestdayname-to-net-core-contract) | [#2996](https://github.com/dotnet/corefx/issues/2996) System.Globalization: Expose DateTimeFormat GetShortestDayName to .NET Core contract

We also took a look at our [backlog](#backlog).

There are also some [follow-ups for the API review board](#follow-ups-for-api-review-board).

## Notes

### #3025 SecureException proposal

Status: **Won't Fix** |
[Issue](https://github.com/dotnet/corefx/issues/3025) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* After a lot of discussion with various folks the consensus is that the proper
  fix is for the app models to register a global unhandled exception handler and
  log the exception information in a safe way.
* That's, for example, the way ASP.NET has addressed this.
* This doesn't prevent other parts of the application to leak information, but
  neither does this proposal. If the most useful piece of information is in
  `PrivateMessage` then that's most likely the part that would be leaked, too.
* One could construct an argument that says that this proposal helps the
  developer think about the contents of their exceptions as being potentially
  sensitive, but that win seems marginal compared to the complexity and impact
  it has on library authors.
* Hence, we concluded **won't fix**.

### #1182 System.Linq performance improvement suggestions

Status: **Should be split** |
[Issue](https://github.com/dotnet/corefx/issues/1182) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* We've done some PRs that were related
* We need to identify the pending work items, if there any.
* We should probably break this down @stephentoub will take a stab.

### #1244 Add overloads to string trimming

Status: **Blocked by initiator** |
[Issue](https://github.com/dotnet/corefx/issues/1244) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* Feedback was given, update is pending

### #1661 Add ParamValue.ToString into ArgumentException message

Status: **Duplicate** |
[Issue](https://github.com/dotnet/corefx/issues/1661) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* Looks a like dupe, @stephentoub will follow up.

### #1942 Proposal: Dictionary\<TKey, TValue>.TryAdd(TKey, TValue)

Status: **Not reviewed** |
[Issue](https://github.com/dotnet/corefx/issues/1942) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* Should we provide APIs for similar situations?
* Split out the `GetOrAdd` feature

### #2056 Review web-specific encoding APIs

Status: **Completed** |
[Issue](https://github.com/dotnet/corefx/issues/2056) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* Closed

### #2996 System.Globalization: Expose DateTimeFormat GetShortestDayName to .NET Core contract

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/2996) |
[Video](https://plus.google.com/events/c9fkhcs150l69aa41j4nekg2ilk)

* Should be done
* However, we need to think about the necessary contract/facade updates for the
  .NET Framework

### Backlog

We've walked the [first page](backlog) of all issues that were marked as 'API
addition' but not marked as 'ready for API review'.

[backlog]: https://github.com/dotnet/corefx/issues?q=is%3Aopen+is%3Aissue+label%3A%22api+addition%22+-label%3A%22ready+for+api+review%22+sort%3Acreated-asc

* [#461](http://github.com/dotnet/corefx/issues/461) --> **Close**. It's not something we'd do but it could be a `corefxlabs` project (too much policy)
* [#490](http://github.com/dotnet/corefx/issues/490) --> **Needs a proposal**. We need a proposal -- Matt Ellis thinks that there is work in progress
* [#492](http://github.com/dotnet/corefx/issues/492) --> **Close**. We're not interested to pursue this now. Initiator could start a project in `corefxlabs`, though.
* [#493](http://github.com/dotnet/corefx/issues/493) --> **Merge**. Should be a Linq performance request, which is [#1182](http://github.com/dotnet/corefx/issues/1182) (if they aren't handled today).
* [#536](http://github.com/dotnet/corefx/issues/536) --> **Needs a proposal**
* [#538](http://github.com/dotnet/corefx/issues/538) --> **Needs a proposal**
* [#574](http://github.com/dotnet/corefx/issues/574) --> **Review**
* [#644](http://github.com/dotnet/corefx/issues/644) --> **Review**
* [#647](http://github.com/dotnet/corefx/issues/647) --> **Close**. Not a core library and should live on top. Of course, if there is any work required to make exchanging types easier, we can address that.
* [#649](http://github.com/dotnet/corefx/issues/649) --> **Review**. We should make a decision whether we should add new methods (`EqualsIgnoreCase`)
* [#699](http://github.com/dotnet/corefx/issues/699) --> **Needs more data**. How does ASP.NET use it? Feel free to open a `corefxlab` project.

### Follow-ups for API review board

* Make a decisions whether `BitVector32` is a legacy type

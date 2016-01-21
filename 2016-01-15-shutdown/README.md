# API Review 2016-01-12

This API review was also recorded and is available on [Google+](https://plus.google.com/events/c147l9eb5ibs2br2aciqvqlapr4).

## Overview

In this API review we reviewed the following PRs and proposals:

* [Notes](#5205-during-shutdown-revisit-finalization-and-provide-a-way-to-clean-up-resources) | [#5205](https://github.com/dotnet/corefx/issues/5205) During shutdown, revisit finalization and provide a way to clean up resources

## Notes

### #5205 During shutdown, revisit finalization and provide a way to clean up resources

Status: **Approved** |
[Issue](https://github.com/dotnet/corefx/issues/5205) |

* We'd change the semantics to run finalizers as a best effort during shutdown
* If customers need to run code on shutdown, they need to subscribe to an event
* Desktop:
    - If you crash, we may not run all finalizers
    - If you have a deadlock, we've a 40 seconds timeout
* CoreCLR:
    - We got rid of the timeout because it's too long for smaller devices, such as phone

* Running threads is problematic
* Should we have a special type?

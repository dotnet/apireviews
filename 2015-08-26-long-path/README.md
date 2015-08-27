# API Review 2015-08-27

This API review was also recorded and is available on Google+:

* [Part 1](https://plus.google.com/events/c3bs3mqme23isi005bhlcv6nu68)
* [Part 2](https://plus.google.com/events/ccj5m9oheonldrlp2hg59mhevtk)

## Overview

In this API review we reviewed [a proposal](NET API FullPath Proposal.md).

## .NET API Proposal for handling full paths

Status: **Design in progress** |
[Video 1](https://plus.google.com/events/c3bs3mqme23isi005bhlcv6nu68) |
[Video 2](https://plus.google.com/events/ccj5m9oheonldrlp2hg59mhevtk)


* Apps that are broken today, will continue to be broken regardless of whether
  we fix that issue or not.
* No risk of causing buffer overruns as the Win32 APIs already take buffer sizes
  and apps pass in `MAX_PATH`, which will cause them to get "buffer too small"
  error.
* The Win32 API `GetFinalPath` fixes casing issues. It seems we need a new API
  that allows us to get the canonical path that hits the disk and follows links
  and fixes casing
* Should `GetFullPath` expand short file names (xxxx~x) to long names?
    - It might be better to move this stuff to this new API instead of
      doing this from `GetFullPath`
    - *Note:* this might be a behavioral breaking change in .NET Framework
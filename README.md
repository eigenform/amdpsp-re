# amdpsp-re

Tools for looking at AMD Platform Security Processor (PSP) binaries, AGESA
firmware images, and other observations about AMD platform security.

- [docs/](./docs) - Notes on various things
- [bin/](./bin) - Scripts

This is really only relevant to firmware images for Family 17h platforms, 
mostly because at some point (probably with the release of Family 19h?), many 
[but not all?] PSP-related binaries embedded in AGESA BIOS images started being 
distributed as encrypted blobs.

## Prior Art

This work is mostly informed by the 2019 TU-Berlin research targeting Zen 1 
machines. For more details, see [PSPReverse/psp-docs](https://github.com/PSPReverse/psp-docs).



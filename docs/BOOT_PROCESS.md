# Boot Process

The boot process, as far as I can tell.

Most of the process relies on code/data stored in SPI flash memories. 
AGESA firmware images have one or more [potentially nested] "PSP directory" 
tables with different executables or other data used during the process.

PSP directory entries are identified with 8-bit IDs, indicating the
*kind* of entry (a particular piece of data, a particular program, etc). 
The actual entry data typically starts with a 256-byte header which includes 
(among other things): 

- Flags indicating whether or not the data is signed/compressed/encrypted
- The size of the data
- A version number
- A load address
- Other things I don't understand yet

## First-stage Bootloader

The boot ROM starts at the high reset vector `0xffff_0000`. 
This is probably fused to at least one die on the package (not clear).

I don't have a copy of the boot ROM, but you'd expect this is mostly just
responsible for loading the second-stage bootloader (entry ID `0x01` in a
PSP directory) from the flash.

PSP directory entries typically start with a 256-byte header which includes
(among other things) a load address. The second-stage bootloader is copied 
into SRAM at physical address `0x0000_0100`. Presumably, the boot ROM 
configures the base of the exception vectors here, and then enters the second 
stage by taking a reset exception.

## Second-stage Bootloader

The second-stage bootloader starts at the reset vector `0x0000_0100`.

At this point, there are quite a lot of different stages to initializing the 
platform: many depend on loading data and other programs from the PSP directory
on flash. 

Initially, the MMU is configured to enforce some distinction between:

- Memory regions reserved for use by the second-stage bootloader
- Memory regions reserved for other code/data loaded from flash

These other programs are typically loaded and entered at `0x0001_5000` in 
unprivileged mode. On top of this, the `SVC` exception handler implements a 
syscall interface exposed to unprivileged programs (via the `SVC` instruction).

Execution in the second-stage bootloader also seems to rely on the particular 
"boot mode" (i.e. a cold boot vs. various types of warm boot).

...

### AGESA Bootloader (ABL) Stages

Dispatched by the second-stage bootloader. Haven't really looked at these yet.

On the surface, looks like these are responsible for initializing 
microcontrollers responsible for the various memory-related PHYs, memory 
training, and MBIST?

...

## TEE/Trusted OS 

Haven't looked at this at all. Presumably runs applications that handle 
requests from x86-land (UEFI firmware and post-boot)?

...


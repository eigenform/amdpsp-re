# PSP AXI Memory Map

The PSP sits on a dedicated AXI bus along with local memories and peripherals.
MMIO registers also expose a configurable bridge to the SMN and SYSHUB
address spaces.

```
Physical address range       Description
----------------------------------------------------
[0x0000_0000 - 0x0004_ffff]: 320KiB SRAM (for Zen2?)

[0x0100_0000 - 0x02ff_ffff]: 32 1MiB-wide SMN "apertures"
	[0x0100_0000 - 0x010f_ffff]: SMN window 00
	[0x0110_0000 - 0x011f_ffff]: SMN window 01
	[0x0120_0000 - 0x012f_ffff]: SMN window 02
	[0x0130_0000 - 0x013f_ffff]: SMN window 03
	...
	[0x02f0_0000 - 0x02ff_ffff]: SMN window 31

[0x0300_0000 - 0x03ff_ffff]: Memory-mapped registers
	[0x0300_1000 - ???????????]: 
	[0x0300_6000 - ???????????]: 
	[0x0301_0000 - ???????????]: 
	[0x0302_0000 - ???????????]: 
	[0x0320_0000 - ???????????]: 
		[0x0320_0048]: Boot ROM revision id? (bootloader)
		[0x0320_00a8]: Panic handler stores something here?
		[0x0320_02f0]: ???
		[0x0320_02f4]: ???

	[0x0321_0000 - ???????????]: 
	[0x0322_0000 - 0x0322_ffff]: SMN Aperture Configuration
	[0x0323_0000 - 0x0323_ffff]: SYSHUB Aperture Configuration 
	[0x0326_0000 - ???????????]: 

[0x0400_0000 - 0xfbff_ffff]: 62 64MiB-wide SYSHUB "apertures"
	[0x0400_0000 - 0x07ff_ffff]: SYSHUB window 00
	[0x0800_0000 - 0x0bff_ffff]: SYSHUB window 01
	[0x0c00_0000 - 0x0fff_ffff]: SYSHUB window 02
	[0x1000_0000 - 0x13ff_ffff]: SYSHUB window 03
	...
	[0xf800_0000 - 0xfbff_ffff]: SYSHUB window 61

[0xfc00_0000 - 0xfffe_ffff]: ???
[0xffff_0000 - 0xffff_ffff]: On-die mask ROM (on-chip bootloader)
```

# PSP Virtual Memory

The bootloader sets up second-level 4KiB page table entries at `0x0001_4c00`, 
used to make a distinction between privileged and unprivileged regions in SRAM
while the bootloader is running other programs from SPI flash.

Most are identity mappings, except for `0x0004_d000 - 0x0004_efff`, which
is mapped to `0x0006_0000 - 0x0006_1fff` virtual?

```
Physical address range       Description
----------------------------------------------------
[0x0000_0000 - 0x0001_0000]: Bootloader code (PL1 R/O)
[0x0001_0000 - 0x0001_5000]: Bootloader data (XN, PL1 R/W)
[0x0001_5000 - 0x0004_9fff]: Application code/data (PL1 R/W, PL0 R/W)
[0x0004_a000 - 0x0004_afff]: TODO (XN, PL1 R/W)
[0x0004_b000 - 0x0004_cfff]: TODO (XN, PL1 R/W)
[0x0004_d000 - 0x0004_efff]: TODO (XN, PL1 R/W)
[0x0004_f000 - 0x0004_ffff]: TODO (XN, PL1 R/W, PL0 R/O)
```


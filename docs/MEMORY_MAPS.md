# Memory Maps

## AXI Memory Map

The PSP issues memory accesses over an AXI bus. The memory map looks like:

```
Physical address range       Description
----------------------------------------------------
[0x0000_0000 - 0x0004_ffff]: 320KiB SRAM (for Zen2?)
[0x0100_0000 - 0x02ff_ffff]: 32 1MiB-wide SMN "apertures"
[0x0300_0000 - 0x03ff_ffff]: Memory-mapped registers
[0x0400_0000 - 0xfbff_ffff]: 62 64MiB-wide SYSHUB "apertures"
[0xfc00_0000 - 0xfffe_ffff]: ???
[0xffff_0000 - 0xffff_ffff]: On-die mask ROM (on-chip bootloader)
```

### PSP Virtual Memory

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

### Memory-mapped I/O Registers

```
Physical address range       Description
----------------------------------------------------
[0x0300_1000 - ???????????]: 
[0x0300_6000 - ???????????]: 
[0x0301_0000 - ???????????]: 
[0x0302_0000 - ???????????]: 
[0x0320_0000 - ???????????]: 
[0x0321_0000 - ???????????]: 
[0x0322_0000 - 0x0322_ffff]: SMN Aperture Configuration
[0x0323_0000 - 0x0323_ffff]: SYSHUB Aperture Configuration 
[0x0326_0000 - ???????????]: 
```

### SMN Apertures

```
Physical address range       Description
----------------------------------------------------
[0x0100_0000 - 0x010f_ffff]: SMN window 00
[0x0110_0000 - 0x011f_ffff]: SMN window 01
[0x0120_0000 - 0x012f_ffff]: SMN window 02
[0x0130_0000 - 0x013f_ffff]: SMN window 03
...
[0x02f0_0000 - 0x02ff_ffff]: SMN window 31
```

### SYSHUB Apertures

```
Physical address range       Description
----------------------------------------------------
[0x0400_0000 - 0x07ff_ffff]: x86 window 00
[0x0800_0000 - 0x0bff_ffff]: x86 window 01
[0x0c00_0000 - 0x0fff_ffff]: x86 window 02
[0x1000_0000 - 0x13ff_ffff]: x86 window 03
...
[0xf800_0000 - 0xfbff_ffff]: x86 window 61
```


## System Management Network (SMN)

The 19h 01h PPR has a brief note about the SMN (from the PHY standpoint):

> System Management Network (SMN) is a packetized SOC network with a minimum 
> 8-bit packet width in each direction with either synchronous or source 
> synchronous clocking. It is expected that the packetized width of SMN may be
> adjusted to provide increased bandwidth over a SMN segment as appropriate to 
> meet SOC and IP requirements. Network and IP interface partitioning is such 
> that the IP interface is immune to changes in packet widths or other 
> characteristics of SMN.

Some PPRs include fragments of the SMN memory map for different machines.
Otherwise, these come from usage in PSP binaries.
Some of these are *most likely* specific to a particular family or chipset.

```
Address range               Description
----------------------------------------------------

[0x0001_c000 - ??????????]: ??? Northbridge PCIe register space? (bootloader)
[0x0001_d000 - ??????????]: ??? Northbridge PCIe register space? (bootloader)
[0x0005_0000 - ??????????]: UMC0_CH (17h,71h PPR)
[0x0005_0a00 - ??????????]: UMC-related? (bootloader)
[0x0005_1000 - ??????????]: ??? (bootloader)
[0x0005_9800 - ??????????]: SMU_THM (17h,71h PPR)
[0x0005_a000 - ??????????]: Related to S0i3? (bootloader)
[0x0005_a800 - ??????????]: ??? (bootloader)
[0x0005_d000 - ??????????]: SMU_FUSE (17h,71h PPR)
[0x0015_0000 - ??????????]: UMC1_CH? (17h,71h PPR)
[0x0025_0000 - ??????????]: UMC2_CH?
[0x0035_0000 - ??????????]: UMC3_CH?

[0x0120_0000 - ??????????]: ACP_MMIO (audio coprocessor) (17h,18h PPR)

[0x01b1_0000 - ??????????]: ??? (bootloader)

[0x0240_0000 - ??????????]: IOMMU_MMIO (17h,18h PPR)

[0x02d0_1000 - ??????????]: ??? (bootloader)
[0x02dc_4000 - ??????????]: ??? (bootloader)
[0x02dc_5000 - ??????????]: ??? (bootloader)

[0x0381_0000 - ??????????]: ??? (bootloader)
[0x03b1_0000 - ??????????]: HSMP?
	[0x03b1_0534]: Mailbox - Message ID (19h,01h PPR)
	[0x03b1_0700 - ??????????]: ??? (bootloader)

[0x03c0_0000 - ??????????]: Bootloader copies SMU_FW here?
[0x0900_0000 - ??????????]: AEB/SEV related? (DEBUG_UNLOCK module)
[0x090a_8000 - ??????????]: ??? (bootloader)
[0x0910_0000 - ??????????]: AEB/SEV related? (DEBUG_UNLOCK module)
[0x0c00_0000 - ??????????]: ??? (bootloader)

[0x114c_0000 - ??????????]: NBIO3_PCI_MCA0 (17h,71h PPR)
[0x118c_0000 - ??????????]: MCA_PCIE (17h,71h PPR)
[0x12ee_0000 - ??????????]: XGMI_PCS0 (17h,31h PPR)
[0x12fe_0000 - ??????????]: XGMI_PCS1 (17h,31h PPR)
[0x130e_0000 - ??????????]: XGMI_PCS2 (17h,31h PPR)
[0x131e_0000 - ??????????]: XGMI_PCS3 (17h,31h PPR)
[0x132e_0000 - ??????????]: XGMI_PCS4 (17h,31h PPR)
[0x133e_0000 - ??????????]: XGMI_PCS5 (17h,31h PPR)

[0x136f_0000 - ??????????]: ??? (bootloader)

[0x13e1_0000 - ??????????]: IOHC_MISC3 (17h,71h PPR)
[0x13f0_0000 - ??????????]: IOMMU_CFG (17h,18h PPR)
[0x13f0_1000 - ??????????]: IOMMU_L2B (17h,18h PPR)
[0x1470_0000 - ??????????]: IOMMU_L1_INT0 (17h,18h PPR)
[0x1480_0000 - ??????????]: IOMMU_L1_INT1 (17h,18h PPR)
[0x1570_0000 - ??????????]: IOMMU_L2A (17h,18h PPR)

[0x178f_0000 - ??????????]: ??? (bootloader)
[0x17e0_0680 - ??????????]: FCH_PD_SLV_I2C? (19h, 51h)

[0x2000_2000 - ??????????]: ??? (bootloader)

[0x2035_0000 - ??????????]: L3CRB00 (17h,71h PPR)
[0x2075_0000 - ??????????]: L3CRB01 (17h,71h PPR)
[0x20b5_0000 - ??????????]: L3CRB10 (17h,71h PPR)
[0x20f5_0000 - ??????????]: L3CRB11 (17h,71h PPR)

[0x3000_0000 - ??????????]: AEB related? (DEBUG_UNLOCK module)
[0x3008_1000 - ??????????]: ??? (bootloader)
[0x3008_1800 - ??????????]: CCD00_FUSE_CCD_SMU_FUSE_DEC (17h,71h PPR)
[0x3030_0000 - ??????????]: ??? (bootloader)
[0x3040_0000 - ??????????]: CCD00_MP5_MMIO_PUBLIC (17h,71h PPR)
[0x3050_0000 - ??????????]: Bootloader copies MP5_FW here?
[0x3060_0000 - ??????????]: ??? (bootloader)
[0x307f_0000 - ??????????]: ??? (bootloader)
[0x3208_1800 - ??????????]: CCD01_FUSE_CCD_SMU_FUSE_DEC (17h,71h PPR)
[0x3240_0000 - ??????????]: CCD01_MP5_MMIO_PUBLIC (17h,71h PPR)
```

## SYSHUB (x86-relevant) Memory Map

This is basically the x86-visible physical address space, with the exception
of some restricted space on top (vaguely mentioned in the PPRs)?

These are 64-bit addresses, but the high 16 bits are always zero (AFAICT).

```
Physical address range       Description
----------------------------------------------------
[0x0000_0000_0000 - 0xfffc_ffff_ffff]: Physical memory?
[0xfffd_0000_0000 - 0xfffd_ffff_ffff]: Reserved?
[0xfffe_0000_0000 - 0xfffe_ffff_ffff]: Reserved?
[0xffff_0000_0000 - 0xffff_ffff_ffff]: Reserved?
```

The 17h 20h PPR has some description of the ACPI MMIO region, although it's
not clear if this is exactly the same across the client/server platforms
(probably not):

```
Physical address    Description
----------------------------------------------------
[0x0000_0000_0000]: LPC/SPI ROM

[0x0000_fec0_0000]: FCH::IOAPIC
[0x0000_fec1_0000]: FCH::ITF_SPI
[0x0000_fec2_0000]: FCH::ITF_ESPI
[0x0000_fed0_0000]: FCH::TMR::HPET

[0x0000_fed8_0000]: Base of ACPI MMIO regions
	[0x0000_fed8_0000]: FCH::USBLEGACY
	[0x0000_fed8_0200]: FCH::SMI
	[0x0000_fed8_0300]: FCH::PM
	[0x0000_fed8_0400]: FCH::PM2
	[0x0000_fed8_0500]: "BIOS RAM?"
	[0x0000_fed8_0600]: "CMOS RAM?"
	[0x0000_fed8_0700]: FCH::IO
	[0x0000_fed8_0800]: "ACPI"
	[0x0000_fed8_0900]: "ASF registers"
	[0x0000_fed8_0a00]: "SMBus registers"
	[0x0000_fed8_0b00]: FCH::WDT
	[0x0000_fed8_0c00]: "HPET"
	[0x0000_fed8_0d00]: FCH::IOMUX
	[0x0000_fed8_0e00]: FCH::MISC
	[0x0000_fed8_1000]: "Reserved. Serial Debug bus"
	[0x0000_fed8_1100]: "Shadow System Counter"
	[0x0000_fed8_1200]: Reserved
	[0x0000_fed8_1400]: "DP-VGA"
	[0x0000_fed8_1500]: FCH::GPIO  (bank 0)
	[0x0000_fed8_1600]: FCH::GPIO (bank 1)
	[0x0000_fed8_1700]: FCH::GPIO  (bank 2)
	[0x0000_fed8_1d00]: FCH::TMR::ACDC
	[0x0000_fed8_1e00]: FCH::AOAC

[0x0000_fedc_0000]: AL2AHB?
[0x0000_fedc_1000]: Reserved?
[0x0000_fedc_2000]: Reserved?
[0x0000_fedc_3000]: Reserved?
[0x0000_fedc_4000]: I2C_2 Master
[0x0000_fedc_5000]: I2C_3 Master
[0x0000_fedc_6000]: I2C_4 Slave for USB PD
[0x0000_fedc_7000]: DMAC0
[0x0000_fedc_8000]: DMAC1
[0x0000_fedc_9000]: UART0
[0x0000_fedc_a000]: UART1
[0x0000_fedc_b000]: Reserved?
[0x0000_fedc_c000]: DMA_2
[0x0000_fedc_d000]: DMA_3
[0x0000_fedc_e000]: UART_2
[0x0000_fedc_f000]: UART_3

[0x0000_fedd_5000]: FCH::EMMC::MMIO
[0x0000_fedd_5800]: FCH::EMMC
```



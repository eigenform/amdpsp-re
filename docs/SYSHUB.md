# SYSHUB Address Space

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

Instruction-based Sampling (IBS) can be used to snoop loads/stores associated
with microcoded instructions like `RDMSR`. Doing this reveals a lot of reserved
physical addresses targeted by microcoded loads/stores, ie.

```
// Looks like MCA registers for [off-core] devices are mapped here
[0x0000_fffd_f940_0700 - 0x0000_fffd_f940_0768]: MCA::L3
[0x0000_fffd_f940_0800 - 0x0000_fffd_f940_0868]: MCA::L3
[0x0000_fffd_f940_0900 - 0x0000_fffd_f940_0968]: MCA::L3
[0x0000_fffd_f940_0a00 - 0x0000_fffd_f940_0a68]: MCA::L3
[0x0000_fffd_f940_0b00 - 0x0000_fffd_f940_0b68]: MCA::L3
[0x0000_fffd_f940_0c00 - 0x0000_fffd_f940_0c68]: MCA::L3
[0x0000_fffd_f940_0d00 - 0x0000_fffd_f940_0d68]: MCA::L3
[0x0000_fffd_f940_0e00 - 0x0000_fffd_f940_0e68]: MCA::L3
[0x0000_fffd_f940_0f00 - 0x0000_fffd_f940_0f68]: MCA::MP5
[0x0000_fffd_f940_1000 - 0x0000_fffd_f940_1068]: MCA::PB
[0x0000_fffd_f940_1100 - 0x0000_fffd_f940_1168]: MCA::UMC
[0x0000_fffd_f940_1200 - 0x0000_fffd_f940_1268]: MCA::UMC
[0x0000_fffd_f940_1300 - 0x0000_fffd_f940_1368]: MCA::CS
[0x0000_fffd_f940_1400 - 0x0000_fffd_f940_1468]: MCA::CS
[0x0000_fffd_f940_1500 - 0x0000_fffd_f940_1568]: MSR 0xc000_215{0..d}
[0x0000_fffd_f940_1600 - 0x0000_fffd_f940_1668]: MSR 0xc000_216{0..d} 
[0x0000_fffd_f940_1700 - 0x0000_fffd_f940_1768]: MSR 0xc000_217{0..d}
[0x0000_fffd_f940_1800 - 0x0000_fffd_f940_1868]: MCA::SMU
[0x0000_fffd_f940_1900 - 0x0000_fffd_f940_1968]: MCA::PSP
[0x0000_fffd_f940_1a00 - 0x0000_fffd_f940_1a68]: MSR 0xc000_21a{0..d}
[0x0000_fffd_f940_1b00 - 0x0000_fffd_f940_1b68]: MCA::PIE

[0x0000_fffd_fe01_0c00]: CPUID EAX=0x0000_000b, 0x8000_001e
[0x0000_fffe_0000_0090]: CPUID EAX=0x8000_0007

// These are Core::X86::Msr::{DF_PERF_CTL, DF_PERF_CTR}
[0x0000_fffe_0000_5040 - 0x0000_fffe_0000_5078]: MSR 0xc001024{0..7}

// Undocumented MSRs, probably PMCs for something else?
[0x0000_fffe_0000_5384]: MSR 0xc0002073 (undocumented?)
[0x0000_fffe_0000_5390]: MSR 0xc0010191 (undocumented?)
[0x0000_fffe_0000_5398]: MSR 0xc0010193 (undocumented?)
[0x0000_fffe_0000_53a0]: MSR 0xc0010195 (undocumented?)
[0x0000_fffe_0000_53a8]: MSR 0xc0010197 (undocumented?)
[0x0000_fffe_0000_6080]: MSR 0xc0010190 (undocumented?)
[0x0000_fffe_0000_6088]: MSR 0xc0010192 (undocumented?)
[0x0000_fffe_0000_6090]: MSR 0xc0010194 (undocumented?)
[0x0000_fffe_0000_6098]: MSR 0xc0010196 (undocumented?)
[0x0000_fffe_0000_630c]: MSR 0xc001103a (Core::X86::Msr::IBS_CTL)

[0x0000_ffff_ffff_fe00]: MSR 0xc001_0115 (Core::X86::Msr::IGNNE)
[0x0000_ffff_ffff_fe1e]: MSR 0xc000_010f (undocumented?)
[0x0000_ffff_ffff_fe47]: CPUID EAX=0x0000_0001
[0x0000_ffff_ffff_fe5e]: CPUID EAX=0x0000_000d, 0x8000_001c
[0x0000_ffff_ffff_ff91]: Various MSRs (L3 QOS, L3 PMCs, P-State registers?)
                         `INVD` and `WBINVD` touch this. CPUID EAX=0x8000_0006.
[0x0000_ffff_ffff_ffc2]: MSR 0x0000_00{10,e7} (Core::X86::Msr::{TSC, MPERF}
                        `RDPRU` also touches this.
```



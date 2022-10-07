# Terminological Reference

If you're looking into any of this, you will probably come across a lot of
different acronyms. Here's a couple:

```
# Bus and Interconnect


SDF    - Scalable Data Fabric (sometimes just "DF")
SDP    - Scalable Data Port (data fabric link)
SCF    - Scalable Control Fabric
CS     - Coherent Slave (on the Data Fabric)
CCM    - Cache-coherent Master (ie. x86 cores on the data fabric?)
IOMS   - IO Master/Slave
IFOP   - Infinity Fabric [On-Package]

TWIX   - Intra-socket control fabric interface?
WAFL   - Inter-socket control fabric interface?
CAKE   - Inter-socket data fabric link? ("Coherent AMD socKet Extender")
GMI    - Intra-socket data fabric interface?
XGMI   - Inter-socket data fabric interface?

PIE    - "Power, Interrupts, Etc?"
SMN    - System Management Network
IOHC   - IOHUB Core [Crossbar]
DXIO   - Distributed IO [Crossbar] (PHY muxing?)
NBIO   - Northbridge IO
NBIF   - Northbridge Interface?

GMHUB  - Graphics Memory Hub
MMHUB  - Multimedia Hub
ATHUB  - Address Translation Hub
IOHUB  - IOMMU-mediated interface to the Data Fabric
SYSHUB - Switching/arbitration between I/O devices and data fabric?
         See PPR for AMD Family 19h Model 51h A1:
         > The System Hub is used to connect the high speed IO logic into the
         > system. This allows the high speed IO devices to quickly and 
         > efficiently access the memory controllers on the system while 
         > allowing efficient priority control for preventing any single high 
         > speed interface from dominating the memory controller.

# Components on the *package*
CCD    - Core Complex Die
CCX    - Core Complex (an x86 core)

# Microcontrollers and I/O Devices
ACP    - Audio Co-processor
CCP    - Cryptographic Co-processor
FCH    - Fusion Controller Hub (southbridge)
HSMP   - Host System Management Port (mailbox interface to SMU?)
HSP    - Hardware Security Processor (i.e. Pluton?)
MP0    - Usually refers to the Platform Security Processor (PSP)?
MP1    - Usually refers to the System Management Unit (SMU)?
MP2    - Microprocessor related to the Sensor Fusion Hub (SFH) and I2C?
         I think this is only relevant for APUs (i.e. in laptops)?
MP5    - Microprocessor related to a Base Management Controller? (BMC)
PMU    - PHY Microcontroller
PSP    - Platform Security Processor
SFH    - Sensor Fusion Hub
SMU    - System Management Unit
UMC    - Unified Memory Controller (DDR controller)
SBI    - Sideband Interface
SB-RMI - Sideband [Remote Management] Interface
SB-TSI - Sideband [Thermal Sensor?] Interface

# Security-related
AEB    - Attestation Evidence Broker
ARB    - Anti-Rollback
DRTM   - Dynamic Root for Trusted Measurement
KDS    - Key Distribution System
RPMC   - Replay Protected Monotonic Counter
OTP    - One-time Programmable (Memory)
PDS    - ???
PSB    - Platform Secure Boot
SEV    - Secure Encrypted Virtualization
SNP    - Secure Nested Paging
SOS    - Secure Operating System
SPL    - Secure Patch Level
SVN    - Secure Version Number
TA     - Trusted Application
TCB    - Trusted Compute Base
TMR    - Trusted Memory Region
TOS    - Trusted Operating System
TEE    - Trusted Execution Environment
VCEK   - Versioned Chip Endorsement Key

# AGESA/BIOS-related
ABL    - AGESA Bootloader
AGESA  - AMD Generic Encapsulated Software Architecture
APCB   - AMD PSP Customization Block
APOB   - AGESA PSP Output Buffer
CBS    - Custom BIOS Settings (settings defined by the OEM?)
CPB    - Core Performance Boost?
CRB    - Customer Reference Board
EFS    - Embedded File System (?)

# APU/GPU-related
CP     - Command Processor
CVIP   - Computer Vision Image Processing?
DCHUB  - Display Controller Hub?
DCN    - Display Core Next
DCCG   - Display Controller Clock Generation
SDPIF  - Scalable Data Port Interface

# Generic/Misc.
AXI    - Advanced eXtensible Interface (an AMBA bus protocol)
PHY    - Physical Layer
         You might also see these, referring to different PHY subcomponents:
         PMD - Physical Medium Dependent Layer
         PMA - Physical Medium Attachment Layer
         PCS - Physical Coding Sublayer
SPD    - Serial presence detect (DIMM/RAM discovery?)
NTB    - PCIe "Non-Transparent Bridging"
PTDMA  - PCIe "Passthrough DMA"

```

[^1]: [https://doc.coreboot.org/soc/amd/psp_integration.html]()
[^2]: [https://doc.coreboot.org/soc/amd/family17h.html]()
[^3]: [https://www.coelacanth-dream.com/posts/2020/01/14/amdgpu-abbreviation/]()
[^4]: [https://github.com/amd/firmware_binaries]()
[^5]: [https://www.kernel.org/doc/html/latest/gpu/amdgpu/amdgpu-glossary.html]()
[^6]: [https://en.wikichip.org/wiki/amd/packages/socket_sp3]
[^7]: [https://en.wikichip.org/wiki/amd/infinity_fabric]



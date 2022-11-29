# Asus-Z590-E-Hackintosh


--------------------------------------------------------------------------------------------------------------

# Software
    -   Bootloader: OpenCore 0.8.6-Release
    -   OS: macOS Monterey 12.6.1 (iMac20,2)

--------------------------------------------------------------------------------------------------------------

# Hardware
    -   Motherboard: Asus ROG Strix Z590-E Gaming
    -   CPU: Intel Core i9-10900K
    -   RAM: TEAMGROUP T-Force Xtreem ARGB + G.Skill Trident Z RGB [3600MHz CL14 (4 x 16GB) CL14-15-15-35]
    -   GPU: AMD Radeon Pro WX 4100 + Nvidia RTX 3080 Founders Edition
    -   Wi-Fi/BT: Broadcom BCM94360NG 802.11ac 1200Mbps Bluetooth 4.0
    -   Ethernet: Intel 2.5 Gb Ethernet I225-V
    -   Storage NVME: 1x WD Black SN850 1TB + 1x WD Blue SN570 1TB + 1x Toshiba XG3 1TB
    -   Storage SATA: 2x WD Blue WDS100T2B0A 1TB + 1x WD Blue WDS400T2B0A 4TB
    -   Case: RAIJINTEK Thetis Window ATX Mid Tower Case
    -   PSU: Lian Li SP750 850 W 80+ Gold SFX
    -   CPU Cooler: ID-COOLING FROSTFLOW X 240 Snow
    -   Fans: ARCTIC P12 PWM PST A-RGB 3-pack

--------------------------------------------------------------------------------------------------------------

# BIOS Settings
    -   Version: 1402
    Advanced
    - CPU Configuration
        - Intel (VMX) Virtualization Technology: Enabled (if DisableIoMapper to YES)
        - Hyper-Threading: Enabled
    - System Agent (SA)-Configuration
        - VT-d: Enabled (if DisableIoMapper to YES)
        - Memory Configuration
           - Memory Remap Feature: Enabled (if DisableIoMapper to YES)
        - Graphics Configuration
           - iGPU Multi-Monitor: Enabled (disable if you don't have igpu)
           - DVMT Pre-Allocated: 128 MB (skip if you don't have igpu)
    - APM Configuration
        - ErP Ready to: Enable (S4+S5) (enable full sleep without leds)
    - PCH Storage Configuration
        - SATA Mode Selection: AHCI
    - PCH-FW Configuration
        - PTT: Disabled (this is Intel Platform Trust / TPM)
    - Thunderbolt(TM) Configuration
        - Discrete Thunderbolt(TM) Support: Disabled
    - PCI Subsystem Settings
        - Above 4G Decoding: Enabled
        - Re-Size BAR Support: Disabled
    - USB Configuration
        - Legacy USB Support: Enabled
        - XHCI Hand-off: Enabled
    Boot
    - CSM (Compatibility Support Module)
        - Launch CSM: Disabled (Must be off in most cases, GPU errors/stalls like gIO are common when enabled)
    - Secure Boot
        - OS Type: Windows UEFI mode
        - Key Management
            - Clear Secure Boot Keys: Execute
    - Boot Configuration
        - Fast Boot: Disabled

# Missing From Bios:
    - VT-x
    - Execute Disable Bit
    - Software Guard Extensions (SGX)
    - CFG-Lock (Asus Z590 are factory unlocked. AppleCpuPmCfgLock and AppleXcpmCfgLock quirks are not needed)

--------------------------------------------------------------------------------------------------------------

# What's Working
    -   Intel UHD630 headless mode (iGPU)
    -   AMD Radeon Pro WX 4100 (dGPU)
    -   Audio Realtek ALCS1220A
    -   Intel I225-V 2.5Gb Ethernet
    -   Wi-Fi and Bluetooth (via BCM94360NG by replacing onboard motherboard Wi-Fi card)
    -   USB
    -   Restart/Shutdown
    -   Sleep/Wake
    -   Power Management (Native support)
    -   Handoff
    -   Continuity
# To Test:
    -   Amazon Prime Video and Netflix in Safari
    -   AppleTV
    -   SideCar
    -   Facetime
    -   iMessage
# Not Working
    -   Thunderbolt
    -   iGPU display out (Z590 chipset is not compatilbe with hardware acceleration)

--------------------------------------------------------------------------------------------------------------

# Troubleshooting:
    - Random Freezes/Reboots:
        - Resolution: Remove all samsung drives from the build. Samdung SSD controllers are not compatible with Mac OS seemingly what so ever. If you have any Samsung drives in your system, I recommend you remove them for this build. Having them installed even just as storage drives causes random freezes and crashes.
            - drives tested with: Samsung PM981, Samsung 870 QVO
    - DVMT Pre-Allocated missing in bios
        - Resolution: Power down your computer and move your HDMI or Display Cable to the motherboard ports and power up. Wait for the display to be connected and enter the bios. Now the option should be there.
    - Sleep/Wake Issues
        - Resolution: Recreate your SSDTs for your specific build. I used the Dortania guides for this.
            - https://dortania.github.io/Getting-Started-With-ACPI/ssdt-platform.html#desktop

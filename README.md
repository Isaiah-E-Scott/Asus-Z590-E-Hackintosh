# Asus-Z590-E-Hackintosh Triple Boot Guide

This repository is about hackintosh on Asus ROG STRIX Z590-E. All the hardware is working as expected, and it's ready for daily usage.

Anyone who has the same board can use my EFI directly or with minimal changes. The source EFI folder uses the release version of OpenCore. If you are having issues then it’s recommended to use the debug version for troubleshooting.

This build is utilizing a multiple drive triple boot of Mac, Windows and Linux. This means that each operating system is installed on its own drive and separated from one another. I will not be providing a guide for single drive multi booting.

Don’t forget to edit the EFI/OC/config.plist file, you should generate your own SMBIOS info by following the [Comet Lake Config Guide #PlatformInfo.](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#platforminfo)

Highly recommended reading the whole OpenCore Install Guide before starting.

- image of opencore bootloader

![Screen Shot 2022-11-30 at 9 25 22 PM](https://user-images.githubusercontent.com/14919064/204953256-bb909cd8-accd-4b82-9e81-7ca227769dd8.png)

![Screen Shot 2022-11-30 at 9 24 14 PM](https://user-images.githubusercontent.com/14919064/204953542-616c04e1-ccee-4be2-9f11-23cfc8cb5141.png)

- image of geekbench scores

--------------------------------------------------------------------------------------------------------------

# Table of Contents
- [Software](#software)
- [Hardware](#hardware)
- [Bios](#bios)
- [Functionality](#functionality)
- [Installation](#installation)
    - [Ubuntu](#ubuntu)
    - [Windows](#windows)
    - [Mac](#mac)
- [Post Installation](#post_installation)
- [Troubleshooting](#troubleshooting)
- [Changelog](#changelog)

--------------------------------------------------------------------------------------------------------------
    
# Software

-   **Bootloader:** OpenCore 0.8.6-Release
-   **Operating System:** macOS Monterey 12.6.1 (iMac20,2)
-   **Operating System:** Windows 10 Pro
-   **Operating System:** Ubuntu 22.10

--------------------------------------------------------------------------------------------------------------

# Hardware
-   **Motherboard:** Asus ROG Strix Z590-E Gaming
-   **CPU:** Intel Core i9-10900K
-   **RAM:** TEAMGROUP T-Force Xtreem ARGB + G.Skill Trident Z RGB [3600MHz CL14 (4 x 16GB) CL14-15-15-35]
-   **GPU:** AMD Radeon Pro WX 4100 + Nvidia RTX 3080 Founders Edition
-   **Wi-Fi/BT:** Broadcom BCM94360NG 802.11ac 1200Mbps Bluetooth 4.0
-   **Ethernet:** Intel 2.5 Gb Ethernet I225-V
-   **Storage NVME:** 1x WD Black SN850 1TB + 1x WD Blue SN570 1TB + 1x Toshiba XG3 1TB
-   **Storage SATA:** 2x WD Blue WDS100T2B0A 1TB + 1x WD Blue WDS400T2B0A 4TB
-   **Case:** RAIJINTEK Thetis Window ATX Mid Tower Case
-   **PSU:** Lian Li SP750 850 W 80+ Gold SFX
-   **CPU Cooler:** ID-COOLING FROSTFLOW X 240 Snow
-   **Fans:** ARCTIC P12 PWM PST A-RGB 3-pack

--------------------------------------------------------------------------------------------------------------

# BIOS
- **Bios Version:** 1402

## Advanced
- CPU Configuration
    - Intel (VMX) Virtualization Technology: Enabled _(if DisableIoMapper to YES)_
    - Hyper-Threading: Enabled
- System Agent (SA)-Configuration
    - VT-d: Enabled _(if DisableIoMapper to YES)_
    - Memory Configuration
       - Memory Remap Feature: Enabled _(if DisableIoMapper to YES)_
    - Graphics Configuration
       - iGPU Multi-Monitor: Enabled _(disable if you don't have igpu)_
       - DVMT Pre-Allocated: 64 MB _(skip if you don't have igpu)_
           - _I personally use 128 MB as I use a 3440x1440 monitor. Test as needed_
- APM Configuration
    - ErP Ready to: Enable (S4+S5) _(enable full sleep without leds)_
- PCH Storage Configuration
    - SATA Mode Selection: AHCI
- PCH-FW Configuration
    - PTT: Disabled _(this is Intel Platform Trust / TPM)_
- Thunderbolt(TM) Configuration
    - Discrete Thunderbolt(TM) Support: Disabled
- PCI Subsystem Settings
    - Above 4G Decoding: Enabled
    - Re-Size BAR Support: Disabled
- USB Configuration
    - Legacy USB Support: Enabled
    - XHCI Hand-off: Enabled

## Boot
- CSM _(Compatibility Support Module)_
    - Launch CSM: Disabled _(Must be off in most cases, GPU errors/stalls like gIO are common when enabled)_
- Secure Boot
    - OS Type: Windows UEFI mode
    - Key Management
        - Clear Secure Boot Keys: Execute
- Boot Configuration
    - Fast Boot: Disabled

## Missing From Bios
- VT-x
- Execute Disable Bit
- Software Guard Extensions _(SGX)_
- CFG-Lock _(Asus Z590 are factory unlocked. AppleCpuPmCfgLock and AppleXcpmCfgLock quirks are not needed)_

--------------------------------------------------------------------------------------------------------------

# Functionality

## What's Working:
 &#x2611; Intel UHD630 headless mode (iGPU)<br/>
 &#x2611; AMD Radeon Pro WX 4100 (dGPU)<br/>
 &#x2611; Audio Realtek ALCS1220A<br/>
 &#x2611; Intel I225-V 2.5Gb Ethernet<br/>
 &#x2611; Wi-Fi and Bluetooth (via BCM94360NG by replacing onboard motherboard Wi-Fi card)<br/>
 &#x2611; USB<br/>
 &#x2611; Restart/Shutdown<br/>
 &#x2611; Sleep/Wake<br/>
 &#x2611; Power Management (Native support)<br/>
 &#x2611; Handoff<br/>
 &#x2611; Continuity<br/>
 
## To Test:
 &#x2610; Amazon Prime Video and Netflix in Safari<br/>
 &#x2610; AppleTV<br/>
 &#x2610; SideCar<br/>
 &#x2610; Facetime<br/>
 &#x2610; iMessage<br/>
 
## Not Working:
 &#x2612; Thunderbolt<br/>
 &#x2612; iGPU display out (Z590 chipset is not compatilbe with hardware acceleration)<br/>

--------------------------------------------------------------------------------------------------------------

# Installation

If you intend to dual boot or triple boot your machine, then I highly recommend you install them in the following order, Ubuntu, Windows, Then Mac OS. This is because **grub and the windows boot loader has a bad habit of installing the efi installation files on whatever efi partition is available in the system. As such, just do it them without any drives in the system.** This will save you a bunch of time.

## Ubuntu

1. Remove all drives that have any operating systems from your machine.
2. This is the easiest process. Just follow the provided guide from the official website.
    - https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview

## Windows

1. Remove all drives that have any operating systems from your machine.
2. This is another easy one. Just follow the provided guide from the official website.
    - https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/install-windows-from-a-usb-flash-drive?view=windows-11

## Mac

1. 

--------------------------------------------------------------------------------------------------------------

# Post_Installation

--------------------------------------------------------------------------------------------------------------

# Troubleshooting
- **Random Freezes/Reboots:**
    - **Resolution:** Remove all samsung drives from the build. Samdung SSD controllers are not compatible with Mac OS seemingly what so ever. If you have any Samsung drives in your system, I recommend you remove them for this build. Having them installed even just as storage drives causes random freezes and crashes.
        - drives tested with: Samsung PM981, Samsung 870 QVO
        <br>
- **DVMT Pre-Allocated missing in bios**
    - **Resolution:** Power down your computer and move your HDMI or Display Cable to the motherboard ports and power up. Wait for the display to be connected and enter the bios. Now the option should be there.
    <br>
- **Sleep/Wake Issues**
    - **Resolution 1:** change your bios settings
        - Bios/Advance/APM Configuration/ErP Ready to: Enable (S4+S5)
            - this prevents USB from immediately waking up and this allows it to actually sleep
    - **Resolution 2:** Recreate your SSDTs for your specific build. I used the Dortania guides for this.
        - https://dortania.github.io/Getting-Started-With-ACPI/ssdt-platform.html#desktop
        <br>
- **Black Screen when booting:**
    - **Resolution:** add agdpmod=pikera to bootargs
    <br>
- **If you are using a DGPU in the first pcie slot:**
    - **Resolution:** delete block PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0) in config.plist

--------------------------------------------------------------------------------------------------------------

# Changelog

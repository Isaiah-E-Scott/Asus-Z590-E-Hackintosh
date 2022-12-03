# Asus-Z590-E-Hackintosh Triple Boot Guide

This repository is about hackintosh on Asus ROG STRIX Z590-E. All the hardware is working as expected, and it's ready for daily usage.

Anyone who has the same board can use my EFI directly or with minimal changes. The source EFI folder uses the release version of OpenCore. If you are having issues, then it’s recommended to use the debug version for troubleshooting.

This build is utilizing a multiple drive triple boot of Mac, Windows, and Linux. This means that each operating system is installed on its own drive and separated from one another. I will not be providing a guide for single drive multi booting.

Don’t forget to edit the EFI/OC/config.plist file, you should generate your own SMBIOS info by following the [Comet Lake Config Guide #PlatformInfo.](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#platforminfo)

Highly recommended reading the whole OpenCore Install Guide before starting.

- image of opencore bootloader

![Screen Shot 2022-11-30 at 9 25 22 PM](https://user-images.githubusercontent.com/14919064/204953256-bb909cd8-accd-4b82-9e81-7ca227769dd8.png)

![Screen Shot 2022-11-30 at 9 24 14 PM](https://user-images.githubusercontent.com/14919064/204953542-616c04e1-ccee-4be2-9f11-23cfc8cb5141.png)

![Screen Shot 2022-12-01 at 7 01 27 AM](https://user-images.githubusercontent.com/14919064/205048290-a35dd6a3-3cf0-4208-b533-e488d56bd118.png)

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
   - [IGPU](#igpu)
   - [NO_IGPU](#no_igpu)
   - [Wi-Fi/Bluetooth](#wi-fi)
   - [USB](#usb)
   - [Sleep](#sleep)
- [Troubleshooting](#troubleshooting)
- [Changelog](#changelog)
- [Sources](#sources)

--------------------------------------------------------------------------------------------------------------
    
# Software

-   **Bootloader:** OpenCore 0.8.6-Release
-   **Operating System:** macOS Monterey 12.6.1 (iMac20,2)
-   **Operating System:** Windows 10 Pro
-   **Operating System:** Ubuntu 22.10

--------------------------------------------------------------------------------------------------------------

# Hardware

| Component         | Details                                                                                         |
| :-----------      | :-------------                                                                                  |
| **Motherboard:**  | Asus ROG Strix Z590-E Gaming                                                                    |
| **RAM:**          | <ul><li>TEAMGROUP T-Force Xtreem ARGB 3600MHz CL14 (2 x 16GB)</li><li>G.Skill Trident Z RGB 3600MHz CL14 (2 x 16GB)</li></ul> |
| **CPU:**          | Intel Core i9-10900K                                                                            |
| **iGPU:**         | Intel UHD 630 (Headless)                                                                        |
| **GPU:**          | <ul><li>AMD Radeon Pro WX 4100 4GB</li><li>Nvidia RTX 3080 Founders Edition 10GB</li></ul>      |
| **Wi-Fi/BT:**     | Broadcom BCM94360NG 802.11ac 1200Mbps Bluetooth 4.0                                             |
| **Audio:**        | Realtek ALC4080 + Savitech SV3H712 AMP (layout-id:7)                                            |
| **Ethernet:**     | Intel 2.5 Gb Ethernet I225-V                                                                    |
| **Storage NVME:** | <ul><li>1x WD Black SN850 1TB</li><li>1x Toshiba XG6 1TB</li><li>1x Toshiba XG3 1TB</li></ul>   |
| **Storage SATA:** | <ul><li>2x WD Blue WDS100T2B0A 1TB</li><li>1x WD Blue WDS400T2B0A 4TB</li></ul>                 |
| **Case:**         | RAIJINTEK Thetis Window ATX Mid Tower Case                                                      |
| **PSU:**          | Lian Li SP750 850 W 80+ Gold SFX                                                                |
| **CPU Cooler:**   | ID-COOLING FROSTFLOW X 240 Snow                                                                 |
| **Fans:**         | ARCTIC P12 PWM PST A-RGB 3-pack                                                                 |

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

1. boot into Windows
2. follow the Dortania guide to create the installation usb using windows
    -  https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos
    -  I highly recommend using the 
3. copy my provided EFI folder into your usb
4. restart your pc and boot into the usb
5. now install mac normally

--------------------------------------------------------------------------------------------------------------

# Post_Installation

## IGPU

This is enabled in the EFI by default

Intel UHD630 Headless mode working for hardware acceleration

Working by: **ig-platform-id = 0300C89B**

DeviceProperties:
```
<key>PciRoot(0x0)/Pci(0x2,0x0)</key>
	<dict>
		<key>AAPL,ig-platform-id</key>
		<data>AwDImw==</data>
	</dict>
```

## No_IGPU

This is for f skew processors without IGPU

Remove the iGPU code block from above

also remove **igfxonln=1** from the boot-args

it would end up looking like this:
```
<key>boot-args</key>
<string>debug=0x100 keepsyms=1</string>
```

## Wi-Fi

The only way to get fully functioning bluetooth and wi-fi is to replace the wi-fi card. I ended up replacing the onboard wi-fi card in the motherboard itself with BCM94360NG. Therefore, we keep all of our pci-e expansion slots.

In order to do this, you must remove the vrm heatsink, pcie cover, and the first m.2 heatsink to access the wi-fi card.

1. remove the first m.2 heatsink

![step1](https://user-images.githubusercontent.com/14919064/205451947-fa4894ba-0f6e-472d-a1a1-c6003230a998.png)

2. remove the screws from the underside of the motherboard

![step2](https://user-images.githubusercontent.com/14919064/205452571-f266e30c-5411-4876-84c6-a6772f979134.png)

3. remove the heatsinks from the motherboard

![step3](https://user-images.githubusercontent.com/14919064/205452754-908d802e-e88e-4985-a2b5-ff15f0be5827.png)

4. remove the integrated io shield

![step4](https://user-images.githubusercontent.com/14919064/205452906-58f20336-419c-4044-994a-ab8d09b66c8a.png)

5. remove the silver enloosure holding the wi-fi card

![step5](https://user-images.githubusercontent.com/14919064/205453072-2835a425-3e4d-41c4-a7ca-cd3f4f27b8d4.png)

6. unscrew and unplug the old wi-fi card
7. replace the old wifi card and plug the cables back in for the antennas
    - there will be some tape holding the original card down. it should peel away after a little force is applied
    
![rE00QKoJC0LEKEcNljoaXoM4FhIhuiJVeaFQnvfJn4A](https://user-images.githubusercontent.com/14919064/205152263-55391634-418b-402b-ad88-b90dcf818888.jpg)

8. put it all back together in reverse order

![step6](https://user-images.githubusercontent.com/14919064/205453204-7c371580-ebda-4535-93eb-40c7e0ba1852.png)

## USB

I did my USB mapping via USBToolBox. If you want full install details then I recommend use that page directly.
- https://github.com/USBToolBox/kext

![USB Map Rear IO](https://user-images.githubusercontent.com/14919064/205454702-da89fbe1-85db-4799-800d-a8af62ba0642.png)

![Untitled-1 (2)](https://user-images.githubusercontent.com/14919064/205457596-14f415d6-3fd4-4274-8aeb-c75e0657a9ee.png)

![USB Map Configuration](https://user-images.githubusercontent.com/14919064/205456948-d7f42615-7027-4e83-b651-71ba14e47fd2.PNG)

| Type                             | Port        | USBToolBox                            |
| :------------------------------- | :---------- | :----------------------------------   |
| Internal Audio                   | HS14        | Port 14                               |
| USB 2.0                          | HS12        | Port 12                               |
| USB 2.0                          | HS11        | Port 11                               |
| USB 3.2 Gen 1                    | HS10 / SS10 | Port 10 (USB 2.0) / Port 24 (USB 3.0) |
| USB 3.0                          | HS09 / SS09 | Port 9 (USB 2.0) / Port 23 (USB 3.0)  |
| USB 3.0                          | HS08 / SS08 | Port 8 (USB 2.0) / Port 22 (USB 3.0)  |
| USB 3.2 Gen 2 type c w/ switch   | HS07 / SS07 | Port 7 (USB 2.0) / Port 21 (USB 3.0)  |
| USB 3.2 Gen 2                    | HS06 / SS06 | Port 6 (USB 2.0) / Port 20 (USB 3.0)  |
| USB 3.2 Gen 2                    | HS05 / SS05 | Port 5 (USB 2.0) / Port 19 (USB 3.0)  |
| USB 3.2 Gen 2x2 type c w/ switch | HS03 / SS03 | Port 3 (USB 2.0) / Port 18 (USB 3.0)  |
| Internal Wi-Fi / Bluetooth       | HS02        | Port 2                                |

Note:

- If you do not install a compatible wi-fi card, then I recommend you disable the internal wi-fi card.

## Sleep

Wake works with DP output and power button. GPRW Patch is used to disabling the USB device instant wake.

Note:

- Without enabling GPRW, a keyboard press or mouse click can wake up the display as well, but a second press or click is needed when the light is on, I tried to fix it by following Keyboard Wake Issues Guide, but didn't work. So my choice is to just use the power button, disable SSDT-GPRW if you want to use a keyboard or mouse to wake up.

--------------------------------------------------------------------------------------------------------------

# Troubleshooting
- **Random Freezes/Reboots:**
    - **Resolution:** Remove all samsung drives from the build. Samdung SSD controllers are not compatible with Mac OS seemingly whatsoever. If you have any Samsung drives in your system, I recommend you remove them for this build. Having them installed even just as storage drives causes random freezes and crashes.
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

V1: initial release on Opencore 8.6 for Monterey 12.6.1

--------------------------------------------------------------------------------------------------------------

# Sources

- high quality pictures provided by kitguru tech, nexus, gadget pilipinas, and direct dial
    - https://www.kitguru.net/components/motherboard/luke-hill/asus-rog-z590-motherboard-round-up/3/
    - https://hexus.net/tech/reviews/mainboard/147673-asus-rog-strix-z590-e-gaming-wifi/
    - https://www.gadgetpilipinas.net/2021/04/asus-rog-strix-z590-e-gaming-motherboard-review/
    - https://www.directdial.com/us/item/asus-rog-strix-z590-e-90mb1640-m0aay0-gaming-wifi-6e-lga-1200-intel-11th-10th-gen-atx-gaming-desktop-motherboard/rogstrixz590egamingwifi

# dell-latitude-7275-hackintosh
Files for installing MacOS on Dell Latitude 7275. This probably works for Dell XPS 12 9250 since they have very similar internals.

### What Doesn't Work

* Trackpad (attempting to get full macOS Trackpad gesture support working)
* Many of the function buttons (paper display, refresh, airplane mode, lock, switch display, disable camera only sometimes works)
* Internet service during sleep (Find My Mac, Allow Apple Watch to unlock your Mac, iMessage/Facetime wake from sleep)
* ThunderBolt*
* Camera
* Possibly other things

### What Works

* Touchscreen (with gestures as well curtesy of VoodooI2C)
* Audio
* NVMe
* Battery Status
* WiFi with Intel (Can swap for Broadcom if desired, but need appropriate kexts)
* Apple iServices (iMessage, Facetime, iCloud--as long as you update your SMBIOS as described below)
* Sleep (keyboard backlight turns off)**

*I have not attempted to get Thunderbolt working. I believe we may need an updated patch. Although, we can probably reuse SSDTs found in the Catalina guide.

**I believe sleep is working, but need to confirm this.

Please note, features I have not included do not have SSDT or other easy ACPI patches. I am strongly against DSDT patches, as they break with BIOS changes, are extremely fragile, and can break sleep, Apple iServices etc. No part of this guide requires DSDT patching.

### Installation

1. Download the Big Sur 12.0.1 installation file from Apple's App Store and write it to a USB with the createinstallmediainstall method.
  1. Please make sure the USB is formatted as GPT.
1. I mounted the EFI partition on the USB with an, old, but highly useful tool called EFI MountainShow (or you could just use diskutil via command line as well)
1. Copy the EFI partition I provided to the EFI partition you mounted
1. Update the SMBIOS in the EFI > OC > config.plist with new BSN, Serial, and SmUUID
  1. I use Clover Configuator to generate these instead of the recommended GenSMBIOS script as it's quicker
  1. I recommend using PlistEdit Pro to edit the plist. Please note, SMBIOS information can be found at Root > PlatformInfo > Generic
1. Boot USB and follow installation
1. Once booted into macOS open terminal and run

```
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
```

1. Finally, mount the EFI partition on the USB and on your system disk. Copy the EFI (or at least just BOOT and OC) to your EFI partition on your system disk.

1. The following command fixes the RTC/HID wake issues while in sleep. Please note, that this comes at the cost of services like Find My Mac, Allow Apple Watch to unlock your Mac, iMessage/Facetime wake during sleep, which probably doesn't matter as sleep doesn't really work with TCPKEEPALIVE enabled anyway.

```
sudo pmset -a TCPKEEPALIVE 0
```

After this, everything should be set. Please note, there ARE things to fix, but this should serve as a good starting spot for most folks, who don't mind the shortcomings currently described in the "What Doesn't Work" section. Enjoy!

### Resources

I would like to thank the following people whom helped me directly or by reusing content of their's I found (which I hope is alright):

* [dortania's ​OpenCore guide](https://dortania.github.io/)

And so many more

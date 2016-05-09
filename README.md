# HW Spec
* Intel i5 2500k (Sandy Bridge)
* Asus P8Z68-v GEN3
* Asus GTX 560
* Corsair Vegeance LD 16GB (4x 4Gb)
* Crucial CT120 M500 - 120Gb

# BIOS Settings
* Default settings
* Intel virtualization - Enabled
* Primary GPU - Integrated (I am using three monitors so one - primary - is in the internal GPU and rest is in GTX560)

# Installation
* Create USB Stick with this guide - http://www.tonymacx86.com/el-capitan-desktop-guides/172672-unibeast-install-os-x-el-capitan-any-supported-intel-based-pc.html
* Use preset for Intel GPU injection
* Put the custom UEFI driver inside the EFI partition (mount via http://www.osx86.net/file/49-clover-configurator/)
* Use MultiBeast
* > Quick Start > UEFI
* > Drivers > Network > AppleIntelE1000e
* > Drivers > Audio > ALC892
* > Customize > Intel HD 3000
* > Customize > Sandy Bridge Core i5 (will be probably replaced with SSDT)
* > Customize > System Def > MacMini5,1
* Put nullpowermanagement.kext to the kext/10.11 folder on usb install stick

# After install
* Put FakeSMC and AppleIntelE1000e.kext to EFI partition to the kext/10.11/
* Instal smbios
*

# Bootloader
* Clover 3320
* Aditional installed drivers can be found here https://github.com/wilima/Hackintosh/tree/master/CLOVER/drivers32UEFI and https://github.com/wilima/Hackintosh/tree/master/CLOVER/drivers64UEFI

# config.plist
https://github.com/wilima/Hackintosh/blob/master/CLOVER/config.plist
* SMBIOS - MacMini5,1

# Audio
I have used this guide for my audio - https://www.reddit.com/r/hackintosh/comments/397wl1/guide_realtek_alc_8xx_1150_with_native_applehda/
Or use MultiBeast ALC892 audio driver

# Internal GPU and GTX560 working together / AirPlay
I have used this guide - http://www.tonymacx86.com/graphics/128226-integrated-discrete-graphics-working-together.html#post784770

# Native CPU/iGPU Power managment
I have used this guide - http://www.tonymacx86.com/mavericks-desktop-support/128926-mavericks-native-cpu-igpu-power-management.html
Install patched DSDT and SSDT

Additional tweaks:

**DSDT Patch**
```
#Maintained by: RehabMan for: Laptop Patches
#system_SMBUS.txt


#   SMBUS fix
into device label BUS0 parent_adr 0x001F0003 remove_entry;
into device name_adr 0x001F0003 insert
begin
Device (BUS0)\n
{\n
    Name (_CID, "smbus")\n
    Name (_ADR, Zero)\n
    Device (DVL0)\n
    {\n
        Name (_ADR, 0x57)\n
        Name (_CID, "diagsvault")\n
        Method (_DSM, 4, NotSerialized)\n
        {\n
            If (LEqual (Arg2, Zero)) { Return (Buffer() { 0x03 } ) }\n
            Return (Package() { "address", 0x57 })\n
        }\n
    }\n
}\n
end;
```

**config.plist**
* config.plist/ACPI/SSDT/DropOem/NO (Clover)

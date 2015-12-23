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

# Bootloader
* Clover 3320
* Aditional installed drivers can be found here https://github.com/wilima/Hackintosh/tree/master/CLOVER/drivers32UEFI and https://github.com/wilima/Hackintosh/tree/master/CLOVER/drivers64UEFI

# config.plist
https://github.com/wilima/Hackintosh/blob/master/CLOVER/config.plist
* SMBIOS - MacMini5,1

# Audio
I have used this guide for my audio - https://www.reddit.com/r/hackintosh/comments/397wl1/guide_realtek_alc_8xx_1150_with_native_applehda/

# Internal GPU and GTX560 working together / AirPlay
I have used this guide - http://www.tonymacx86.com/graphics/128226-integrated-discrete-graphics-working-together.html#post784770

# Native CPU/iGPU Power managment - NOT WORKING NOW
I have used this guide - http://www.tonymacx86.com/mavericks-desktop-support/128926-mavericks-native-cpu-igpu-power-management.html

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


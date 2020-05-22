# HP Elite X2 G1 Hackintosh

Hardware Specs:

```
CPU:     Intel M7 6Y75
RAM:     8GB 1866MHz LPDDR3
GPU:     HD 515
AUDIO:   CX20724
Touch:   Wacom HID Based Display(WCOM4814)
```

## ACPI


For those curious, I've also provided an ACPI dump of my laptop(BIOS ver. 1.48 Rev.A)
* [Firmware Dumps](/ACPI/ACPI-Dumps/)
* [Custom SSDTs](/ACPI/Custom-SSDTs)


**Required SSDTs**:

| SSDT | ACPI Patches | Comments
| :--- | :--- | :--- |
| SSDT-BAT | A lot | Fixes Battery Readouts |
| SSDT-EC-USBX | N/A | Creates a fake EC and adds USB Power Properties |
| SSDT-HP-FixLidSleep | N/A | Fixes sleep with keyboard lid |
| SSDT-PLUG | N/A | Adds `plugin-type` to `\_PR.CPU0`, allows XCPM to load |
| SSDT-PNLF | N/A | Adds Backlight control support |
| SSDT-PTS | `_PTS` to `XPTS` | Fixes USB Shutdown calls |
| SSDT-SBUS-MCHC | N/A | Allows AppleSMBus to load |
| SSDT-SLPB | N/A | Fixes Sleep button support(maybe? Doesn't work for me) |
| SSDT-TBHP | N/A | Fixes USB-C Hotplug |
| SSDT-TPL0 | `PS0`/`PS3` to `XPS0`/`XPS3` | Attempts to fix I2C touchscreen |


**Removing XOSI Renames**:

Thanks to DhiankG, XOSI renames are no longer needed. Instead only needing the following patch:

```
Comment:
	Enable Touchscreen in macOS
Find:
	95 4F 53 59 53 0B DC 07
Replace:
	95 4F 53 59 53 0B 00 00
```

[Source](https://ptb.discordapp.com/channels/186648463541272576/573338555305295903/713434444861800589)

## Kexts

Hardware specific kexts:

* VoodooI2C
* VoodooI2CHID
* AlpsT4USB

## Configuration Specifics

**DeviceProperties**:

* `PciRoot(0x0)/Pci(0x1F,0x3)`
  * `alc-layout-id | Data | 03000000`
* `PciRoot(0x0)/Pci(0x2,0x0)`
  * `APPL,ig-platform-id | Data | 00001E19`
  
  
**Kernel**:

* Quirks:
  * `AppleCpuPmCfgLock` set to True
  * `AppleCpuPmCfgLock` set to True

**Misc**:

* `BlessOverride`:
  * `\EFI\Microsoft\Boot\bootmgrfw.efi`
  
**PlatformInfo**:

* Generic -> SystemProductName:
  * MacBook9,1
  
## Miscellaneous fixes

Mainly quality of life improvements.

**HiDPI**:

* Download [RDM]() and set the following custom resolution:

| Horizontal | Vertical | HiDPI |
| :--- | :--- | :--- |
| 1280 | 854 | True |
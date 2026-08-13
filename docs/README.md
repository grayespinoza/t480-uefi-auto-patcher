# T480 UEFI Auto-Patcher

## Requirements

1. Lenovo T480
2. Linux PC
3. CH341A USB programmer
4. SOIC-8 clip
5. [Auto-Patcher](https://www.badcaps.net/forum/troubleshooting-hardware-devices-and-electronics-theory/troubleshooting-laptops-tablets-and-mobile-devices/bios-requests-only/78215-lenovo-bios-auto-patcher-for-supervisor-password-removal)

## Disassembly

1. Power down T480
2. Disconnect external charger
3. Disconnect external battery
4. Remove bottom cover
5. Disconnect internal battery
6. Disconnect CMOS battery
7. Disconnect all storage devices

## Flashprog

1. Locate BIOS chip
2. Verify [Flashprog chip support](https://flashprog.org/wiki/Supported_Hardware)

```shell
sudo pacman -S flashprog
```

## Dumping BIOS

1. Connect CH341A to SOIC-8 clip making sure to align both pin 1's
2. Connect SOIC-8 clip to BIOS chip making sure to align red wire on clip to dot on chip
3. Plug in CH341A into Linux PC

```shell
flashprog -p ch341a_spi -r t480_bios_1.bin
flashprog -p ch341a_spi -r t480_bios_2.bin
flashprog -p ch341a_spi -r t480_bios_3.bin
sha256sum t480_bios_*.bin
```

Verify all three checksums match.

## Patching BIOS

1. Add execute permission to `UEFIReplace`

```shell
cp t480_bios_1.bin t480_bios.bin
```

3. Execute `autopatch` on `t480_bios.bin`

## Flashing BIOS

```shell
flashprog -p ch341a_spi -w t480_bios_PATCHED.bin
```

1. Unplug CH341A from Linux PC
2. Remove the SOIC-8 clip from the BIOS chip
3. Reinstall T480 batteries
4. Power up T480
5. Enter any character when prompted for supervisor password
6. Press enter when it shows hardware ID
7. Press space bar twice when prompted
8. Power down T480
9. Uninstall T480 batteries
10. Connect SOIC-8 clip to BIOS chip
11. Plug in CH341A into Linux PC

```shell
flashprog -p ch341a_spi -w t480_bios.bin
```

Your original BIOS has now been flashed back without the supervisor password. You can now disconnect the programmer and clip, and reassemble your T480.

# T480 UEFI Auto-Patcher

## Requirements

1. Lenovo T480
2. Linux PC
3. CH341A USB programmer
4. SOIC8 clip
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

```shell
flashprog -p ch341a_spi -r t480_bios_1.bin
flashprog -p ch341a_spi -r t480_bios_2.bin
flashprog -p ch341a_spi -r t480_bios_3.bin
sha256sum t480_bios_*.bin
```

## Patching BIOS

1. Add execute permission to `UEFIReplace`

```shell
cp t480_bios_1.bin t480_bios.bin
bash ./autopatch t480_bios.bin
flashprog -p ch341a_spi -w t480_bios_PATCHED.bin
```

## Flashing BIOS

1. Unplug CH341A from Linux PC
2. Remove the SOIC8 clip from the BIOS chip
3. Follow instructions from badcaps.net

```shell
flashprog -p ch341a_spi -w t480_bios.bin
```

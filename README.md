# amlogic-card-maker-macos
Adaptation of https://www.cnx-software.com/2016/11/19/how-to-create-a-bootable-recovery-sd-card-for-amlogic-tv-boxes/

## Pre-requisites
You'll need to format an SD card with a single FAT32 partition, leaving enough free, unallocated space before the partition.

## Instructions
1. Compile the tool with gcc:

```
gcc src/aml-upgrade-package-extract.c -o aml-upgrade-package-extract
```

2. Execute it referencing the image you'd like to burn:

```
./aml-upgrade-package-extract <path_to_image>
```

3. From the extracted files, flash aml_sdc_burn.UBOOT to the SD card (replace <sd_device> with the corresponding SD device; in my case `disk2`):

```
diskutil unmountDisk disk2
sudo dd if=aml_sdc_burn.UBOOT of=/dev/disk2 bs=1 count=442
sudo dd if=aml_sdc_burn.UBOOT of=/dev/disk2 seek=1 skip=1 bs=512
sync
```

If the second `dd` command fails, unmount the disk again and retry it, as it may have reattached automatically.

4. Re-mount the SD card, and copy the firmware file along with the `aml_sdc_burn.*` files:

```
cp aml_sdc_burn.ini aml_sdc_burn.UBOOT <partition_mount_point>
cp <path_to_image> <partition_mount_point>/aml_upgrade_package.img
diskutil unmountDisk disk2
```

5. The card is now ready to be inserted in the TV box.

# Partitions

## Disc layout
The system I run the Arch Home Server setup on
holds three discs. Two internal and one external
harddisc.

| Disc | Type | Storage |
| ---- | ---- | ------- |
| sda | spinning rust | 1 TB |
| sdb | NVME | 256 GB |
| sdc | external HDD | 1 TB |

Unfortunately, the system's bios recognizes the
NVME drive as sdb. This must be mentioned by
following this guide, as this is something
unusual.

With this three devices installed, the following
partitions are considered for the home server 
setup.

| Partition | Mount point | Partition type | Size |
| --------- | ------------ | -------------- | --- |
| '/dev/sda1' | subvolume(s) | btrfs | 931.5 GB |
| '/dev/sdb1' | '/boot' | EFI system partition | 1.0 GB |
| '/dev/sdb2' | subvolume(s) | btrfs | 221.5 GB |
| '/dev/sdc1' | '/mnt/backup | btrfs | 931.5 GB |

## Create the partitions
To create the new partition layout on the harddiscs
we will use ’fdisk‘ which requires root privileges.

> [!CAUTION]
> Creating new partitions on a harddrive will cause the loss of the data stored on this device. Please make sure you have backups of your data!

’’’sh
fdisk /dev/sda
’’’
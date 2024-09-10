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
| `/dev/sda1` | subvolume(s) | btrfs | 931.5 GB |
| `/dev/sdb1` | `/boot` | EFI system partition | 1.0 GB |
| `/dev/sdb2` | subvolume(s) | btrfs | 221.5 GB |
| `/dev/sdc1` | `mnt/backup` | btrfs | 931.5 GB |

## Create the partitions
To create the new partition layout on the harddiscs
we will use `fdisk` which requires root privileges.

> [!CAUTION]
> Creating new partitions on a harddrive will cause the loss of the data stored on this device. Please make sure you have backups of your data!

```
fdisk /dev/sda
```
Create all partitions as described above and do the same
for `/dev/sdb` and `/dev/sdc`.

With the partitions in place, we can now format them to
the desired type.

```
mkfs.btrfs -L "root" /dev/sda1
mkfs.fat -F 32 -L "efi" /dev/sdb1
mkfs.btrfs -L "storage" /dev/sdb2
mkfs.btrfs -L "backup" /dev/sdc1
```

> [!TIP]
> You can also create the partition on `/dev/sdc` later, if the drive vontains already data you have backed-up.

Finally, it is time to mount the new generated partitions.

```
mount --mkdir -o rw,noatime,compress=zstd:3 /dev/sda1 /mnt/storage
mount --mkdir /dev/sdb1 /mnt/boot
mount --mkdir -o rw,noatime,compress=zstd:3 /dev/sdb2 /mnt
mount --mkdir -o rw,noatime,compress=zstd:3 /dev/sdc3 /mnt/backup
```

> [!TIP]
> Mount the device using `mount --mkdir` will automatically create the directory.

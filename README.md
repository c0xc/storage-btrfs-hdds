storage-hdd-table
=================

HDD overview script for BTRFS and ZFS storage setups.

Checks which drives are attached how (path) and what partitions
and filesystems they have. If a drive is part of a BTRFS volume,
the label is shown. If it's part of a ZFS pool, that's indicated as well.
If no drive is provided as argument, all drives are listed.



Rationale
---------

This script was initially written for BTRFS storage pools with many drives,
to see which drives belong to the pool and which ones are empty that
can be used as replacements.
It can also be used in ZFS setups in the same way.

With 3 or 5 drives in the system, replacing a drive in a BTRFS volume is easy
because it's usually obvious which drive is the replacement and what's the
dev id of the old drive that has to be passed to the btrfs-replace command.

However, with 10 or more drives in the system, such a seemingly simple task
becomes very difficult. Finding the right drive in a list that doesn't even
fit on the screen is error-prone. Replacing the wrong drive or picking
the wrong replacement may have fatal consequences.

The point of this script is to provide the admin with a clear overview
that makes it obvious which drives are not in use yet
(those without a BTRFS label, see the example).

Note that the script assumes that full hdd block devices are used
as BTRFS devices, not partitions of a user-defined size.



Example
-------

The following example shows three BTRFS drives and makes it obvious that
"sdd" is the root disk. The fourth large disk "sde" is not in use yet and
may be used as a replacement.

    HDD        BTRFS  Serial            Path                                                    Capacity  Partitions
    /dev/sda   DATA   WD-AAAAAAAAAAAA   pci-0000:05:00.0-sas-exp0x5001e6746649bfff-phy0-lun-0   2.7T      1 (btrfs)
    /dev/sdb   DATA   WD-AAAAAAAAAAAA   pci-0000:05:00.0-sas-exp0x5001e6746649bfff-phy1-lun-0   2.7T      1 (btrfs)
    /dev/sdc   DATA   WD-AAAAAAAAAAAA   pci-0000:05:00.0-sas-exp0x5001e6746649bfff-phy2-lun-0   3.7T      1 (btrfs)
    /dev/sdd          WD-AAAAAAAAAAAA   pci-0000:05:00.0-sas-exp0x5001e677b66fefff-phy11-lun-0  232.9G    2 (ext4, linux-swap(v1))
    /dev/sde          WD-AAAAAAAAAAAA   pci-0000:05:00.0-sas-exp0x5001e6746649bfff-phy2-lun-0   3.7T      0


For a full example, see the example file.

To replace a drive in a ZFS pool, you'll want to have its unique name.
Use the name option for that. Grep for the serial number to see the newly attached drive:

    # adm-hdd-table --name | grep R54
    /dev/sda  ata-WDC_WD30EFRX-68EUZN0_WD-XXXXXXXXXR54   zfs-4f018fba35a0e5dd  2.7T



Author
------

Philip Seeger (philip@philip-seeger.de)


---
tags:
  - lvm
  - linux
---
# Linux Logical Volumes and LVM

Logical Volume Management (LVM) is a storage management system for Linux that provides more flexibility than traditional partitioning schemes. Let me explain both logical volumes and the LVM system in detail.

## Logical Volume Manager (LVM) Overview

LVM is a device mapper framework that provides logical volume management for the Linux kernel. It allows for:

- Creating logical storage volumes across multiple physical devices
- Dynamic resizing of storage volumes without downtime
- Taking snapshots of volumes for backup purposes
- Migrating data between physical devices while online

## LVM Architecture

LVM follows a layered architecture with three main components:

1. **Physical Volumes (PVs)**: These are the actual storage devices or partitions (like /dev/sda1, /dev/sdb, etc.) that provide the storage capacity.
2. **Volume Groups (VGs)**: A collection of one or more physical volumes that serve as a pool of storage space.
3. **Logical Volumes (LVs)**: Virtual partitions created from the volume groups that function like regular partitions.

## Physical Volumes (PVs)

A physical volume is the foundation of the LVM system:

- Any block device can be initialized as a physical volume
- Each PV contains a label at the beginning (metadata area)
- The space on a PV is divided into physical extents (PEs), which are fixed-size units (default is 4MB)
- PVs can span entire disks or disk partitions

## Volume Groups (VGs)

Volume groups act as storage pools:

- A VG combines the storage capacity of multiple physical volumes
- You can add or remove PVs from a VG dynamically
- All logical volumes must belong to a volume group
- A system can have multiple volume groups

## Logical Volumes (LVs)

Logical volumes are the equivalent of partitions in traditional storage systems:

- Created from the available space in a volume group
- Can span multiple physical volumes
- Can be formatted with any filesystem (ext4, xfs, etc.)
- Can be mounted like regular partitions
- Can be resized while in use (depending on the filesystem)
- Support various allocation policies (linear, striped, mirrored)

## LVM Commands

Here are the key commands for managing LVM:

### Physical Volume Management

```
pvcreate /dev/sdb         # Create a physical volume
pvdisplay                 # Display PV information
pvs                       # Shorter display of PVs
pvremove /dev/sdb         # Remove a PV
```

### Volume Group Management

```
vgcreate vg_name /dev/sdb # Create a volume group
vgextend vg_name /dev/sdc # Add a PV to a VG
vgreduce vg_name /dev/sdc # Remove a PV from a VG
vgdisplay                 # Display VG information
vgs                       # Shorter display of VGs
vgremove vg_name          # Remove a volume group
```

### Logical Volume Management

```
lvcreate -L 10G -n lv_name vg_name  # Create a 10GB logical volume
lvcreate -l 100%FREE -n lv_name vg_name  # Use all free space
lvextend -L +5G /dev/vg_name/lv_name  # Extend LV by 5GB
lvreduce -L -5G /dev/vg_name/lv_name  # Reduce LV by 5GB
lvdisplay                            # Display LV information
lvs                                  # Shorter display of LVs
lvremove /dev/vg_name/lv_name        # Remove a logical volume
```

## Advanced LVM Features

### Snapshots

LVM allows creating point-in-time snapshots of logical volumes:

```
lvcreate -L 1G -s -n snap_name /dev/vg_name/lv_name
```

Snapshots are useful for:

- Backups without downtime
- Testing changes before applying them permanently
- Creating consistent images for virtual machines

### Thin Provisioning

Thin provisioning allows you to allocate more space to logical volumes than physically available:

```
lvcreate --thinpool pool_name -L 10G vg_name
lvcreate -V 20G --thin -n lv_name vg_name/pool_name
```

### Striping

Striping distributes data across multiple physical volumes to improve performance:

```
lvcreate -L 10G -n lv_name -i 2 vg_name
```

### Mirroring

Mirroring duplicates data across multiple physical volumes for redundancy:

```
lvcreate -L 10G -m 1 -n lv_name vg_name
```

## Common LVM Workflow Example

Here's a typical workflow for setting up LVM:

1. Initialize physical volumes:
    
    ```
    pvcreate /dev/sdb /dev/sdc
    ```
    
2. Create a volume group:
    
    ```
    vgcreate data_vg /dev/sdb /dev/sdc
    ```
    
3. Create logical volumes:
    
    ```
    lvcreate -L 15G -n data_lv data_vg
    lvcreate -L 5G -n backup_lv data_vg
    ```
    
4. Format the logical volumes:
    
    ```
    mkfs.ext4 /dev/data_vg/data_lv
    mkfs.ext4 /dev/data_vg/backup_lv
    ```
    
5. Mount the logical volumes:
    
    ```
    mkdir -p /data /backup
    mount /dev/data_vg/data_lv /data
    mount /dev/data_vg/backup_lv /backup
    ```
    
6. To make mounts persistent, add to /etc/fstab:
    
    ```
    /dev/data_vg/data_lv /data ext4 defaults 0 0
    /dev/data_vg/backup_lv /backup ext4 defaults 0 0
    ```
    
## Persistently mount
![[Pasted image 20250411151659.png]]
## Benefits of LVM

1. **Flexibility**: Easily resize, add, or remove storage without downtime
2. **Consolidation**: Combine multiple physical disks into a single logical resource
3. **Snapshots**: Take point-in-time copies for backup or testing
4. **Online data migration**: Move data between storage devices while in use
5. **Performance options**: Implement striping for better throughput
6. **Redundancy options**: Implement mirroring for fault tolerance

LVM has become the standard for storage management in most Linux distributions, providing a powerful and flexible way to manage disk space beyond the limitations of traditional partitioning schemes.

----

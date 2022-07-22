# Storage Management - Proxmox

What are the best practices for storage management? What types of storage are available in proxmox? This note will have all the explanations.

## Local and Shared Storage

Local storage is a type of storage that is only available to one node, whereas Shared storage is accessible by multiple nodes or instances of proxmox. Here are the benefits of both options:

Local Storage:
- Speed, since data is not uploaded via the cloud.
- Works without an internet connection.
- Complete control over the data.

Shared Storage:
- Live migration of VMs from one node to another **without any downtime**.
- Easily accessible data throughout all the nodes in a cluster.

Local storage usually is enough for homelabs, but if you are dealing with hundreds of Terabytes and a couple of proxmox nodes, shared storage will be gradually more preferable.

## Filesystem types and which to choose

Proxmox offers a variety of storage types, some of them are better than other in some scenarios. Here are the pros and cons of some of the storage types:

### ZFS (local) - Zettabyte File System

Zettabyte File System was created by Sun Microsystems, which is now acquired by Oracle. It combines a file system with a volume manager, which provides a method of resizing and managing partitions better than traditional partitioning.

Pros:
- Compression on-the-fly, which enables us to store more data on a disk.
- Prevents "bit rot".
- Data recovery features.
- Creates incremental backups and snapshots.
- Data deduplication, meaning duplicate files are removed to save space.

Cons:
- Uses a lot of RAM.
- A lot of features are useless if using one or two hard disks.

ZFS is good for big data and multiple identically sized hard disks, other than that it is better to use other types of filesystems.

### Directory - ext4 / XFS

The directory option creates a simple ext4 or XFS filesystem that we can use for everything, such as: Backup files, VM and ISO images, Container Templates and more.

### LVM - Logical Volume Manager

A Logical Volume Management is a method of managing data on a disk, similarily to partitioning. Thanks to LVM, we are able to combine partitions and resize them without any data corruption.

### LVM-Thin

This option is the same as LVM, but with an additional feature of snapshots. LVM-Thin uses a method called *thin-provisioning*, which enables the user to allocate more data than physically possible. The difference between traditional-provisioning and thin-provisioning is that data is allocated only when it is **written** in thin-provisioning, whereas you have the amount of potential data on a logical volume **pre-allocated** in traditional-provisioning. Storage amount in every logical volume is *dynamic*, which changes upon writing additional data to a logical volume.

I think this method would be the preferred way to create storage for VM images, simply because compared to the "Directory" option, you have the snapshots feature with other image formats than *qcow2*.



## Seperating disk 

The Proxmox Management Console automatically detects changes in a layout of a disk. You can leverage that to create multiple different storage units that are on the same disk:

1. Go to the node that has the drive you want to partition.
2. Go to `Shell`.
3. Create a simple Linux partition using `fdisk /dev/sdX`.

Now you can use that partition to be used by proxmox, rather than using a whole disk.

## Resource
https://www.reddit.com/r/sysadmin/comments/n0otab/pros_and_cons_of_zfs/
https://en.wikipedia.org/wiki/Volume_manager
https://pve.proxmox.com/wiki/Storage:_Directory
https://www.youtube.com/watch?v=bpZKjeK0uTQ - Thin-provisioning definition
# resize-LVM-partition

Shutdown the VM:

```sudo virsh shutdown <vm_name>```

Resize the virtual disk:

```sudo qemu-img resize /var/lib/libvirt/images/<vm_name>.qcow2 +20G```

Start the VM

```sudo virsh start <vm_name>```

Rescan the disk to recognize the new space:

```echo 1 | sudo tee /sys/class/block/vda/device/rescan```
or 
```reboot```

Use parted to resize the partition:

```sudo parted /dev/vda```

Inside the parted prompt:

```(parted) print
(parted) resizepart 2 100%
(parted) quit```

Resize the Physical Volume
Resize the physical volume:

```sudo pvresize /dev/vda2```

Extend the logical volume:

```sudo lvextend -l +100%FREE /dev/centos/root```

Resize the filesystem:
If your filesystem is xfs:
```sudo xfs_growfs /```

If your filesystem is ext4:
```sudo resize2fs /dev/centos/root```

Summary of Commands

```
# Shutdown the VM
sudo virsh shutdown <vm_name>

# Resize the virtual disk
sudo qemu-img resize /var/lib/libvirt/images/<vm_name>.qcow2 +20G

# Start the VM
sudo virsh start <vm_name>

# Connect to the VM
sudo virsh console <vm_name>

# Rescan the disk
echo 1 | sudo tee /sys/class/block/vda/device/rescan

# Use parted to resize the partition
sudo parted /dev/vda
# Inside the parted prompt:
# print
# resizepart 2 100%
# quit

# Resize the physical volume
sudo pvresize /dev/vda2

# Extend the logical volume
sudo lvextend -l +100%FREE /dev/centos/root

# Resize the filesystem
# For xfs
sudo xfs_growfs /
# For ext4
sudo resize2fs /dev/centos/root
```

check the modified lvm partition size

``` df -h ```

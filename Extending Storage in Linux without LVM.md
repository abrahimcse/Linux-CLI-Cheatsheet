## Extending Storage in Linux without LVM: Step-by-Step Guide

Expanding storage without using LVM (Logical Volume Manager) is still possible, but the approach is more rigid and less flexible compared to LVM. 

**Method 1: Adding a New Disk and Mounting It to a Directory**
1. `Identify Disk Partitions:`
    ```
    sudo fdisk -l
   ```
Find the new disk's name (e.g., /dev/sdb).

2. `Partition the New Disk:`
    ```
    sudo fdisk /dev/sdb
   ```
    - Type `n` to create a new partition.
    - Press `Enter` to accept default values for partition type, start, and end sectors.
    - Type `w` to write the changes and exit.

3. `Format the New Partition:`
    ```
    sudo mkfs.ext4 /dev/sdb1
    ```
This formats the partition with the ext4 file system. Adjust the file system type if needed.

4. `Create a Mount Point:`
    ```
    sudo mkdir /mnt/newdisk
    ```
5. `Mount the New Partition:`
    ```
    sudo mount /dev/sdb1 /mnt/newdisk
    ```
6. `Verify the Mount:`
    ```
    df -h
    ```
7. `Automate the Mount at Boot:` To ensure that the new partition is automatically mounted when the system reboots, add an entry to `/etc/fstab`:
    ```
    vim etc/fstab
    /dev/sdb1   /mnt/newdisk   ext4   defaults   0   0
    ```
This line tells the system to mount `/dev/sdb1` at `/mnt/newdisk` with the `ext4` file system automatically at startup.

**Method 2: Using Symbolic Links**
You can also use symbolic links to link directories from the new disk to a directory on your existing file system. This can be useful if you have data directories that you want to point to the new disk without modifying your existing structure.

1. `Move Data to the New Disk:`
    ```
    sudo mv /path/to/large/data /mnt/newdisk/large_data
    ```
2. `Create a Symbolic Link:`
    ```
    sudo ln -s /mnt/newdisk/large_data /path/to/large/data
    ```
This way, the system and users can access /path/to/large/data as if it were still on the original location, but it is now physically stored on the new disk.

## Example Commands (Summary)
1. `Check existing partitions and disks:`
    ```
    sudo fdisk -l
    ```
2. `Create and format a new partition:`
    ```
    sudo fdisk /dev/sdb
    sudo mkfs.ext4 /dev/sdb1
    ```
3. `Mount a partition:`
    ```
    sudo mount /dev/sdb1 /mnt/newdisk
    ```
4. `Automate the mount using /etc/fstab:`
    ```
    /dev/sdb1   /mnt/newdisk   ext4   defaults   0   0
    ```
# Important Notes
`Backup your data:` Always back up your data before making changes to your partitions or file systems.
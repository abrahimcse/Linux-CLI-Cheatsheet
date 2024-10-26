## LVM (Logical Volume Manager)

To extend your existing storage using LVM (Logical Volume Manager) when you `add a new hard disk`, you'll need to follow a series of steps. This includes creating `physical volumes`, adding them to a `volume group`, and then `extending your logical volume`. Here's a detailed step-by-step guide for easy implementation:

**Step 1: Verify the New Hard Disk**
1. `List All Available Disks:`
```
sudo fdisk -l
```
This command will show all the disks attached to your system. Identify the new hard disk `(e.g., /dev/sdb)`.

2. `Check Existing LVM Setup:`
    - i. Display Physical Volumes
        ```
        sudo pvs
        ```
    - ii. Display volume groups
        ```
         sudo vgs
        ```
    - iii. Display logical volumes
        ```
        sudo lvs
       ```
**Step 2: Create a Physical Volume (PV) on the New Hard Disk**

1. `Partition the New Disk (optional but recommended):`

    - Start the `fdisk` tool to create a new partition:
        ```
        sudo fdisk /dev/sdb
        ```
    - Follow these steps within `fdisk`:
        - Type `n` to create a new partition.
        - Select `p` for a primary partition.
        - Press `Enter` to accept default values for the partition number, first sector, and last sector.
        - Type `t` to change the partition type.
        - Enter `8e` to set the type to Linux LVM.
        - Type `w` to write the changes and exit.
2. `Create the Physical Volume:`
    ```
    sudo pvcreate /dev/sdb1
    ```
This command initializes the new partition as a physical volume for LVM.

**Step 3: Add the Physical Volume to an Existing Volume Group (VG)**
1. `Add the Physical Volume to Your Volume Group:`
    ```
    sudo vgextend your_volume_group_name /dev/sdb1
    ```
Replace `your_volume_group_name` with the name of your existing volume group (e.g., `vg_data`).

**Step 4: Extend the Logical Volume (LV)**
1. `Find the Name of the Logical Volume:`
    ```
    sudo lvdisplay
    ```
2. `Extend the Logical Volume:`
    ```
    sudo lvextend -L +50G /dev/your_volume_group_name/your_logical_volume_name
    ```
3. `Resize the File System on the Logical Volume:`

    - If you are using an `ext4` file system:
        ```
        sudo resize2fs /dev/your_volume_group_name/your_logical_volume_name
        ```
    - For an `XFS` file system:
        ```
        sudo xfs_growfs /dev/your_volume_group_name/your_logical_volume_name
        ```
**Step 5: Verify the Changes**
1. `Check the Logical Volume Size:`
    ```
    sudo lvdisplay
    ```
2. `Verify the File System Size:`
    ```
    df -h
    ```

## Example Commands (Summary)
1. `Identify the new disk:`
    ```
    sudo fdisk -l
    ```
2. `Partition the new disk:`
    ```
    sudo fdisk /dev/sdb
    ```
3. `Create a physical volume:`
    ```
    sudo pvcreate /dev/sdb1
    ```
4. `Extend the volume group:`
    ```
    sudo vgextend your_volume_group_name /dev/sdb1
    ```
5. `Extend the logical volume:`
    ```
    sudo lvextend -L +50G /dev/your_volume_group_name/your_logical_volume_name
    ```
6. `Resize the file system:`
    - For `ext4`:
    ```
    sudo resize2fs /dev/your_volume_group_name/your_logical_volume_name
    ```
    - For `XFS`:
    ```
    sudo xfs_growfs /dev/your_volume_group_name/your_logical_volume_name
    ```

# Important Notes
`Backup your data:` 
Before making any changes to your disk or logical volumes, it's always a good idea to back up your important data.

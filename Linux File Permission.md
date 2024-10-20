## Linux File Permissions

In Linux, file permissions are critical for controlling access to files and directories. Each file and directory has associated permissions that dictate who can read, write, or execute the file. Here’s a detailed overview of Linux file permissions, including how to view and modify them.

**Understanding Linux File Permissions**


1.   `Permission Types:`
     - `Read (r):` Permission to read the contents of the file. For directories, this allows listing files.
     -   `Write (w):` Permission to modify the contents of the file. For directories, this      allows creating or deleting files within the directory.
     -   `Execute (x):` Permission to execute the file as a program. For directories, this allows accessing files within the directory.

2. `User Classes:`

     - `Owner (u):` The user who owns the file.
     - `Group (g):` A group of users who have been granted access to the file.
     -  `Others (o):` All other users who are not the owner or part of the group.

3. `Permission Representation:`

    - File permissions are displayed in a 10-character string when using the `ls -l` command. The format is:

    ```-rwxrwxrwx```

     - The first character indicates the file type (e.g., - for a regular file, d for a directory).
     - The next nine characters are divided into three sets of three:
          - First set (e.g., `rwx`): Owner permissions
          - Second set (e.g., `rwx`): Group permissions
          - Third set (e.g., `rwx`): Others permissions

For example, `-rwxr--r--` means:

- The file is a regular file (`-`).
- The owner has read, write, and execute permissions (`rwx`).
- The group has read permission (`r--`).
- Others have read permission (`r--`).

**Viewing Permissions**

To view file permissions in Linux, use the `ls` command:
```
ls -l filename
```

**Modifying Permissions**

You can change file permissions using the chmod command. There are two ways to specify permissions: symbolic mode and numeric mode.

1. `Symbolic Mode`

In symbolic mode, you use the following syntax:
```
chmod [u/g/o][+/-][r/w/x] filename
```

- `u` : user (owner)
- `g` : group
- `o` : others
- `+` : add permission
- `-` : remove permission
- `r` : read
- `w` : write
- `x` : execute

`Examples:`

- `Add execute permission for the owner:`
```
chmod u+x filename
```
- `Remove read permission for others:`
```
chmod o-r filename
```
- `Add write permission for the group:`
```
chmod g+w filename
```
2. `Numeric Mode`

In numeric mode, permissions are represented by a three-digit octal number. Each permission type is represented by a specific number:

- Read (`r`) = `4`
- Write (`w`) = `2`
Execute (`x`) = `1`

`To set permissions, add the numbers together:`

- `7` = read, write, execute (`4+2+1`)
- `6` = read, write (`4+2`)
- `5` = read, execute (`4+1`)
- `4` = read (`4`)
- `3` = write, execute (`2+1`)
- `2` = write (`2`)
- `1` = execute (`1`)
- `0` = no permissions

`Examples:`

- `Set permissions to read, write, and execute for the owner, and read and execute for group and others:`
```
chmod 755 filename
```

- `Set permissions to read and write for the owner, and no permissions for group and others:`

```
chmod 600 filename
```
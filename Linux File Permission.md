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




# Structure of Linux FileSystem
```
	- / (root)
	- / bin / sbin
	- / etc
	- / home
	- / var
	- / usr
	- / opt
	- / dev
	- / tmp
```

## / (Root)
-  Top - level directory for the entire filesystem.
- All of the other directories in FHS are in the root directory.
- The root directory is owned by the root user, who has complete control over .
- Only *ROOT*  user has write privilege under this directory.

## /bin (binaries)
 - Contains essential binary executables that are needed during the boot process or in single-user mode.
 - Generally accessible to all user.
 - *ls* : lists files and directories
 - *cat* : concatenates and displays files
 - *cp* : copies files and directories
 - *mv* : moves or renames files and directories
 - *rm* : removes files and directories

## /sbin (system binaries)
- contains binary executables primarily used for system administration.
- Usually require root privileges to execute, since they involve system-level changes.
- *fdisk* : Partitions hard disks
- *fsck* : Checks and repairs file systems
- *init* : Initializes the system
- *reboot* : Reboots the system 
- *Shutdown*  : Shuts down the system



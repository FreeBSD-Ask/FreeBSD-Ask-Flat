# 8.6 Installing Software from DVD

## Mounting the DVD

Before installing software using a DVD, you must first mount the DVD to the system. There are two mounting methods, applicable to local ISO files and physical DVD devices respectively.

To mount an existing file system image, use `mdconfig` with the specified name of the ISO file and an available unit number. Then, reference that unit number to mount it to an existing mount point. After mounting, the files in the ISO file will appear in the mount point.

This example attaches **FreeBSD-14.2-RELEASE-amd64-dvd1.iso** to the memory device **/dev/md0**, then mounts that memory device to **/dist**:

### Directly Mounting a Local ISO

```sh
# mdconfig -f /home/ykla/FreeBSD-14.2-RELEASE-amd64-dvd1.iso  # Replace with the actual ISO path; you can use pwd to check the current path
md0
# mkdir -p /dist  # Create the mount path; it must be this path
# mount -t cd9660 /dev/md0 /dist # Cannot mount ISO directly; will report error block device required
```

Note that `-t cd9660` is used for mounting ISO format.

### Directly Using the DVD Device (e.g., ISO Image Mounted via Virtual Machine)

Observe the ISO image mounting status:

```sh
# gpart show  # Display all disk partition table information in the system

……useless disks omitted……

=>      9  2356635  cd0  MBR  (4.5G)
        9  2356635       - free -  (4.5G)
```

You can see that `cd0` exists, and the size matches expectations.

```sh
# mkdir -p /dist # Create mount point
# mount -t cd9660 /dev/cd0 /dist # Mount the ISO
# ls /dist/ # View mount status
.cshrc		bin		lib		net		root		var
.profile	boot		libexec		packages	sbin
.rr_moved	dev		media		proc		tmp
COPYRIGHT	etc		mnt		rescue		usr
```

### Troubleshooting and Unfinished Business

Because the path in `packages/repos/FreeBSD_install_cdrom.conf` is a fixed value and cannot be modified, if you change the directory **/dist** to another path, the environment variable method will not work.

## Installing DVD Software Directly Using Environment Variables

Have pkg use the specified software repository path to install Xorg:

```sh
# env REPOS_DIR=/dist/packages/repos pkg install xorg
Updating FreeBSD_install_cdrom repository catalogue...
FreeBSD_install_cdrom repository is up to date.
All repositories are up to date.
Checking integrity... done (0 conflicting)
The following 1 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
	xorg: 7.7_3

Number of packages to be installed: 1

Proceed with this action? [y/N]:
```

To list available software on the DVD:

```sh
# env REPOS_DIR=/dist/packages/repos pkg rquery "%n"
```

## Switching the Software Repository to DVD

### Creating a DVD Software Repository

Copy `FreeBSD_install_cdrom.conf` to the **/etc/pkg/** directory:

```sh
# cp /dist/packages/repos/FreeBSD_install_cdrom.conf /etc/pkg/
```

### Test Installation

Install the Xorg graphical system:

```sh
# pkg install xorg
Updating FreeBSD_install_cdrom repository catalogue...
FreeBSD_install_cdrom repository is up to date.
All repositories are up to date.
Checking integrity... done (0 conflicting)
The following 1 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
	xorg: 7.7_3

Number of packages to be installed: 1

Proceed with this action? [y/N]:
```

## Ejecting the DVD and Releasing Resources

When the memory disk is no longer in use, its resources should be released back to the system. First, unmount the file system, then use `mdconfig` to detach the disk from the system and release its resources. Continuing this example:

```sh
# umount /dist
# mdconfig -d -u 0
```

To determine whether there are still memory disks attached to the system, enter the `mdconfig -l` command.

## References

- FreeBSD Project. HOWTO: Install binary package without internet access[EB/OL]. [2026-03-25]. <https://forums.freebsd.org/threads/howto-install-binary-package-without-internet-acces.60723/>. Method for installing binary packages via DVD in a network-free environment.
- FreeBSD Project. pkg(8) -- package manager[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=pkg&sektion=8>. FreeBSD package manager manual page.

## Exercises

1. Read the source code of `bsdconfig`, locate the cause of the `No pkg(8) database found!` error, and try to fix the issue so that it can properly use the DVD to install software.

2. Analyze the design where the path is hardcoded as **/dist** in the DVD installation method, and refactor this mechanism to support custom paths.

3. Modify pkg's repository configuration mechanism to support any directory on the local file system as a software source, and verify its usability in an offline environment.

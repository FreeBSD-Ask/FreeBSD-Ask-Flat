# 8.3 Managing Binary Packages with pkg

FreeBSD's current binary package manager is pkg (formerly pkgng), named after the English word "Package," which is an abbreviation for software package.

## Installing the pkg Package Manager

> **Tip**
>
> To avoid backward compatibility issues, the pkg(8) tool is not pre-installed in the base system.

The base system does not include pkg by default; you need to download and install pkg first:

```sh
# pkg # Type pkg and press Enter
The package management tool is not yet installed on your system. # pkg is not yet installed
Do you want to fetch and install it now? [y/N]: y # Type y and press Enter to confirm installation
Bootstrapping pkg from pkg+https://pkg.FreeBSD.org/FreeBSD:14:amd64/quarterly, please wait... # Note here, the default is the quarterly branch repository
Verifying signature with trusted certificate pkg.freebsd.org.2013102301... done
Installing pkg-1.21.3...
Extracting pkg-1.21.3: 100%
pkg: not enough arguments # This error indicates missing arguments, but it's just for installing pkg itself and can be ignored
Usage: pkg [-v] [-d] [-l] [-N] [-j <jail name or id>|-c <chroot path>|-r <rootdir>] [-C <configuration file>] [-R <repo config dir>] [-o var=value] [-4|-6] <command> [<args>]

For more information on available commands and options see 'pkg help'.
```

> **Tip**
>
> If it stays at `Bootstrapping pkg from ……, please wait...` for a long time, press **Ctrl + C** to interrupt the process, switch to a domestic repository, and try again.

> **Tip**
>
> If you see the message `00206176BC680000:error:0A000086:SSL routines:tls_post_process_server_certificate:certificate verify failed:/usr/src/crypto/openssl/ssl/statem/statem_clnt.c:1890:` (SSL certificate verification failed), please calibrate the system time first.

> ```sh
> # ntpd -q -g pool.ntp.org # Synchronize system time using pool.ntp.org
> ```
>
>> **Discussion Question**
>>
>> In the context of widespread SSL usage, any network issue always requires checking whether the local system time is correct. Users often overlook this (sometimes it is even caused by a damaged encryption module in the CPU), and in most cases, the error messages are extremely unclear. Please think about how to solve this problem.

## Installing Software with pkg

> **Tip**
>
> If you need to check the specific status of a software package in FreeBSD, you can search for "freebsd ports package_name" using a search engine, or look it up directly on [FreshPorts](https://www.freshports.org/).

Using the installation of Chromium as an example:

```sh
$ pkg ins chromium # Install the chromium browser with regular user privileges
pkg: Insufficient privileges to install packages
```

"Insufficient privileges to install packages" means "insufficient privileges, unable to install packages."

Try again:

```sh
$ su # Escalate privileges to root; this requires the regular user to be in the wheel group
Password: # Enter the root account password here!
# pkg ins chromium # Install chromium again
Updating FreeBSD repository catalogue...
Fetching data.pkg: 100%   10 MiB 768.6kB/s    00:14
Processing entries: 100%
FreeBSD repository update completed. 36822 packages processed.
Updating FreeBSD-kmods repository catalogue...
Fetching data.pkg: 100%   31 KiB  32.3kB/s    00:01
Processing entries: 100%
FreeBSD-kmods repository update completed. 213 packages processed.
All repositories are up to date.
The following 6 package(s) will be affected (of 0 checked): # 6 packages will be affected

New packages to be INSTALLED:
        chromium: 142.0.7444.162 [FreeBSD]
        dconf: 0.49.0 [FreeBSD]
        harfbuzz-icu: 10.3.0 [FreeBSD]
        jsoncpp: 1.9.6_1 [FreeBSD]
        sndio: 1.10.0_1 [FreeBSD]
        speex: 1.2.1_1,1 [FreeBSD]

Number of packages to be installed: 6

The process will require 463 MiB more space.
127 MiB to be downloaded.

Proceed with this action? [y/N]: # Type y here and press Enter to install
```

> **Discussion Question**
>
>> [Add Concurrent Downloads of Multiple Packages](https://github.com/freebsd/pkg/issues/1628)
>
> It can be seen that pkg supports neither parallel downloads nor parallel installations. Read the source code, try to solve this problem, and submit a PR.

You may encounter this situation:

```sh
# pkg ins chromium  # Install the Chromium browser
Updating FreeBSD repository catalogue.
Fetching meta.conf: 100%    179 B   0.2kB/s    00:01
Fetching data.pkg: 100%   10 MiB   2.7MB/s    00:04
Processing entries: 100%
FreeBSD repository update completed. 36804 packages processed.
Updating FreeBSD-kmods repository catalogue...
FreeBSD-kmods repository is up to date.
All repositories are up to date.
pkg: No packages available to install matching 'chromium' have been found in the repositories
```

"pkg: No packages available to install matching 'chromium' have been found in the repositories" means "pkg: No packages matching chromium were found in the repositories for installation."

If the preceding output showed "FreeBSD repository update completed. 36804 packages processed." (FreeBSD repository update completed. 36804 packages processed), it means the current software repository is available; it just cannot find the `chromium` package.

This is a manifestation of the "atomic update" deficiency described below.

Additionally, even if the system has i18n configured, pkg's output remains in English.

> **Discussion Question**
>
>> [Is it possible to add i18n multilingual support using po files?](https://github.com/freebsd/pkg/issues/2421)
>>
>> The FreeBSD base system does not have gettext, so there are no plans to do this. If a usable libintl suite becomes available in pkg in the future, this may be reconsidered.
>
> Read the pkg source code, locate the source of the problem, try to solve it, and submit a PR to enable i18n support in pkg.

## Updating Software with pkg

```sh
# pkg upgrade
```

If the error appears: `You must upgrade the ports-mgmt/pkg port first` (You must update pkg itself first).

Solution (prefer using pkg's own upgrade, or compile via Ports):

```sh
# pkg bootstrap -f      # Force reinstall pkg from the remote repository without going through Ports
```

Or:

```sh
# cd /usr/ports/ports-mgmt/pkg
# make deinstall
# make install
```

## Viewing All Installed Software

```sh
# pkg info
```

## Uninstalling Software

`pkg delete` automatically adds packages with unsatisfied dependencies to the deletion list by default and will not break dependency relationships; it only skips dependency checks when the `-f` parameter is used. If you need to clean up "leaf" packages that are no longer depended on by other packages, you can install `pkg_rmleaves` or use the built-in command `pkg autoremove` to remove automatically installed packages that no longer have dependents.

```sh
# pkg install pkg_rmleaves
```

Or

```sh
# cd /usr/ports/ports-mgmt/pkg_rmleaves/
# make install
```

### How to Uninstall All Self-Installed Third-Party Software?

```sh
# pkg delete -fa # If the f parameter is included, the pkg package manager itself will also be uninstalled, since pkg is also software that the user initially installed themselves
Checking integrity... done (0 conflicting)
Deinstallation has been requested for the following 87 packages (of 0 packages in the universe):

Installed packages to be REMOVED:
	alsa-lib: 1.2.12
	brotli: 1.1.0,1
	curl: 9.8.0
……some omitted……
	pcre2: 10.43
	perl5: 5.36.3_1
	pkg: 1.21.3   # If the -f parameter is included, pkg itself will also be deleted, since pkg is also software that the user initially installed
	png: 1.6.43
	xorg-fonts-truetype: 7.7_1
	xorgproto: 2024.1
	zstd: 1.5.6

Number of packages to be removed: 87

The operation will free 825 MiB.

Proceed with deinstalling packages? [y/N]: # Type y and press Enter to complete the uninstallation
```

#### References

- FreeBSD Project. pkg delete -- deletes packages from the database and the system[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=pkg-delete&sektion=8>. Provides detailed specifications and parameter descriptions for the pkg command to delete software packages.
- FreeBSD Project. pkg(7) -- a utility for manipulating packages[EB/OL]. [2026-04-17]. <https://man.freebsd.org/cgi/man.cgi?query=pkg&sektion=7>. FreeBSD package management tool manual page.

## Listing Files Installed by a pkg Package

> **Tip**
>
> The download path for pkg is **/var/cache/pkg/**.

```sh
/
└── var/
    └── cache/
        └── pkg/ # Download path for pkg
```

> **Note**
>
> You can only list files of installed software packages; this command cannot be used for packages that are not installed.

```sh
# pkg info -l xrdp
xrdp-0.10.2_2,1:
	/usr/local/bin/xrdp-dis
	/usr/local/bin/xrdp-dumpfv1
	/usr/local/bin/xrdp-genkeymap
	/usr/local/bin/xrdp-keygen
	/usr/local/bin/xrdp-sesadmin
	/usr/local/bin/xrdp-sesrun
	/usr/local/etc/pam.d/xrdp-sesman
	/usr/local/etc/rc.d/xrdp
	……some omitted……
```

## Finding Missing `.so` Files (Applicable to Linux Compatibility Layer)

> **Warning**
>
> This section only addresses the issue of missing `.so` files in the Linux compatibility layer. If you encounter such issues on FreeBSD, you should first update the system, then update the software repositories and software.

### Installing pkg-provides

```sh
# pkg install pkg-provides
```

Or:

```sh
# cd /usr/ports/ports-mgmt/pkg-provides/
# make install clean
```

### Configuring pkg-provides

- View configuration instructions:

```sh
# pkg info -D pkg-provides
```

Directory structure:

```sh
/usr/local/
├── etc/
│   └── pkg.conf # pkg configuration file
└── lib/
    └── pkg/ # pkg plugin directory
```

- Edit the **/usr/local/etc/pkg.conf** file, find the empty line, and write:

```ini
PKG_PLUGINS_DIR = "/usr/local/lib/pkg/";
PKG_ENABLE_PLUGINS = true;
PLUGINS [ provides ];
```

- Run `pkg plugins`:

```sh
# pkg plugins
NAME       DESC                                          VERSION
provides   A plugin for querying which package provides a particular file 0.7.4
```

- Refresh the database:

```sh
# pkg provides -u
Fetching provides database: 100%   19 MiB   6.6MB/s    00:03
Extracting database....success
```

### Example: Finding `libxcb-icccm.so.4`

```sh
# pkg provides libxcb-icccm.so.4
Name    : xcb-util-wm-0.4.2
Comment : Framework for window manager implementation
Repo    : FreeBSD
Filename: usr/local/lib/libxcb-icccm.so.4.0.0
          usr/local/lib/libxcb-icccm.so.4
```

## Troubleshooting and Unfinished Business

### Unable to Find Packages Mentioned in This Book in pkg

When using pkg to install software packages explicitly mentioned in the textbook and receiving a prompt that they do not exist, the cause can generally be attributed to the following two situations:

- Situation 1: The Port does not actually exist in Ports. Possible reasons include errors in the textbook, the Port has been removed from the Ports collection, or it has been renamed.
- Situation 2: The Port does exist in Ports, but FreeBSD's pkg packages are built periodically (synchronized with Ports' own updates), so there is often a temporary lack of corresponding pkg binary packages.

For specific reasons, it is recommended to check <https://www.freshports.org/>, which displays the dependency status and pkg package build status of software packages.

This book generally also lists the Ports installation method. For example, to look up Port **x11/budgie**, the procedure is as follows: directly visit <https://www.freshports.org/x11/budgie/>.

If the Port exists in Ports but there is temporarily no pkg package, waiting 7–14 days is usually sufficient (the system automatically reports build failures to the maintainer). If you need to install and use it immediately, please use Ports.

### `ld-elf.so.1: Shared object "libmd.so.6" not found, required by "pkg"`

This problem is usually caused by the software repository not being synchronized with changes to the base system ABI in a timely manner.

For general RELEASE, updating the system is sufficient. For CURRENT/STABLE systems, recompiling `pkg` is sufficient.

#### RELEASE

First switch to the latest repository, then reinstall pkg using the pkg package from the repository:

```sh
# pkg-static bootstrap -f  # Force initialize the pkg package manager
```

If ineffective, then:

```sh
# freebsd-update fetch        # Download available FreeBSD updates
# freebsd-update install      # Install downloaded FreeBSD updates
# pkg-static update -f        # Force update local package repository index
# pkg-static upgrade -f pkg   # Force upgrade the pkg tool itself
```

#### CURRENT/STABLE

```sh
# pkg-static delete -f pkg # Force uninstall the current pkg
# cd /usr/ports/ports-mgmt/pkg # Switch directory
# make BATCH=yes install clean # Reinstall pkg using Ports
```

### `pw: user 'package' disappeared during update`

Problem example:

```sh
[1/1] Installing package…
===> Creating groups.
Creating group 'package' with gid '000'.
===> Creating users
Creating user 'package' with uid '000'.
pw: user 'package' disappeared during update
pkg: PRE-INSTALL script failed
```

The cause is that the user database is out of sync.

Update the password database according to **/etc/master.passwd**:

```sh
# /usr/sbin/pwd_mkdb -p /etc/master.passwd
```

### `Shared object "x.so.x" not found, required by "xxx"`

This problem usually occurs because the ABI has been broken; updating will resolve it.

Install `bsdadminscripts2` using pkg:

```sh
# pkg install bsdadminscripts2
```

Or install `bsdadminscripts2` using Ports:

```sh
# cd /usr/ports/ports-mgmt/bsdadminscripts2/
# make install clean
```

Check whether the dynamic library dependencies of installed software packages are complete:

```sh
# pkg_libchk
doxygen-1.9.6_1,2: /usr/local/bin/doxygen misses libmd.so.6
jbig2dec-0.20_1: /usr/local/bin/jbig2dec misses libmd.so.6
jbig2dec-0.20_1: /usr/local/lib/libjbig2dec.so misses libmd.so.6
```

Recompile each package in the above list using Ports (RELEASE can update directly with `pkg`).

#### Appendix: Extended Usage and References for `bsdadminscripts2`

- lonkamikaze. BSD Administration Scripts II[EB/OL]. [2026-03-25]. <https://github.com/lonkamikaze/bsda2>. Provides FreeBSD system administration auxiliary toolset, including package integrity check and other functions.

If PkgBase is used, `bsdadminscripts2` can **check system integrity** and find which system files have been tampered with.

Verify the integrity and consistency of installed software packages:

```sh
# pkg_validate
FreeBSD-pkg-bootstrap-15.snap20241004232339: checksum mismatch for /etc/pkg/FreeBSD.conf
FreeBSD-runtime-15.snap20241004232339: checksum mismatch for /etc/group
FreeBSD-runtime-15.snap20241004232339: checksum mismatch for /etc/master.passwd
```

- `bsdadminscripts2` can also find outdated software on the current system:

```sh
# pkg_version -ql\<
akonadi-23.08.5_1
build2-0.17.0
chromium-128.0.6613.137
```

### `Newer FreeBSD version for package pkg`

Problem example:

```sh
Newer FreeBSD version for package pkg:
To ignore this error set IGNORE_OSVERSION=yes
- package: 1402843
- running kernel: 1400042
Ignore the mismatch and continue? [y/N]:
```

This usually occurs on systems that have lost security support or on CURRENT/STABLE branch systems. It does not affect usage; just type `y`.

To resolve this at the root, you need to uninstall pkg and install `ports-mgmt/pkg` from Ports; or update the entire system from source code.

If you only need to suppress this prompt, simply write `IGNORE_OSVERSION=yes` to the **/etc/make.conf** file as instructed (create the file if it does not exist).

### `pkg: An error occurred while fetching package: No error`

Execute `certctl rehash` with root privileges to refresh certificates.

See: pkg(8): "An error occurred while fetching package: No error"[EB/OL]. [2026-03-26]. <https://forums.freebsd.org/threads/pkg-8-an-error-occured-while-fetching-package-no-error.96761/>.

## Appendix: Analysis of the Difficulties and Current Status of FreeBSD Package Atomic Updates

FreeBSD mirror sites' software repositories (whether official or unofficial) exhibit the following typical phenomena:

- Once a Port is updated, the pkg binary package derived from that Port is immediately removed from the software repository until a new pkg package is built, rather than keeping the old version of the package;
- As soon as a new build begins, the old version package is immediately deleted from the pkg software repository until a new version of the pkg package is built.

A feasible solution is to lock software packages at a fixed version stage (such as the quarterly branch), temporarily not updating, and directly rotating versions.

The problem is that Port updates are irregular, and complex dependency relationships may trigger chain issues. Those with the ability can try to propose new perspectives and suggestions, and provide feedback below or on the [FreeBSD Forum](https://forums.freebsd.org/).

> **Discussion Question**
>
> - Related discussion [the disappearing pkg issue](https://www.reddit.com/r/freebsd/comments/1nlnwtd/the_disappearing_pkg_issue/)
> - The pkg project is located at [freebsd/pkg](https://github.com/freebsd/pkg)
> - The pkg package build system is located at [Poudriere](https://github.com/freebsd/poudriere)
>
> Try: Help the FreeBSD Project implement atomic updates for pkg binary packages?

## Exercises

1. Read the pkg source code, locate the implementation of parallel downloads and installations, try to add parallel download functionality, and verify whether it significantly improves installation speed.

2. Select the atomic update issue of pkg, design an experimental plan, reproduce the problem, and propose a feasible solution.

3. Add i18n support to pkg.

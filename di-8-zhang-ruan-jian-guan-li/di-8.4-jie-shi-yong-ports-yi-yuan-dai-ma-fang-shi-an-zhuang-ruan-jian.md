# 8.4 Installing Software from Source Code Using Ports

Ports is FreeBSD's framework for building software from source code, suitable for scenarios requiring custom compilation options, patching, or installing software not included in the pkg repository.

## Obtaining Ports

> **Note**
>
> `portsnap` has been removed from the base system since FreeBSD 14.0 and is no longer supported in FreeBSD 15.0. Users migrating from older tutorials should use the Git method described below to obtain Ports.

Using an archive can avoid the "chicken-and-egg" problem (e.g., needing to install Git, but the system has neither Ports nor the desire to use pkg).

### Using the Ports Archive

You can download the Ports archive from multiple mirror sources. Below are several commonly used source addresses.

- NJU:

```sh
# fetch https://mirrors.nju.edu.cn/freebsd-ports/ports.tar.gz
```

- Or USTC

```sh
# fetch https://mirrors.ustc.edu.cn/freebsd-ports/ports.tar.gz
```

- Or FreeBSD official

```sh
# fetch https://download.freebsd.org/ftp/ports/ports/ports.tar.gz
```

#### Extracting the Ports Archive

After downloading, you need to extract the archive to the specified location.

```sh
# tar -zxvf ports.tar.gz -C /usr/ # Extract to the /usr/ directory
# rm ports.tar.gz # Delete the archive
```

### Obtaining Ports Using Git

Git is the recommended way to obtain the Ports source code, making it convenient to manage versions and updates.

#### Installing Git

First, you need to install the Git tool so that you can pull the source code.

Install using pkg:

```sh
# pkg install git
```

#### Shallow Clone of the Ports Repository (USTC)

The University of Science and Technology of China provides a mirror source for FreeBSD Ports, and you can quickly obtain the code using a shallow clone.

```sh
# git clone --filter=tree:0 https://mirrors.ustc.edu.cn/freebsd-ports/ports.git /usr/ports
```

> **Note**
>
> `--depth 1` (only pulling the latest log and commit records) puts significant computational pressure on the server. Please try to use the `--filter=tree:0` parameter for cloning.

#### Shallow Clone of the Ports Repository (FreeBSD Official)

You can also obtain the source code directly from the FreeBSD official repository.

```sh
# git clone --filter=tree:0 https://git.FreeBSD.org/ports.git /usr/ports
```

#### Full Clone of the Ports Repository (FreeBSD Official) with Specified Branch

If you need the complete commit history and all branches, you can perform a full clone.

```sh
# git clone https://git.FreeBSD.org/ports.git /usr/ports
```

For detailed explanations of the quarterly branch versus the latest (main) branch, see other relevant chapters. As needed, you can switch to a specific branch, such as a quarterly branch.

Switch to the `2025Q1` branch:

```sh
# git switch 2025Q1
Updating files: 100% (14323/14323), done.
Branch '2025Q1' set up to track 'origin/2025Q1'.
Switched to a new branch '2025Q1'
```

After switching, you can view local branches to confirm.

View local branches:

```sh
# git branch
* 2025Q1
  main
```

The Git branch has been successfully switched.

#### Synchronizing and Updating Ports Git

After obtaining the Ports source code, you need to periodically synchronize updates to get the latest changes.

```sh
# cd /usr/ports/ # Switch to the target directory
# git pull -p # Synchronize and update upstream Ports (-p/--prune is used to clean up remotely deleted ports)
```

If prompted that local changes exist, you can discard local changes before updating:

```sh
# git restore .
# git pull -p
```

#### Appendix: Invalid Certificate Due to Time Error

When using Git to pull code, you may encounter SSL certificate issues (error message like `SSL certificate problem: certificate is not yet valid`), commonly caused by incorrect system time. Use `ntpd -q -g pool.ntp.org` to synchronize the system time to resolve this. For detailed instructions, refer to other relevant chapters in this book.

## Using `whereis` to Query Software Paths

The `whereis` command can help quickly find the paths to a software's executable files, source code, and manual pages.

Find the paths to the python executable, source code, and manual pages:

```sh
# whereis python
```

The output will be:

```sh
python: /usr/ports/lang/python
```

## Viewing Software Package Dependencies

The method for viewing software package dependency relationships is as follows. You can view dependencies whether the software is installed or not.

When the software package is installed:

```sh
# pkg info -d screen
screen-4.9.0_6:
	indexinfo-0.3.1
```

When the software package is not installed:

```sh
root@ykla:/usr/ports/sysutils/htop # make all-depends-list
/usr/ports/ports-mgmt/pkg
/usr/ports/devel/pkgconf
……some omitted……
```

## How to Delete Configuration Files for the Current Port and Its Dependencies

If you need to clean up previously configured options, you can use the following command to delete the configuration files for the current Port and all its dependencies. This command recursively traverses the dependency tree, clearing the `make config` settings for each Port one by one, restoring them to default build parameters.

```sh
# make rmconfig-recursive
```

## How to Download All Required Software Packages at Once

To avoid interruptions during compilation due to network issues, you can download all required software packages at once. `fetch-recursive` recursively fetches the source code packages for the main Port and all its dependencies, combined with `BATCH=yes` to skip interactive options.

```sh
# make BATCH=yes fetch-recursive
```

## Software Compiled via Ports Can Also Be Packaged as pkg Packages

Software compiled and installed using Ports can also be packaged as pkg-format binary packages, facilitating deployment on other machines and avoiding repeated compilation. The packaged file is output to the current directory by default and can be copied to the target machine for installation with `pkg add`.

```sh
# pkg create nginx
```

## Updating FreeBSD Packages/Ports

Before updating, you need to synchronize and update Ports Git first.

Then list outdated Port software:

```sh
# pkg version -l '<'
chromium-127.0.6533.99             <
curl-8.9.1_1                       <
ffmpeg-6.1.2,1                     <
vlc-3.0.21_4,4                     <
w3m-0.5.3.20230718_1               <
```

### Installing portmaster

portmaster is a commonly used Ports update tool that can help manage and update installed software.

- Update installed Ports:

```sh
# cd /usr/ports/ports-mgmt/portmaster && make install clean	# Install portmaster
# portmaster -a # Automatically upgrade all software
# portmaster screen # Upgrade a single software
```

To skip interactive confirmation, you can use options similar to BATCH=yes such as `-a -G --no-confirm`:

```sh
# portmaster -a -G --no-confirm
```

### Viewing Port Dependency Relationships

Before updating software, you can first view the Port's dependency relationships to understand which software the update will affect.

```sh
# portmaster sysutils/htop  --show-work

===>>> Port directory: /usr/ports/sysutils/htop

===>>> Starting check for all dependencies
===>>> Gathering dependency list for sysutils/htop from ports

===>>> Installed devel/autoconf
===>>> Installed devel/automake
===>>> NOT INSTALLED		devel/libtool
===>>> NOT INSTALLED		devel/pkgconf
===>>> NOT INSTALLED		lang/python311
===>>> Installed ports-mgmt/pkg
```

### References

For more information, please refer to the following official documentation.

- FreeBSD Project. portmaster -- manage your ports without external databases or languages[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=portmaster&sektion=8>. Complete description of the Ports management tool that does not require external databases.

## Globally Setting Ports Build Options

The FreeBSD Ports framework supports globally controlling build options and dependencies in **/etc/make.conf**.

### How to Globally Disable MySQL

If you do not want to use MySQL-related options, you can disable them in the global configuration.

```sh
# echo "OPTIONS_UNSET+= MYSQL" >> /etc/make.conf
```

For the complete OPTIONS list, see <https://cgit.freebsd.org/ports/tree/Mk/bsd.port.mk>. For the complete DEFAULT_VERSIONS list, see <https://cgit.freebsd.org/ports/tree/Mk/bsd.default-versions.mk>.

## Appendix: Port Installation Example

### Installing python3

Now, using the installation of python3 as an example, we demonstrate how to compile and install software using Ports.

```sh
# cd /usr/ports/lang/python
# make BATCH=yes install clean
```

Where `BATCH=yes` (batch mode) means building with default parameters.

### How to Set All Required Dependencies

Before compiling software, you sometimes need to set the configuration options for all dependencies first.

```sh
# make config-recursive
```

### How to Use pkg to Install Dependencies

To save compilation time, you can use pkg to install required dependencies and only use Ports to compile the software package itself.

Do not use Ports to compile dependencies; only use Ports to compile the software package itself:

```sh
# make install-missing-packages
```

Using `chinese/fcitx` as an example:

```sh
# cd /usr/ports/chinese/fcitx
# make install-missing-packages
Updating FreeBSD repository catalogue...
FreeBSD repository is up to date.
Updating FreeBSD-base repository catalogue...
FreeBSD-base repository is up to date.
All repositories are up to date.
Updating database digests format: 100%
The following 2 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
	e2fsprogs-libuuid: 1.47.1 [FreeBSD]
	enchant2: 2.2.15_5 [FreeBSD]

Number of packages to be installed: 2

94 KiB to be downloaded.

Proceed with this action? [y/N]:
```

## Troubleshooting and Unfinished Business

### `autoconf-2.72 Invalid perl5 version 5.42.`

This can also be understood as a class of errors like "xxx-yy Invalid zz version aa."

Example: When installing openjdk21 using Ports, the following error occurs:

```sh
[root@Server /usr/ports/java/openjdk21]# make install clean
===> openjdk21-21.0.4+7.1 depends on executable: zip - found
===> openjdk21-21.0.4+7.1 depends on package: autoconf>0 - not found ①
===> autoconf-2.72 Invalid perl5 version 5.42. ②
*** Error code 1

Stop.
make[1]: stopped in /usr/ports/devel/autoconf
*** Error code 1

Stop.
make: stopped in /usr/ports/java/openjdk21
```

Observing the entire process, openjdk21 depends on autoconf, but autoconf is not installed on the system. So it recursively searches for autoconf's dependencies and finds that autoconf depends on perl5; combined with ②, we can see that perl5 is already on the system, but the error `Invalid version` indicates that the perl5 version does not meet the requirements.

This problem generally requires updating Ports first, then updating the perl5 version via `pkg install -f perl5` or `pkg upgrade` to resolve it.

#### References

- FreeBSD Forums. Invalid perl5 version 5.32[EB/OL]. [2026-03-25]. <https://forums.freebsd.org/threads/invalid-perl5-version-5-32.77628/>. The same problem as described above occurred.

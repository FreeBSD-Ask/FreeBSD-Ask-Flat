# 8.5 Ports Build Optimization

## FreeBSD Ports Multi-Threaded Compilation

To speed up compilation, you can configure multi-threaded compilation options to take advantage of multi-core processor performance.

Write the following content to the **/etc/make.conf** file; if it does not exist, `touch` to create the corresponding file.

```ini
FORCE_MAKE_JOBS=yes       # Force enable parallel compilation
MAKE_JOBS_NUMBER=4        # Set the number of parallel compilation jobs to 4
```

On Linux (such as Gentoo), `-jX` or `-j(X+1)` is generally used directly, where `X` is the number of cores.

`4` represents the number of parallel compilation jobs for the processor (usually corresponding to the number of cores or threads).

You can check the number of CPU cores detected by the system using the following command:

```sh
# sysctl kern.smp.cpus
kern.smp.cpus: 16
```

Or check the number of available CPU cores on the system:

```sh
# sysctl hw.ncpu
hw.ncpu: 16
```

The output value can be used as the value for `MAKE_JOBS_NUMBER`.

Search for the Intel processor model plus `ARK` to navigate to the Intel official website to check the thread count.

- FreeBSD Project. SMP -- symmetric multiprocessing kernel subsystem[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=SMP&sektion=4>. Documentation for kern.smp.cpus and hw.ncpu sysctl variables in the SMP subsystem.

## Setting /tmp as a Memory File System

To improve read and write speeds for temporary files, you can mount the **/tmp** directory as a memory file system tmpfs.

Edit the **/etc/fstab** file and write the following line:

```ini
tmpfs /tmp tmpfs rw 0 0
```

`reboot` to apply the changes.

### References

- FreeBSD Project. tmpfs -- in-memory file system[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=tmpfs&sektion=4>. Official technical specification for the in-memory file system tmpfs.

## Using ccache for Compilation Caching

ccache is a compilation caching tool that can accelerate the process of repeated compilation.

> **Warning**
>
> Using ccache may cause compilation failures. It is only effective during repeated compilation; the first compilation will not be accelerated and may even be slower. It is a space-for-time tradeoff.

ccache3 is a commonly used version.

### Installing ccache3

Install using pkg:

```sh
# pkg install ccache
```

- Install using Ports:

```sh
# cd /usr/ports/devel/ccache/
# make install clean
```

After installation, you can view the symlinks created by ccache:

```sh
# ls -al /usr/local/libexec/ccache
total 56
drwxr-xr-x   3 root wheel 15 Sep 20 02:02 .
drwxr-xr-x  18 root wheel 49 Sep 20 01:39 ..
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 CC -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 c++ -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 cc -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 clang -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 clang++ -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 clang++15 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 02:02 clang++18 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 clang15 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 02:02 clang18 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 cpp13 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 g++13 -> /usr/local/bin/ccache
lrwxr-xr-x   1 root wheel 21 Sep 20 00:29 gcc13 -> /usr/local/bin/ccache
drwxr-xr-x   2 root wheel 15 Sep 20 02:02 world
```

#### Configuring ccache3

Configure ccache to enable compilation caching:

- Modify the **/etc/make.conf** file and add the following line to enable ccache for accelerated compilation:

```ini
WITH_CCACHE_BUILD=yes
```

To prevent the cache from occupying too much disk space, it is recommended to set a maximum cache size limit.

- Set the ccache compilation cache maximum to 10 GB:

```sh
# ccache -M 10G
Set cache size limit to 10.0 GB
root@ykla:/usr/ports/www/chromium # ccache -s
cache directory                     /root/.ccache
primary config                      /root/.ccache/ccache.conf
secondary config      (readonly)    /usr/local/etc/ccache.conf
cache hit (direct)                     0
cache hit (preprocessed)               0
cache miss                             0
cache hit rate                      0.00 %
cleanups performed                     0
files in cache                         0
cache size                           0.0 kB
max cache size                      10.0 GB
```

After using it for a period of time, you can view ccache's statistics to understand the cache hit rate.

- Display ccache statistics after compiling with Ports for a while:

```sh
# ccache -s
cache directory                     /root/.ccache
primary config                      /root/.ccache/ccache.conf
secondary config      (readonly)    /usr/local/etc/ccache.conf
stats updated                       Fri Sep 20 02:05:35 2024
cache hit (direct)                    20
cache hit (preprocessed)              17
cache miss                           918
cache hit rate                      3.87 %
called for link                      121
called for preprocessing              26
compile failed                       115
preprocessor error                    66
bad compiler arguments                15
autoconf compile/link                523
no input file                         71
cleanups performed                     0
files in cache                      2305
cache size                           0.0 kB
max cache size                      10.0 GB
```

### Installing ccache4

ccache4 is a newer major version of ccache. The installation and configuration methods are the same as ccache3; only the package name is different.

Install using pkg:

```sh
# pkg install ccache4
```

Or install using Ports:

```sh
# cd /usr/ports/devel/ccache4/
# make install clean
```

#### Configuring ccache4

The symlinks and **/etc/make.conf** configuration are the same as ccache3 and will not be repeated. The output format of ccache4 is slightly different:

- Set the compilation cache maximum to 20 GB:

```sh
# ccache -M 20G
Set cache size limit to 20.0 GB
```

- After compiling with Ports for a while, view the compilation cache:

```sh
# ccache -s
Cacheable calls:   558 /  579 (96.37%)
  Hits:            110 /  558 (19.71%)
    Direct:        110 /  110 (100.0%)
    Preprocessed:    0 /  110 ( 0.00%)
  Misses:          448 /  558 (80.29%)
Uncacheable calls:  21 /  579 ( 3.63%)
Local storage:
  Cache size (GB): 0.0 / 20.0 ( 0.11%)
  Hits:            110 /  558 (19.71%)
  Misses:          448 /  558 (80.29%)
```

Display ccache4's current configuration parameters:

```sh
# ccache -p
(default) absolute_paths_in_stderr = false
(default) base_dir =
(default) cache_dir = /root/.cache/ccache
……some omitted……
```

### References

For more detailed information and usage of ccache, please refer to the following resources.

- FreeBSD Project. ccache-howto-freebsd.txt.in[EB/OL]. [2026-03-25]. <https://github.com/freebsd/freebsd-ports/blob/main/devel/ccache/files/ccache-howto-freebsd.txt.in>. Configuration guide for ccache in FreeBSD Ports, explaining how to enable cache acceleration during compilation.
- FreeBSD Project. ccache - a fast C/C++ compiler cache[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=ccache&sektion=1&n=1>.

## Multi-Threaded Downloading

To speed up the download of Ports source code, you can use multi-threaded download tools.

### axel

axel is a lightweight multi-threaded download tool that can significantly improve download speeds.

Install using pkg:

```sh
# pkg install axel
```

Or install using Ports:

```sh
# cd /usr/ports/ftp/axel/
# make install clean
```

After installation, you need to configure the Ports framework to use axel as the download tool.

Create or edit the **/etc/make.conf** file and write the following lines:

```ini
FETCH_CMD=axel                # Set axel as the download tool
FETCH_BEFORE_ARGS=-n 10 -a    # Set axel pre-download parameters: use 10 threads and show alternative progress bar
FETCH_AFTER_ARGS=              # Post-download command parameters are empty
DISABLE_SIZE=yes               # Disable file size check
```

### wget2

Install using Ports:

```sh
# cd /usr/ports/www/wget2/ && make install clean
```

Create or edit the **/etc/make.conf** file and write the following lines:

```ini
FETCH_CMD=wget2               # Set wget2 as the download tool
FETCH_BEFORE_ARGS=-c -t 3 --max-threads=16 # Set wget2 pre-download parameters
FETCH_AFTER_ARGS=             # Post-download command parameters are empty
DISABLE_SIZE=yes              # Disable file size check
```

wget2 parameter descriptions:

- `-c` Resume broken download
- `-t 3` Retry count 3
- `--max-threads=16` Set the maximum concurrent download thread count to 16, default is 5

> **Tip**
>
> Many servers do not support multiple threads downloading simultaneously. This puts significant pressure on the server and may trigger countermeasures, such as adding the downloading IP to a blacklist.

### References

- FreeBSD Project. ports -- contributed applications[EB/OL]. [2026-03-25]. <https://man.freebsd.org/cgi/man.cgi?query=ports&sektion=7>. Official documentation for the Ports framework, including FETCH_CMD and BATCH parameter descriptions.

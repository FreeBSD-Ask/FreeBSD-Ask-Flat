# 3.3 Migration Guide for macOS Users

macOS shares a large number of BSD components with FreeBSD, so migrating from macOS to FreeBSD has a lower barrier to entry for the command line compared to migrating from Windows.

## The Shared BSD Lineage

From a historical perspective, the core layer (Darwin) of macOS (and the derived iOS, iPadOS, etc.) is based on BSD code, integrated with other technologies.

```text
Original Unix
        |
        V
     4.3 BSD ------------------------------------------------------+
        |                                                          |
        +----------------------+                                   | (Based on 4.3 BSD)
        |                      |                                   V
        V                      V                           CMU Mach Project
   4.3BSD-Reno             386BSD 0.1                              |
        |                      |                                   V
        |                      V                                 Mach
        V                 FreeBSD 1.0                              |
   4.4BSD-Lite                 |                   +---------------+---------------+
                               |                   |                               |
                               V                   V                               V
                        FreeBSD 4.x/5.x        OSFMK 7.3                     NeXT Mach 2.5
                               |            (Based on Mach 3.0)              (Integrated 4.3 BSD)
                               |                   |                               |
                               |                   |                       +-------+
                               |                   |                       |       |
                               |                   |                       |       V
                               |                   |                       |    NeXTSTEP
                               |                   |                       |       |
                               |                   |                       |       V
                               |                   |                       |    OPENSTEP
                               |                   |                       |       |
                               |                   |                       |       V
                               |                   |                       |    Rhapsody
                               |                   |                       |       |
                               +---+               |                       |       |
                                   |               |                       |       |
                                   V               V                       V       |
                               +-----------------------------------------------+   |
                               |                   XNU Kernel                  |   |
                               |                                               |   |
                               |        (OSFMK 7.3 Based on Mach 3.0          |   |
                               |        + FreeBSD VFS/Networking)              |   |
                               +-----------------------------------------------+   |
                                                       |                           |
                                                       V                           |
                                                     Darwin                        |
                                       (XNU Kernel + FreeBSD Userland)             |
                                                       |                           |
                                                       +---------------------------+
                                                                     |
                                                                     V
                                                                   macOS
                                                      (Darwin + Aqua UI + Cocoa)
```

The macOS family of operating systems can be viewed as an independent, BSD-like operating system branch, on equal standing with systems such as OpenBSD, NetBSD, and FreeBSD.

| Component | Source |
| --------- | ------ |
| XNU Kernel | Based on Mach microkernel (CMU), NeXTStep/OpenStep architecture, integrating BSD subsystem (originally from 4.3BSD, integrated via NeXTSTEP, with Apple continuously syncing code from newer FreeBSD versions) |
| Network Stack | BSD/FreeBSD TCP/IP protocol stack as core (IPv4/IPv6), including IOKit driver interface; NKE (Network Kernel Extensions) |
| Virtual File System | Based on BSD VFS, supporting HFS+, APFS, UFS and other file systems |
| Userland Tools | BSD user tools (such as ls, cp, grep, etc.), modified and enhanced by Apple |
| Memory Management | Mach virtual memory management (VM), partially influenced by BSD memory management mechanisms, supporting paging, protection, and shared memory |
| Process Model | Mach IPC, Security Trailers, Mandatory Access Control (MAC) mechanisms, supporting multitasking and security policies |

## Basic Comparison

| Feature | macOS | FreeBSD | Notes |
| ------- | ----- | ------- | ----- |
| shell | zsh | sh | FreeBSD optionally supports zsh |
| Privilege escalation | sudo | Optional | FreeBSD optionally supports sudo |
| Software management (binary) | Optional, typically Homebrew | pkg | / |
| Software management (source-based) | MacPorts | Ports | MacPorts derives from Ports |
| Firewall | pf (Packet Filter) | Built-in pf (Packet Filter) and other firewalls | Optional support |
| Service management | launchd (command `launchctl`) | BSD init + RC system | / |
| Compiler | Clang + LLVM | Clang + LLVM | / |
| Lifecycle | 3 years | 4 years | / |
| File system | APFS (default) | ZFS (default) | / |
| UNIX certification | Certified per version | Not certified | / |

> **Note**
>
> Many command-line tools in macOS (such as `sed`, `grep`, `awk`) are older versions and differ from the current FreeBSD versions.

## APFS vs ZFS File System Comparison

| Feature | APFS | ZFS |
| ------- | ---- | --- |
| Checksums | Metadata only | Metadata + Data (end-to-end) |
| Snapshots | Supported | Supported |
| Copy-on-Write | Supported | Supported |
| Compression | Supports transparent compression (LZFSE, etc., requires manual enablement), not automatic real-time | Supported (LZ4, GZIP, ZSTD, etc.) |
| Encryption | Natively supported | Natively supported |
| Deduplication | Does not support global deduplication | Supported |
| Cloning | Supported | Supported |
| RAID | Not supported | RAID-Z / RAID-Z2 / RAID-Z3 |
| Space Sharing | Volume sharing within containers | Dataset sharing within storage pools |
| Maximum File Size | 8 EiB | 16 EiB |
| Maximum Pool/Container Size | 8 EiB | 2^128 bytes (theoretical addressing limit) |
| Case Sensitivity | Case-insensitive by default on macOS | Case-sensitive by default (adjustable via property) |

macOS's APFS is case-insensitive by default, while FreeBSD's ZFS and UFS are both case-sensitive by default.

## References

- Apple Inc. Darwin[EB/OL]. [2026-04-18]. <https://opensource.apple.com/>. Apple's open source Darwin operating system foundation, including the XNU kernel and BSD subsystem.
- LEVIN J. *Mac OS X and iOS Internals*[M]. Translated by Zheng Siyao, Fang Peici. Beijing: Tsinghua University Press, 2014. ISBN: 978-7-302-34867-2.
- Jason Perlow. Apple's Open Source Roots: The BSD Heritage Behind macOS and iOS[EB/OL]. (2024-07-08)[2026-03-26]. <https://thenewstack.io/apples-open-source-roots-the-bsd-heritage-behind-macos-and-ios/>. Comparison table referenced from here.
- Apple Inc. File System[EB/OL]. [2026-04-18]. <https://developer.apple.com/documentation/foundation/about-apple-file-system>. Official APFS technical guide.
- OpenZFS. OpenZFS Documentation[EB/OL]. [2026-04-18]. <https://openzfs.org/wiki/Main_Page>. OpenZFS project documentation.
